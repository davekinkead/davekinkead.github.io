---
layout: post
title: Content Addressable Storage - Implementing Git with Ruby
category: thoughts
date: 18 Oct 2015
---

As alluded to in [Part I](/thoughts/implementing-git-in-ruby/) of this series, the thing makes understanding Git so hard is that how it is used and explained is too detached from its underlying data model.  So the first step to understanding Git is to stop thinking about it as [VCS](https://en.wikipedia.org/wiki/Version_control) or [SCM](https://en.wikipedia.org/wiki/Software_configuration_management).  

True, Git is both those things (and more!) but the path to Git enlightenment requires forgetting all that for the time being.  For now, simply close your eyes, breath deeply and repeat:

> Git is Content Addressable Storage...  
> Git is Content Addressable Storage...  
> Git is Content Addressable Storage...  
> Git is Content Addressable Storage...  

[Content Addressable Storage](https://en.wikipedia.org/wiki/Content-addressable_storage)(CAS) is a way of finding stuff based on its content.  They way this is typically done is with the use of a [cryptographic hash](https://en.wikipedia.org/wiki/Cryptographic_hash_function) - a one way mathematical function that deterministically generates a unique output based on some input.  Give the function the same content and it will spit out the same unique ID every time.  Give it different content and you get a different ID.

So think of Git as a simple file store.  Give it a file and Git will save it to disk and spit out a 40 character ID for it.  Give it that ID and Git will return the file contents.  You can try this yourself in the terminal:


    $ echo "The Zen of Git" | git hash-object --stdin -w
    $ => 25ae764590fe9bfd6463672add5ab09156d7f1b8

    $ git cat-file -p 25ae764
    $ => The Zen of Git


There is a great deal of power in this simplicity.  Because storage is content addressable, file names are irrelevant.  The same content with multiple file names only needs to be stored once.  Git is also smart enough to lookup IDs optimistically - you only need the first 7 or so characters of the ID as long as these are unique.  At its heart, Git is just a simple key:value store.


## Objects 

There are four different object types in Git: _blobs_, _trees_, _commits_, and _tags_.  We'll cover each of these in more detail shortly but for now, it's useful to understand the hashing and storage process common to all.

Every object has an _ID_, a _header_, and _contents_.  The header is simply the object type, followed by a space, the content's size in bytes, and finally a null byte.  The file contents are compressed and stored in binary form.

> TYPE - SPACE - SIZE - NULL_BYTE   
> CONTENT


## Hashing Objects

To generate the ID, Git passes the header and contents to a [SHA1](https://en.wikipedia.org/wiki/SHA-1) hash function.  This can be done manually with `git hash-object`, or in our Ruby implementation as:


    require 'digest/sha1'

    module Git
      def self.hash(type, content)
        Digest::SHA1.hexdigest "#{header(type, content)}#{content}"
      end

      def self.header(type, content)
        "#{type.to_s} #{content.bytesize}\x00"
      end
    end


## Storing Objects

Git saves files in the _object store_ which by default is found in the `.git/objects` directory of your repo. The directory and file name are derived from the SHA1 hash with the first two characters forming the subdirectory and the remaining 38 becoming the filename.  Thus, the file that hashes to `25ae764590fe9bfd6463672add5ab09156d7f1b8` will be saved in `.git/objects/25/ae764590fe9bfd6463672add5ab09156d7f1b8`


    module Git
      def self.sha_to_path(sha)
        dir  = ".git/objects/#{sha[0..1]}"
        file = "#{dir}/#{sha[2..-1]}"
        [dir, file]        
      end
    end


To save space, git compresses the file header & contents.  We can implement this in Ruby using [Zlib](http://ruby-doc.org/stdlib-2.2.0/libdoc/zlib/rdoc/Zlib.html) from the standard library.


    require 'zlib'
    require 'fileutils'

    module Git
      def self.save(type, content)
        sha = hash type, content
        dir, file = sha_to_path sha

        unless File.exists? file
          FileUtils.mkdir_p dir
          File.open(file, 'w+') do |f| 
            f.write Zlib::Deflate.deflate(header(type, content) << content)
          end
        end

        sha
      end
    end


## Reading Objects

Reading objects is just the reverse of saving them.  Given a sha1 hash, we determine the path and filename, unzip the content, and then display it based on object type.  We'll look at the type specific content shortly.


    module Git
      def self.read(hash)
        dir, file = sha_to_path hash
        head, content = Zlib::Inflate.inflate(File.read file).split "\x00"
        type, size = head.split " "
        Git.send "read_#{type}", content
      end
    end


--- new post here

## The Blob

Blobs represent file contents are the most common object type in Git.  They are also the simplest to understand as the object contents are just the compressed object header and file contents.  As such, nothing else needs to be implemented and we can see if the code above works by comparing it against the actual git commands.


    Git.save :blob, "It's Git. In Ruby!"
    # => 83ca550b885011f19e7ee36fe840252f9e334f9d


If you try and read that file directly in text editor, you'll see a jumble of binary.  Instead, you can access it with the git command:


    $ git cat-file -p 83ca550b


To read a blob, all we need to do is unzip the contents and return them.


    module Git
      def self.read_blob(contents)
        contents
      end
    end


    Git.read "83ca550b885011f19e7ee36fe840252f9e334f9d"
    # => "It's Git. In Ruby!"


--- new post here

## The Tree

Blobs don't contain any metadata such as filename so this information needs to be stored somewhere else.  Enter the tree.  Trees are just a representation of a directory structure and contain metadata about _leaves_ - other trees and blobs.

IMG

This data includes the file name, object type, the object hash, and the object mode or permissions.  Git display it something like this.


    # 100644 blob 83ca550b885011f19e7ee36fe840252f9e334f9d    hello.txt
    # 040000 tree 1a80d3bf201288206919102ac3267f6414f8489d    posts


Here's where things get a little trickier though.  The format of a tree's contents that Git displays is different to how Git stores it.  The display format shown above is:

> MODE - SPACE - TYPE - SPACE - HASH - TAB - FILENAME - NEWLINE

But the internal storage format is:

> MODE - SPACE - FILENAME - NULL_BYTE - BINARY_OBJECT_HASH

Welcome to the frustrating inconsistencies of Git - mixing ASCII and binary all over the place!  The difference between the regular 40 char SHA1 hash that is displayed and the 20 byte version that is saved turns out to be pretty simple though.  In the 40 char hash, each 2 char pair represents a single byte hex value.  We can convert between the two with the following code.


    module Git
      def self.hex_to_bin(hash)
        hash.scan(/../).map { |x| x.hex.chr }.join
      end

      def self.bin_to_hex(hash)
        hash.unpack('H*').first
      end
    end

    Git.hex_to_bin "83ca550b885011f19e7ee36fe840252f9e334f9d"
    # => "\x83\xCAU\v\x88P\x11\xF1\x9E~\xE3o\xE8@%/\x9E3O\x9D"

    Git.bin_to_hex "\x83\xCAU\v\x88P\x11\xF1\x9E~\xE3o\xE8@%/\x9E3O\x9D"
    # => "83ca550b885011f19e7ee36fe840252f9e334f9d"


To build the tree...


    module Git
      def self.mktree(tree)

      end
    end


    index = File.read ".git/index"
    p index[0...4]
    p Git.bin_to_hex(index[4...8]).to_i
    p Git.bin_to_hex(index[8...12]).to_i
    p Git.bin_to_hex(index[12...18]).to_i
    p Git.bin_to_hex(index[-20..-1])
    p Digest::SHA1.hexdigest(index[0...-20])

A 12-byte header consisting of:
  4-byte signature:
  The signature is { 'D', 'I', 'R', 'C' } (stands for "dircache")
  4-byte version number:
  The current supported versions are 2, 3 and 4.
  32-bit number of index entries.
A number of sorted index entries.


160-bit SHA-1 over the content of the index file before this checksum.


- the commit

- sidenote the tag
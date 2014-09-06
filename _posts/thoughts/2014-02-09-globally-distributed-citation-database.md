---
title: Towards a Globally Distributed Citation Database
author: Dave Kinkead
category: thoughts
layout: post
---

What's a citation?  Essentially, it is nothing more than a pointer to some other work.  It tells a reader - human or machine - that this work points to, or references, another.  But what do we mean by 'pointing'?  Does a citation point to the location of another work - telling us where we can find it like a library catalogue - or does it point to the contents of the work - a designator of identity like someone's name?

When scholarship was tied to physical paper, citations were designators of identity.  When John Locke ridiculed Sir Robert Filmer's defence of Divine Right in his First Treatise of Government, he wasn't referring to the location of his copy of _Patriarcha_, he was referring to the identity, the contents, and the ideas found in that particular edition.  Once in print, the contents of what was cited persisted forever. Assuming of course that we could still access the work.

Now that scholarship has moved online however, digital identifiers like [DOIs][] point to locations.  Like a library catalogue or URL, citations these days tell us where to find a work, rather than what's in it.  But both the content and locations of these works can change.  [Linkrot][linkrot] and [short citation halflife][bugeja] are well known problems with digital citations that DOIs have been unable to [fully address][DOIprob].  Even when the location persists, we have no way of know if the information we access is the same as when it was cited - hence the addition of 'accessed on' to URL references.

Citations today have multiple audiences.  They need to provide sufficient information to allow a human reader to identify the source being referenced, without overwhelming the limited physical space of the page, or disturb reading flow.  But they also need to be machine readable - something that traditional citation styles turn into a [non-trivial endeavour][parsing].  While humans can happily deal with the fuzzy patterns of multiple free form citation styles, we struggle with machine readable ones like ISBNs.

Adding to the challenge is the plethora of citation styles.  The [CSL][csl-repo] project alone lists over 900 citation styles in use today which has resulted in an every growing array of reference management software that while solving one problem, has created another - the interoperability and sharing of bibliographic data.  Citations are difficult to share owing to proprietary software using their own formats, and open software like bibtex having no consistent citation key standards.

Citations are just links but links don't need to point to locations.  While most web users would be familiar with Uniform Resource Locators (URLs), it is unlikely most would have come across Uniform Resource Names (URNs).  Both URLs and URNs are Uniform Resource Indicators (URIs), but URNs differ in that they uniquely identify content rather than locations.  The way they do this is by generating a [cryptographic hash][hash] - a one way, deterministic function to generate an id - of the contents of a file.  The contents of the file determine the hash and the hash is unique to those contents.  Identical contents produce the same hash, and changing the contents will change the hash.

So what?  Well, if we consider all the information necessary to uniquely identify a work - author, title, publication date, etc - to be a citation data object, then we can hash the contents of the citation object and generate a globally unique citation key.  What's more, because the hash function can be implemented in a line or two of code, the process of generating keys can be globally distributed - anyone can apply the function to a citation object and get the same key as anyone else would.  In this way, all the cost and organisational disadvantages of having a centralised issuing authority are completely avoided.

Anyone can now create their own citation database, store and share citation data globally, and be sure that there wont be any key conflicts. 
 
## The Specification

The Globally Distributed Citation Database (GDCD - snappy name to come) specification contains a few parts.  

### Data 

The first is the citation object, which contains a series of key:value pairs of bibliographic data [^csl]. Keys can be any alphanumeric character plus `-` and `_` with the following provisos:

1.  Any key listed in the CSL Schema must conform with the CSL Schema
2.  The following keys are reserved and will not be used to generate the hash  

    > accessed, canonical, citekey, id, key

The citation object can be stored in any form but for hashing, it must be a valid JSON object.  

### Hashing

Once we have some citation data, we can hash it to generate a cite-hash or id.  The algorithm for generating the hash is:

    cite-hash -> sha-1( base64( stringified( ordered( cleaned( cite-json ) ) ) ) )

Here, the citation object in JSON form is cleaned by removing the reserved keys, then ordered alphanumerically by key.  This will ensure that alphabetical keys are ordered but numeric keys (arrays) remain unchanged - especially important where things like author order has semantic meaning.

Next the object is [stringified](https://npmjs.org/package/json-stable-stringify), converted to base64 encoding and the resulting string is passed to a [sha-1 hash](https://en.wikipedia.org/wiki/SHA-1) function.  The result is a globally unique 40 character string such as `a3db403190262e696b059ca91fec9e023b954a10` (and the first 7 characters should be sufficiently unique for day to day use).

### Storage

Data storage which is just a simple key:value document store of cite-hash:cite-object.  Locally this could be something as simple as a flat file (the entire data store could even be valid JSON).  Remotely, a distributed database like [CouchDB](https://couchdb.apache.org/) makes a perfect platform for a global citation service.  

Anyone could then replicate parts or the entire data store as they wished.  Libraries could host their own copies with easy - 200 million academic works, each with a canonical and say 5 differing versions - would take less than 2TB of storage.  Add some form of URN resolution such as `cite:a3db40319` which would return the cite-json object, and we have a robust, low cost global citation API that solves most use cases.

The GDCD isn't intended to replace DOIs. DOIs resolves the location problem which GDCD content problem of citations.  They are best thought of as ISBNs for citation data but distributed and far more flexible.

Thoughts? Please get in touch...



[^csl]: See the The [CSL schema][] for an excellent example of citation data. 

[bugeja]: http://www.academia.edu/799204/The_Half-Life_Phenomenon_Eroding_Citations_In_Journals

[linkrot]: https://en.wikipedia.org/wiki/Link_rot

[parsing]: http://labs.crossref.org/resolving-citations-we-dont-need-no-stinkin-parser/

[csl-repo]: https://github.com/citation-style-language/styles

[DOIprob]: http://crosstech.crossref.org/2013/09/dois-unambiguously-and-persistently-identify-published-trustworthy-citable-online-scholarly-literature-right.html

[grep]: https://en.wikipedia.org/wiki/Grep#Usage_as_a_verb

[DOIs]: http://www.doi.org/

[ra]: http://quod.lib.umich.edu/j/jep/3336451.0004.203

[mag]: https://en.wikipedia.org/wiki/Magnet_URI_scheme

[hash]: https://en.wikipedia.org/wiki/Hash_function

[CSL schema]: https://github.com/citation-style-language/schema/blob/master/csl-data.json
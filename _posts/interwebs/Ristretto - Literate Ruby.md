# Ristretto - Literate Ruby

  - execute blog code in ruby
  - $ ristretto post.md will:
    - parse indented code only
    - parse any `require_relative` commands that don't end in .rb or nil
    - execute parsed code
  - `-t` will transpile and pip to stdout

Does it work?

    p self

    def say_hello
      "Hello, Ristretto"
    end

I don't know.  Let's see.

    p say_hello
# Citations should embrace the hype(rlink)

In trying to flesh out a coherent strategy for the development of a scholarly markdown, one issue I find myself beating my head against a wall on, is the matter of references and citation.  There's much debate on the [digital] [academe] [interwebs] about an appropriate method of citing the works of others when using digital media.  Part of the problem stems from the fact that the markdown specification wasn't designed with scholarly writing in mind.  

Footnotes weren't part of Gruber's original idea.  Reading the [specifications][markdown], it's clear that the primary intention of markdown was making writing for the web more readable for authors.  It was only later that the [power] and [simplicity] of text based writing became clearer to many in the digital academe.  And with the advent of scripts like [Pandoc], markdown has become much more than simply a text-to-html translator - it can now serve as a common format that effectively separates semantics from presentation while maintaining intuitive readability.

Another, more significant part of the problem however, is that traditional citation styles were designed for static media.  Referencing serves a number of purposes in scholarship, not least the formal acknowledgement of others, and the encapsulation of apropos ideas not central to the current text.

The styles of referencing that has evolved to server these purposes are functions of the medium they evolved in - the printed text.  Print, despite its wonderful tactility, is a static medium.  Once produced, a printed work cannot be readily altered, nor can it reach out and direct its reader to other works.  Hypermedia by contrast can do both.  Hypermedia is in effect, republished every time a web browser makes a request to the server.  The published equivalent would be reprinting a work every time a page is turned.

Of course, the _[hyper]_ in hypermedia refers to its ability to link to other documents in non-linear ways.  Scholarly work is typically an episode within a broader conversation.  It is temporally and logically placed within a particular context, the position of which is established by citations.

As scholarship moves from static to hyper media, citation style should follow.  Citations should embrace the hyperlink because the hyperlink is simply an enhanced reference - one that not only informs, but also guides.  An not just human readers but a whole range of computer agents like web crawlers and distributed applications.  Digital scholarship has a broader audience than the traditional paper one.

So in designing a scholarly markdown, the syntax for citations has already been clarified for us - the hyperlink.  Only some presentation formats like print journals require a traditional citation format - and this can easily can accomplished whist maintaing the citation as hyperlink.

How do we do this?  Easy - just use the current markdown link syntax of

    [link text][ref]
    
    [ref]: url "optional title"
    
and apply a convention within the reference to indicate this is more than just a link, along with a static text format - I prefer the `@` symbol as it's widely used to specify a person.  This would result in something like:

    ...despite Breakey's [2009:p74][@breakey2009] protestations that utilitarians have access to sufficient information....
    
    [@breakey2009]: http://dx.doi.org/10.1017/S0953820808003373 "Hugh Breakey (2009). The Epistemic and Informational Requirements of Utilitarianism. Utilitas, 21, pp 72-99."

The advantage of this approach is threefold.  Firstly, it treats citations as links when used in hypermedia, with all the accompanying advantages of this.  Secondly, it maintains static media citation conventions in the link title field (which will also be visible on link hover events).  

Finally, it provides sufficient hooks for external scripts to make citation style automatic.  A preprocessor could for example, could look up any link reference key such as `[@breakey2009]` from an external library file or API, and insert the reference url and format the reference title at the end of the document according to a [CSL].  With this in place, all one would need to do to cite an author in text would be write [text][@author].

The purpose of markdown is to make writing simple and readable. End of story. Everything else, such as the heavy lifting of citation and reference formatting, can be done by other tools.

[digital]: http://blog.martinfenner.org/2013/06/19/citations-in-scholarly-markdown/
[academe]: http://blog.yoavram.com/citations-in-markdown-using-pandoc/
[interwebs]: http://www.gradhacker.org/2012/11/20/using-markdown-like-an-academic/
[markdown]: http://daringfireball.net/projects/markdown/
[csl]: https://github.com/citation-style-language
[hyper]: http://www.w3.org/Proposal.html
[power]: http://chronicle.com/blogs/profhacker/writing-power-tools-text-editors/38940
[simplicity]: http://genewilburn.wordpress.com/2013/08/15/regaining-simplicity-markdown-for-writers/
[pandoc]: http://johnmacfarlane.net/pandoc/
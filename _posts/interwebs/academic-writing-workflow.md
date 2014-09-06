# My Academic Writing Workflow

> Markdown + Pandoc + Marked


## Write in Markdown


## Transform in Marked


## Citations


Marked + Pandoc + Bibtex

    /usr/local/bin/pandoc
    -f markdown -t html5 --filter pandoc-citeproc
    
    ---
    title: The Limits of Liberalism
    author: Dave Kinkead
    email: d.kinkead@uq.edu.au
    status: First Draft
    license: CC-BY-SA-NC
    bibliography: /Users/dave/Dropbox/Research/readings/.library.bibtex
    ---
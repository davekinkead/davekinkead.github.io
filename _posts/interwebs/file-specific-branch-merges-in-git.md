# File specific branch merges in git

One scenario that seems to be far more common in academia than software development is the need to merge only specific files from one branch into another.  As a diligent digital scholar, I'm sure you are already using a version control friendly workflow - like branching semantically atomic chunks of work that can then be merged individually in pretty much any order - so you have greater flexibility in how you curate and ensemble your scholarship.  

Often though, and despite our best intentions, we need greater flexibility in how we merge our work.  Take the following example:

> You are working on your `chapter-3` branch and editing the `chapter-3.md` file.  As your chapter progresses, you also make changes to `outline.md` and `abstract.md` to reflect changes that have resulted from your new findings.  While you aren't yet ready to merge the whole `chapter-3` branch into `master`, you'd like to merge the changes you've made to the `abstract.md` and `outline.md` files now rather than wait for chapter 3 to be finished.

Unfortunately, `git cherrypick` wont work here because this is applied to commits, not files.  You could perhaps temporarily copy the files you want to merge somewhere else, revert to master and paste & overwrite them - but that would be a sloppy approach ripe for mistakes and one that ignores the purpose of a version control system.  A far more elegant and simple method however, is to use the options from `git checkout`.

    $ git branch
      master
      chapter-1
      chapter-2
    * chapter-3
    $ git checkout master
    $ git checkout chapter-3 outline.md abstract.md
    $ git status
    # On branch master
    # Changes to be committed:
    #   (use "git reset HEAD <file>..." to unstage)
    #
    #   new file:   outline.md
    #   new file:   abstract.md
    #
    $ git commit -m "Merge abstract and outlines from chapter 3 draft"
    [master e1ecaab]: "Merge abstract and outlines from chapter 3 draft"
    2 files changed, 293 insertions(+), 8 deletions(-)
     create mode 100644 abstract.md
     create mode 100644 outline.md
     
Now, the changes to `abstract.md` and `outline.md` you made on the `chapter-3` branch have been successfully merged into `master` while none of the other changes on `chapter-3` have been.
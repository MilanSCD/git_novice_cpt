---
title: 'Making Changes'
teaching: 10
exercises: 2
---

::::::::::::::::::::::::::::::::::::::: objectives

- Go through the modify-add-commit cycle for one or more files.
- Explain where information is stored at each stage of that cycle.
- Distinguish between descriptive and non-descriptive commit messages.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- How do I record changes in Git?
- How do I check the status of my version control repository?
- How do I record notes about what changes I made and why?

::::::::::::::::::::::::::::::::::::::::::::::::::

## Introduction

Once we have our repository set up, we can start making use of git's tracking features. To start with, we can use the `git status` command.
Files in the repository directory are either tracked or untracked.

### Tracked and untracked files

![File system diagram. The working tree files you see consist of files in the git HEAD, staged modifications, unstaged modifications and untracked files](fig/git_filesystem_diagram.svg)


Untracked files are files in the directory which have not yet been added to the git repository and git will not be able to track changes in these files.
however, git is aware of them: they will show up as untracked in a `git status` output.

Tracked files are files which have been added to the repository. Git will check to see if these files have been modified relative to the last commit (restore point).
Files only have to be committed once to be tracked from that point onwards.

### Modified files

Modified files are themselves either "unstaged", meaning they have not been marked to be included in the next restore point ("commit"), or "staged", meaning they will be included in the next commit.

When we use `git status`, we can see which files are in each state.

![git state diagram. git commands which change the state are shown as arrows. commands used in the modify add commit cycle are shown with their inverses. Note that commands from the staged and commited states apply to all the files in that state unless specified.](fig/git_modify_add_commit_cycle_diagram.png)

::::::::: callout
### .gitignore
Some files we explicitly choose not to track. They could be files generated from tests or sensitive things we don't want to share. To keep them untracked, we list them in a `.gitignore` file, which tells git to ignore them. We usually create this file at the start of a repository and update it as we go along. When files which are listed in the `.gitignore` are modified, the changes won't be shown in `git status` and cannot be staged or committed unless forced with additional commands.
:::::::::

Making changes and tracking them in git follows a 3-step cycle:

### 1 Modify
- make a change like making a new file or editing a paragraph
- this may include multiple files

### 2 Add
- tell git to bundle this modification as part of the next "save"
- multiple modifications or files can be added to this
- we call this bundle the "staging area". Files are "staged" using the `git add <file>` command
- only staged modifications can be part of a commit
- if a file has been modified after it has been staged, the new modification has to be staged again to be included.

### 3 Commit

- tell git to save the set of modifications we previously added, and create a new restore point ("commit")
- a message is added to describe the logical change from the previous point
- the message is written as an imperative by convention eg. `git commit -m add config file`

:::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::: discussion
### Atomic Commits
The idea of this cycle is that we should only create commits (restore points) for a minimal set of modifications that constitute a single self-consistent logical change. Each commit is saved as difference between the current commit and the previous one. Once we commit, the staged changes are now just part of the current version, so the staging area is empty. A copy of the committed version is saved in the .git directory.
::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::::

### Git History

We can then build up our project using this cycle with a new commit each time we make a logical change. Each commit is labelled with the commit message and a hash code that uniquely identifies it. This builds what we call the "history": the chain of commits which describe each step we took to get to the current version. We can view this history using the `git log` command.

![Simple git history. Each commit adds modifications to the last one. The branch "main" is just a label pointing to commit C4. "HEAD" is also just a label showing what is currently in the file system. We will see how we can add branches later.](fig/git_simple_history_diagram.png)

:::::::::::::::::::::::::::: challenge

## Follow the recipe

Make a new subdirectory in your recipes repository, and name it after a recipe you know.
Within this directory, create an `Ingredients.md` file and a `Method.md` file. 

Fill the files with the simplest details of your recipe and commit them to the repository.

Change one of ingredients and keep the name consistent in both files.
How should we add them to the repository to ensure each version is self consistent?

Make 4 or 5 more logical changes to your recipe and commit them when you feel appropriate.
Check your work using the `git status` and `git log` commands as you go along.

Don't worry about mistakes for now, just fix them in the next commit
:::::::::::::::::::::::::::: solution

For consistency we can add both files together:
`git add Ingredients.md Method.md`.
We can commit them like so: `git commit -m "change ingredient x to ingredient y"`



::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::


## Undoing things

Making changes and creating commits can seem daunting at first. It's easy to mix up what files to add to a commit, and mistakes happen all the time. We don't want these mistakes to also be saved indefinitely. The good news is that we can undo any of the steps in the cycle. 

![common git restore commands](fig/git_restore_comparison_diagram.svg)

Within the modify, add, commit cycle, we can undo any state changes and invert any command:

We can undo modifications to a file and restore the version in the previous commit using `git restore <file>`

We can unstage a file from the staging area while keeping the modification in the file system by doing `git restore --staged <file>`

![Git reset diagram](fig/git_reset_comparison_diagram.svg)

Finally, if we are working locally, we can undo the commit. This is called rewriting the history, and it is important that we only do this if the commit is local and hasn't been pushed to a remote, otherwise we risk permanently changing the history for everyone and affecting their work.

To undo the commit, we can use `git reset` with additional flags:

- `--soft`  undoes the act of the commit and keeps the staged modifications

- `--mixed` keeps the modifications but leaves them unstaged

- `--hard` undoes all the modifications and returns the state to the previous commit.


::::::::::::::::::::::::::::::::::::: challenge 

## Challenge 1: Git Tango


**Part 1: Two steps forward one step back**

Try creating a new file called `git_tango.md`

type in instructions like:

```output
# Git Tango
two steps forward
```

then use the git commands to track and stage the file.

confirm the change of state with `git status`

now unstage the file and confirm it again.

From this point, confirm each step using `git status` and `git log`

**Part 2: Three steps forward**

modify the file and add another instruction.

- use the commands to stage the new changes

- commit staged changes

- confirm the changes with `git status` and `git log`




**Part 3: One step back**

modify the file and add an incorrect instruction.

- use the commands to stage and commit the error.

- confirm the error with `git status` and `git log`

- undo the commit leaving modifications in the staging area



**Part 4: One step forward two steps back**

rename the commit and then unstage it

- commit again with a different message 

- undo the commit keeping modifications but unstaged


**Part 5: Three more steps forward**

correct the instruction in the file, then add and commit it



**Part 6: Three steps back**

completely undo the commit so it's unchanged from Part 2.


:::::::::::::::::::::::: solution 
### Part 1
use the commands `git add <file>` to stage a file
use `git status` to check it has been staged
use `git restore --staged <file>` to unstage the file
use `git status` to check it has been unstaged. 
::::::::::::::::::::::::
:::::::::::::::::::::::: solution
### Part 2 

modify the file then use `git add <file>` and `git commit -m "commit message"` to add a commit
use `git status` and `git log` to check the commit has been added.
::::::::::::::::::::::::
:::::::::::::::::::::::: solution
### Part 3

initially same as Part 2
then to inverse use `git reset --soft`
::::::::::::::::::::::::
:::::::::::::::::::::::: solution
### Part 4
use `git commit -m "new commit message"`
then to inverse and unstage `git reset --mixed`
::::::::::::::::::::::::
:::::::::::::::::::::::: solution
### Part 5
same as Part 2
::::::::::::::::::::::::
:::::::::::::::::::::::: solution
### Part 6
use `git reset --hard`


:::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::::

Note that when using `git reset` to undo a `git commit`, the same rule is applied to all the staged changes the commit contains, similar to how a `git commit` puts all the staged changes into a commit. This is different to how `git add <files>` and `git restore --staged <files>` apply to individual files. 

Now that you are familiar with making commits, try to add a few steps to this recipe in separate commits.
Remember to make single logical changes each time and use a descriptive commit message.

Finally use `git log` to see your history and commit messages.
Compare your log with someone else's and see if you can follow the changes they made.
Hopefully this will highlight the importance of single logical changes and accurate, concise commit messages!


::::::::::::::::::::::::::::::::::::: keypoints 

- Check the current state using `git status` and `git log`
- Make self-consistent logical changes with the modify add commit cycle
- Use concise, accurate commit messages to help you follow your process
- undo any part of this cycle using `git restore --staged` or `git reset with --soft, --mixed, or  --hard` 


::::::::::::::::::::::::::::::::::::::::::::::::


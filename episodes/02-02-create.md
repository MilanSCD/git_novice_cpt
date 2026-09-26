---
title: Creating a Repository
teaching: 10
exercises: 0
---

::::::::::::::::::::::::::::::::::::::: objectives

- Create a local Git repository.
- Describe the purpose of the `.git` directory.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- Where does Git store information?

::::::::::::::::::::::::::::::::::::::::::::::::::

Once Git is configured,
we can start using it.

First, let's create a new directory in the `Desktop` folder for our work and then change the current working directory to the newly created one:

```bash
$ cd ~/Desktop
$ mkdir recipes
$ cd recipes
```

Then we tell Git to make `recipes` a [repository](../learners/reference.md#repository)
\-- a place where Git can store versions of our files:

```bash
$ git init
```

If we use `ls` to show the directory's contents,
it appears that nothing has changed:

```bash
$ ls
```

But if we add the `-a` flag to show everything,
we can see that Git has created a hidden directory within `recipes` called `.git`:

```bash
$ ls -a
```

```output
.	..	.git
```

Git uses this special subdirectory to store all the information about the project,
including the tracked files and sub-directories located within the project's directory.
If we ever delete the `.git` subdirectory,
we will lose the project's history.

You only have to deal with the files in the project as normal and will never have to touch the .git subdirectory
git will handle what goes in there for you, so don't touch it!

We can now start using one of the most important git commands, which is particularly helpful to beginners. `git status` tells us the status of our project, and better, a list of changes in the project and options on what to do with those changes. We can use it as often as we want, whenever we want to understand what is going on.

```bash
$ git status
```

```output
On branch main

No commits yet

nothing to commit (create/copy files and use "git add" to track)
```

If you are using a different version of `git`, the exact
wording of the output might be slightly different.

:::::::::::::::::::::::::::::::::::::::  callout

## Places to Create Git Repositories

Once a repository is created with `git init` in a directory, it can track everything in that directory and any subdirectory.
Other repositories are not required to track subdirectories and doing this can lead to problems where the outer repository tracks inner ones.

Separation is key:

- unrelated projects should be version controlled separately in separate folders
- only relevant files should be in the repository directory and version controlled.

Also, note
that the creation of the `recipes` directory and its initialization as a
repository are completely separate processes.

:::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: keypoints

- `git init` initializes a repository.
- Git stores all of its repository data in the `.git` directory.
- Only one repository is required for a project and all its sub directories

::::::::::::::::::::::::::::::::::::::::::::::::::

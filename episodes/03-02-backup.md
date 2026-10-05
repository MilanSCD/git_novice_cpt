---
title: 'Backing it up'
teaching: 10
exercises: 2
---
::::::::::::::::::::::::::::::::::::::: objectives

- Explain what remote repositories are and why they are useful.
- Push to or pull from a remote repository.

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- How do I back up my work safely on the web?

::::::::::::::::::::::::::::::::::::::::::::::::::

Version control really comes into its own when we begin to collaborate with
other people.  We already have most of the machinery we need to do this; the
only thing missing is to copy changes from one repository to another.

Systems like Git allow us to move work between any two repositories.  In
practice, though, it's easiest to use one copy as a central hub, and to keep it
on the web rather than on someone's laptop.  Most programmers use hosting
services like [GitHub](https://github.com), [Bitbucket](https://bitbucket.org) or
[GitLab](https://gitlab.com/) to hold those main copies.

Let's start by sharing the changes we've made to our current project with the
world. To this end we are going to create a *remote* repository that will be linked to our *local* repository.

## 1\. Create a remote repository

Log in to [GitHub](https://github.com), then click on the icon in the top right corner to
create a new repository called `recipes`:

![](fig/github-create-repo-01.png){alt='The first step in creating a repository on GitHub: clicking the "create new" button'}

Name your repository "recipes" and then click "Create Repository".

Note: Since this repository will be connected to a local repository, it needs to be empty. Leave
"Initialize this repository with a README" unchecked, and keep "None" as options for both "Add
.gitignore" and "Add a license." See the "GitHub License and README files" exercise below for a full
explanation of why the repository needs to be empty.

![](fig/github-create-repo-02.png){alt='The second step in creating a repository on GitHub: filling out the new repository form to provide the repository name, and specify that neither a readme nor a license should be created'}

As soon as the repository is created, GitHub displays a page with a URL and some
information on how to configure your local repository:

![](fig/github-create-repo-03.png){alt='The summary page displayed by GitHub after a new repository has been created. It contains instructions for configuring the new GitHub repository as a git remote'}

This effectively does the following on GitHub's servers:

```bash
$ mkdir recipes
$ cd recipes
$ git init
```

Note that our local repository still contains our earlier work on our recipes, but the
remote repository on GitHub appears empty as it doesn't contain any files yet.

## 2\. Connect local to remote repository

Now we connect the two repositories.  We do this by making the
GitHub repository a [remote](../learners/reference.md#remote) for the local repository.
The home page of the repository on GitHub includes the URL string we need to
identify it:

![](fig/github-change-repo-string.png){alt='A screenshot showing that clicking on "SSH" will make GitHub provide the SSH URL for a repository instead of the HTTPS URL'}

Click on the 'SSH' link to change the [protocol](../learners/reference.md#protocol) from HTTPS to SSH.

:::::::::::::::::::::::::::::::::::::::::  callout

## HTTPS vs. SSH

We use SSH here because, while it requires some additional configuration, it is a
security protocol widely used by many applications.  The steps below describe SSH at a
minimum level for GitHub.


::::::::::::::::::::::::::::::::::::::::::::::::::

![](fig/github-find-repo-string.png){alt='Clicking the "Copy to Clipboard" button on GitHub to obtain the repository\'s URL'}

Copy that URL from the browser, go into the local `recipes` repository, and run
this command:

```bash
$ git remote add origin git@github.com:alflin/recipes.git
```

Make sure to use the URL for your repository rather than Alfredo's: the only
difference should be your username instead of `alflin`.

`origin` is a local name used to refer to the remote repository. It could be called
anything, but `origin` is a convention that is often used by default in git
and GitHub, so it's helpful to stick with this unless there's a reason not to.

We can check that the command has worked by running `git remote -v`:

```bash
$ git remote -v
```

```output
origin   git@github.com:alflin/recipes.git (fetch)
origin   git@github.com:alflin/recipes.git (push)
```

We'll discuss remotes in more detail in the next episode, while
talking about how they might be used for collaboration.

## 4\. Push local changes to a remote

Now that authentication is setup, we can return to the remote.  This command will push the changes from
our local repository to the repository on GitHub:

```bash
$ git push origin main
```

Since Alfredo set up a passphrase, it will prompt him for it.  If you completed advanced settings for your authentication, it
will not prompt for a passphrase.

```output
Enumerating objects: 16, done.
Counting objects: 100% (16/16), done.
Delta compression using up to 8 threads.
Compressing objects: 100% (11/11), done.
Writing objects: 100% (16/16), 1.45 KiB | 372.00 KiB/s, done.
Total 16 (delta 2), reused 0 (delta 0)
remote: Resolving deltas: 100% (2/2), done.
To https://github.com/alflin/recipes.git
 * [new branch]      main -> main
```


Our local and remote repositories are now in sync.

![in the general no conflict case, a git push adds your new commits to your remote branch so they are in sync](fig/git_push_diagram.svg)


:::::::::::::::::::::::::::::::::::::::::  callout

## The '-u' Flag

You may see a `-u` option used with `git push` in some documentation.  This
option is synonymous with the `--set-upstream-to` option for the `git branch`
command, and is used to associate the current branch with a remote branch so
that the `git pull` command can be used without any arguments. To do this,
simply use `git push -u origin main` once the remote has been set up.


::::::::::::::::::::::::::::::::::::::::::::::::::

We can pull changes from the remote repository to the local one as well:

```bash
$ git pull origin main
```

```output
From https://github.com/alflin/recipes
 * branch            main     -> FETCH_HEAD
Already up-to-date.
```

Pulling has no effect in this case because the two repositories are already
synchronized.  If someone else had pushed some changes to the repository on
GitHub, though, this command would download them to our local repository.

![In the general no conflict case a git pull adds its new commits to your local branch](fig/git_pull_diagram.svg)

If you don't want to necessarily add the commits, but you want to refresh your view of the remote branch you can use `git fetch` instead.
A `git pull` is actually just a `git fetch` followed by a `git merge` which we will discuss later.

![Git fetch updates the remote tracking branch. here origin/main moves to C4 and the local main branch is 2 commits behind](fig/git_fetch_diagram.svg)

:::::::::::::::::::::::::::::::::::::::  challenge

## GitHub GUI

Browse to your `recipes` repository on GitHub.
Under the Code tab, find and click on the text that says "XX commits" (where "XX" is some number).
Hover over, and click on, the three buttons to the right of each commit.
What information can you gather/explore from these buttons?
How would you get that same information in the shell?

:::::::::::::::  solution

## Solution

When you click on the left-most button, you'll see all of the changes that were made in that
particular commit. Green shaded lines indicate additions and red ones removals. In the shell we
can do the same thing with `git diff`. In particular, `git diff ID1..ID2` where ID1 and
ID2 are commit identifiers (e.g. `git diff a3bf1e5..041e637`) will show the differences
between those two commits.

The middle button (with the picture of two overlapping squares or pages) copies the full identifier of the commit
to the clipboard. In the shell, `git log` will show you the full commit identifier for each commit.

The right-most button lets you view all of the files in the repository at the time of that
commit. To do this in the shell, we'd need to checkout the repository at that particular time.
We can do this with `git checkout ID` where ID is the identifier of the commit we want to
look at. If we do this, we need to remember to put the repository back to the right state
afterwards!



:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::::  callout

## Uploading files directly in GitHub browser

Github also allows you to skip the command line and upload files directly to
your repository without having to leave the browser. There are two options.
First you can click the "Upload files" button in the toolbar at the top of the
file tree. Or, you can drag and drop files from your desktop onto the file
tree. You can read more about this [on this GitHub page](https://help.github.com/articles/adding-a-file-to-a-repository/).


::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## GitHub Timestamp

Create a remote repository on GitHub. Push the contents of your local
repository to the remote. Make changes to your local repository and push these
changes. Go to the repo you just created on GitHub and check the
[timestamps](../learners/reference.md#timestamp) of the files. How does GitHub
record times, and why?

:::::::::::::::  solution

## Solution

GitHub displays timestamps in a human readable relative format (i.e. "22 hours ago" or "three
weeks ago"). However, if you hover over the timestamp, you can see the exact time at which the
last change to the file occurred.



:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## Push vs. Commit

In this episode, we introduced the "git push" command.
How is "git push" different from "git commit"?

:::::::::::::::  solution

## Solution

When we push changes, we're interacting with a remote repository to update it with the changes
we've made locally (often this corresponds to sharing the changes we've made with others).
Commit only updates your local repository.



:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::  challenge

## GitHub License and README files

In this episode we learned about creating a remote repository on GitHub, but when you initialized
your GitHub repo, you didn't add a README.md or a license file. If you had, what do you think
would have happened when you tried to link your local and remote repositories?

:::::::::::::::  solution

## Solution

In this case, we'd see a merge conflict due to unrelated histories. When GitHub creates a
README.md file, it performs a commit in the remote repository. When you try to pull the remote
repository to your local repository, Git detects that they have histories that do not share a
common origin and refuses to merge.

```bash
$ git pull origin main
```

```output
warning: no common commits
remote: Enumerating objects: 3, done.
remote: Counting objects: 100% (3/3), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), done.
From https://github.com/alflin/recipes
 * branch            main     -> FETCH_HEAD
 * [new branch]      main     -> origin/main
fatal: refusing to merge unrelated histories
```

You can force git to merge the two repositories with the option `--allow-unrelated-histories`.
Be careful when you use this option and carefully examine the contents of local and remote
repositories before merging.

```bash
$ git pull --allow-unrelated-histories origin main
```

```output
From https://github.com/alflin/recipes
 * branch            main     -> FETCH_HEAD
Merge made by the 'recursive' strategy.
README.md | 1 +
1 file changed, 1 insertion(+)
create mode 100644 README.md
```

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: keypoints

- A local Git repository can be connected to one or more remote repositories.
- Use the SSH protocol to connect to remote repositories.
- `git push` copies changes from a local repository to a remote repository.
- `git pull` copies changes from a remote repository to a local repository.

::::::::::::::::::::::::::::::::::::::::::::::::::

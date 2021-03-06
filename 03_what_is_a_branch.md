# What is a branch?

## Creating a repository to play around in

```sh
mkdir workshop
cd workshop
git init
git switch -c root
git commit -m "First commit" --allow-empty
git switch -c main
```

## Branches are just pointers

After having talked about commits, talking about branches almost feels
anticlimactic. A branch is simply an alias for a commit hash that we can move
around in the commit tree.

```sh
clear
git log
```

If we look in the brackets next to the commit in the output we will see some
variation on the following: `(HEAD -> main)`. We can think of HEAD as the red
symbol on a map that says "This is your current position".

The symbol indicates that HEAD is currently pointing at the branch main. The
information about what HEAD is currently pointing to can be found in `.git/HEAD`

```sh
ls -a
ls .git
nvim .git/HEAD
```

We can see that it points to a different file, refs/heads/main. We continue
our investigation:

```sh
nvim .git/refs/heads/main
```

So main is just another name for a hash! This is one of the main takeaways
that in this workshop. Except as handy aliases,
branches have absolutely no meaning in the git object model.

## Creating a branch

Creating a branch is done with `git switch -c <name>`. 

```sh
git switch -c example
git log # Notice that HEAD is pointing to a new branch
ls .git/refs/heads/
nvim .git/HEAD
```

As we can see there can be multiple branches pointing to the same commit. HEAD
shows which one we are currently working on. That branch will follow us around
when we perform branch operations.

## Creating some commits to move around between

In order to create some more space to move around, let's create a bunch of commits:

```sh
for i in {1..5} ; do git commit --allow-empty -m "example $i" ; done
git switch main
for i in {1..5} ; do git commit --allow-empty -m "main $i" ; done

git log
```

## Visualizing branches

At this point it is getting very difficult to visualize both branches and their
commits. Let's improve our experience.

```sh
git log --oneline # Only see the commit headings
git log --oneline --all # See all refs and HEAD
git log --oneline --all --graph # Visualize the commit relationships
git log --oneline --all --graph -10 # Only show the last 10 commits
git log --oneline --all --graph
```

We are going to start referring to this view as the commit tree, or simply the
tree. Let's at it for a bit, to make sure that we understand it.

1. The stars are individual commits
2. The columns are linear commit histories, i.e. where each commit is based
   directly on the previous one.
3. Branching points are shown with the | and / characters

## Switching between branches

The switch command lets us move HEAD around.

```sh
git switch example
git log --oneline --all --graph # Notice what happened to HEAD
git switch main
git log --oneline --all --graph # Notice what happened to HEAD
git switch example
git log --oneline --all --graph # Notice what happened to HEAD
nvim .git/HEAD
nvim .git/refs/heads/example
nvim .git/refs/heads/main
git log --oneline --all --graph # Notice what happened to HEAD
```

## Extra: Creating an alias

**We're running the log command all the time! Let's create an alias**

There are two ways to create an alias in a commit. You can either add it to the
git config or by adding it as a binary on the path.

### Inside the git config

```sh
git config --global --edit
```

```gitconfig
[alias]
# Shell command
    hello = "!echo 'hello world!'"
# Git command
    ll = "log --oneline --all --graph"
```

### On the path

```sh
git hello
git ll

nvim ~/bin/git-l
```

```sh
#!/bin/sh

git --no-pager log --oneline --all --graph -"${1:-20}"
```

```sh
chmod +x ~/bin/git-l
git l
git l 5
g l 5
```


## Detaching HEAD

HEAD does actually not have to point to a branch, it can also point to a commit
directly.

```sh
git log --oneline --all --graph # HEAD is attached to example
git show HEAD --format='format:%H'
git switch -d "$(git show HEAD --format='format:%H')"
git log --oneline --all --graph # Here HEAD is detached (no arrow)
```

We note that there is no longer an arrow between HEAD and main. HEAD points
directly to the commit hash.

```sh
nvim .git/HEAD
```

While cool, the disadvantage with doing this is that when we
make commits in this state, no branches will be updated to point to the new
commits, so we'll have to remember the commit hashes ourselves (or create
a branch before moving on).

**With effective use of branches, we won't have to remember commit hashes much.
Just remember that the branches are only temporary labels for commits.**

```sh
git log --oneline --all --graph
git switch -c temp
git log --oneline --all --graph
cat .git/HEAD
cat .git/refs/heads/example
cat .git/refs/heads/temp
cat .git/refs/heads/main
```

## Moving branches around

**Warning! Always make sure to commit any changes that we want to keep before
moving on! Uncommitted changes can not be recovered!**

If we have want to have some files in our projects that should not be included
in the git history, it can be helpful to set up a global gitignore file.

```sh
git config --global --edit
```

```gitconfig
[core]
  excludesfile = "~/.gitignore_global"
```

To move a branch to a different commit / ref, we use `git reset --hard
<ref-or-hash>`. We can think of `git reset --hard` as a `git switch` operation
that brings the attached branch along.

```sh
git switch temp
git log --oneline --all --graph
git reset --hard main
git log --oneline --all --graph
git reset --hard example
git log --oneline --all --graph
git reset --hard example~2
git log --oneline --all --graph
git reset --hard main~2
git log --oneline --all --graph
git reset --hard root
git log --oneline --all --graph
git reset --hard main~
git log --oneline --all --graph
git reset --hard example
git log --oneline --all --graph
```

It's a good idea to play around with this until we feel confident that the commit tree
does not change at all when we create, change or update branches. We are just moving a pointer
around in the tree.

## Removing branches

To remove a branch (with force) in git, we run `git branch -D temp`.

```sh
git log --oneline --all --graph
git branch -D temp # Cannot delete current branch
git switch main
git log --oneline --all --graph
git branch -D temp
git log --oneline --all --graph
```

To check our understanding: Consider what would happen if we were to delete the
branch example here. What do we think would happen?

```sh
git branch -D example
git log --oneline --all --graph
```

**Where did the commits go?**

## Clean-up

```sh
cd ..
rm -rf workshop
```

Next Module: [How do I recover from mistakes?](04_using_the_reflog.md)  
Back to [README.md](README.md)

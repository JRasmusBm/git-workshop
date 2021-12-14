# What is a branch?

After having talked about commits, talking about branches almost feels
anticlimactic. A branch is simply an alias for a commit hash that you can move
around in the commit tree.

```sh
git log
```

If you look in the brackets next to the commit in the output you will see some
variation on the following: `(HEAD -> main)`. You can think of HEAD as the red
symbol on a map that says "This is your current position".

The symbol indicates that HEAD is currently pointing at the branch main. The
information about what HEAD is currently pointing to can be found in `.git/HEAD`

```sh
nvim .git/HEAD
```

We can see that it points to a different file, refs/heads/main. We continue
our investigation:

```sh
:wq
nvim .git/refs/heads/main
```

So main is just another name for a hash! This is one of the main takeaways
that I want to give everybody in this workshop. Except as handy aliases,
branches have absolutely no meaning in the git object model.

HEAD does actually not have to point to a branch, it can also point to a commit
directly.

```sh
git show HEAD
git show HEAD --format='format:%H'
git checkout "$(git show HEAD --format='format:%H')"
git log
```

Notice that there is no longer an arrow between HEAD and main. HEAD points
directly to the commit hash.

```sh
nvim .git/HEAD
:wq
```

While cool, the disadvantage with doing this is that when you
make commits in this state, no branches will be updated to point to the new
commits, so you'll have to remember the commit hashes yourself (or create
a branch before moving on).

**With effective use of branches, you won't have to remember commit hashes much.
Just remember that the branches are only temporary labels for commits.**

## Creating a branch

```sh
git checkout -b example
git log # Notice that HEAD is pointing to a new branch
ls .git/refs/heads/
```

As you can tell there can be multiple branches pointing to the same commit. HEAD
shows which one we are currently working on. That branch will follow us around
when we perform branch operations.

## Switching between branches

```sh
git checkout main
git log # notice what happened to HEAD
git checkout example
git log # notice what happened to HEAD
```

## Removing branches

```sh
git checkout main
git branch -D example
git log
```

As long as you have no other software involved using the branches to know which
version of your software to operate on, deleting branches is not scary at all.

Let me show you, let's create a bunch of commits:

```sh
git checkout -b example
for i in {1..5} ; do git commit --allow-empty -m "example $i" ; done
git checkout main
for i in {1..5} ; do git commit --allow-empty -m "main $i" ; done

git log
```

At this point it is getting very difficult to visualize both branches and their
commits. Let's improve our experience.

```sh
git log --oneline # Only see the commit headings
git log --oneline --all # See all refs and HEAD
git log --oneline --all --graph # Visualize the commit relationships
git log --oneline --all --graph -10 # Only show the last 10 commits
git log --oneline --all --graph
```

I am going to start referring to this view as the commit tree, or simply the
tree. Let's at it for a bit, to make sure that we understand it.

1. The stars are individual commits
2. The columns are linear commit histories, i.e. where each commit is based
   directly on the previous one.
3. Branching points are shown with the | and / characters

Let's look at what happens when I checkout a branch.

```sh
git checkout example
git log --oneline --all --graph
git checkout main
```

We're just moving the HEAD pointer around. Let's look again in the .git/ folder:

```sh
cat .git/HEAD
cat .git/refs/heads/example
cat .git/refs/heads/main
```

```sh
git checkout main
git log --oneline --all --graph
```

## Moving branches around

**Warning! Make sure you commit any changes that you want to keep before
moving on!**

It can be helpful to set up a global gitignore file.

```sh
git config --global --edit
```

```gitconfig
[core]
  excludesfile = "~/.gitignore_global"
```

```sh
git log --oneline --all --graph
git reset --hard <target-commit>
```


Feel free to play around with this until you feel confident that the commit tree
does not change at all when you change branches. You are just moving a pointer
around in the tree.

Let's get back to the example of deleting a branch without worrying.

To check your understanding: Consider what would happen if I were to delete the
branch example here. What do you think would happen?

```sh
git branch -D example
git log --oneline --all --graph
```

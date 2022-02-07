# What is a commit

## Create a repository to play around in

```sh
mkdir workshop
cd workshop
git init
git switch -c root
git commit -m "First commit" --allow-empty
git switch -c main
```

## Add a commit with a file

Now we're going to try to understand what a commit is. To do that we'll create
a new one. We'll start by adding a TypeScript file.

```sh
nvim README.md
```

```markdown
# Hi!
```

```sh
git add README.md
git commit
```

We add a commit message describing the change and potentially why it was made.

```markdown
Add README.md

**Why** was the change needed?

To be able to demonstrate what a commit is.
```

When we close the file in our editor, given that we made any change to the
commit message, the contents of the file (minus the comments) are added as
the commit message.

## Add a second commit that updates the same file

I want to show what a commit looks like that edits an existing file.

```sh
nvim README.md # Add a welcome notice
```

```markdown
# Hi!

Welcome to my repository!
```

```sh
git add README.md
git commit
```

```markdown
Add a welcome notice to the README

**Why** was the change needed?

To show what happens when we create a commit with changes to an
existing file.
```

## What is a commit?

We will now look at each of the three commits we have in our repository so far.
The `~` symbol lets us select a relative commit, which can be really useful when
we don't want to look up a commit hash.

```sh
clear
git log
clear
git show main
clear
git show main~1
clear
git show main~2
```

A commit in git is:

- An immutable object
- That contains
  - changes since the last commit (not the whole code-base)
  - metadata like creator and time
- Represented by a hash

- The commit log is the source of truth for git
- The current file tree is found by traversing the commit log from the beginning
  of the project until the current commit.

## Clean-up

```sh
cd ..
rm -rf workshop
```

Next Module: [What is a branch? How do I use them effectively?](03_what_is_a_branch.md)  
Back to [README.md](README.md)

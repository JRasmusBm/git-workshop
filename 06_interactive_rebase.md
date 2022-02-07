# Interactive Rebase

## Setting the stage

Let's set up a new repository with some hastily made commits.

```sh
mkdir workshop
cd workshop
git init
git checkout -b root
git commit -m "First commit" --allow-empty
git checkout -b main
```

We'll create an example branch, since it's generally not recommended to perform
interactive rebases in branches that are either:

1. Connected to CD (`staging` and `prod` for example)
2. Checked out by other people (unless you communicate it well)

```sh
git checkout -b example
mkdir src
nvim src/app.ts
```

```typescript
console.log('Hello world!');
```

```sh
git add .
git commit -m "WIP"

nvim src/app.ts # Add mistake
git add src
git commit -m "Add very important equation"

nvim src/app.ts # Fix formatting
git add src
git commit -m "Fix formatting"

git log --oneline --all --graph
git checkout -b old_example
git checkout -b example
```

It's hard to make a minimal example that's also useful, but bear with me. We
have three commits that we are maybe not so proud of.

```sh
git rebase -i HEAD~2 # Can pass any ref
git rebase -i main # Most common
```

The file that opens shows one commit per line, prefaced with the operation.
I recommend reading the comment when you have time.

**Note:** In the default layout in the interactive rebase file, the latest
commit is **at the bottom**. In some GUIs (i.e. VSCode) they are ordered from
top to bottom instead. 

To perform an operation, one changes the first word of the line with the
revision one wants to change, and that will be taken into account when the
rebase is run.

## Reword - Changing a commit message

```sh
git rebase -i main
git log --oneline --all --graph
```

We change the word `pick` on the first line to `r` (or `reword`) and close the
file. Immediately on closing the file the commit message for the file marked as
`reword` will be opened up.

```gitcommit
Add a hello-world file

**Why** is the change needed?

So that we can check that we can run the program.
```

Notice that all the commit hashes changed! This means that if we imagine that
origin is pointing to `old_example`, we will have to perform aforce push in
order to update it.

```sh
# git push -f
```

Is equivalent to (if it would have been possible with a remote)

```sh
git checkout old_example
git reset --hard example
git checkout example

git log --oneline --all --graph
```

## Squash - Joining two commits into one

Sometimes we want to keep the changes, but as part of the previous commit. This
is especially useful when we make a small fix that is not helpful as part of the
final git history.

```sh
git rebase -i main
```

We change the word `pick` on the line with the formatting fix to `s` (or
`squash`) and close the file. Immediately on closing the file the commit message
for the new commit (which is the two previous commits combined) will open up. We
remove the second commit message and close the file.

```sh
git log --oneline --all --graph
```

The formatting commit will have been merged into the commit that came before it,
which we can be verify by examining the commit.

```sh
git show HEAD~
```

We can see that the commit now looks like we added the hello world message with
the correct formatting from the start.

## Fixup - Squashing and disregarding the commit message

We'll reset our changes back to `old_example` to show how to do the same thing
as with `squash` with `fixup`.

```sh
git reset --hard old_example
git log --oneline --all --graph
git rebase -i main
```

We change the word `pick` on the first line to `f` (or `fixup`) and close the file. Since we didn't have to edit the commit message, we're already done.

```sh
git log --oneline --all --graph
git show HEAD~

# Pretending to force push
git checkout old_example
git reset --hard example
git checkout example
```

## Reordering commits

Another useful operation that can be done in an interactive rebase is to reorder
commits. This is done by changing the lines around.

```sh
git log --oneline --all --graph
git rebase -i main
```

Swap the lines and close the file. This will result in a conflict because the
first commit added the file, which means that when we reorder them, the commit
we're putting first is editing a non-existent file.

```sh
nvim src/app.ts # Remove the line with hello world
git add src
git rebase --continue

nvim src/app.ts # Keep both changes
git add src
git rebase --continue

git log --oneline --all --graph
```

## Drop - Exclude a commit from the history

Sometimes when editing the git history we find a commit that we don't need
anymore. Then the `drop` operation can be useful.

```sh
git log --oneline --all --graph
git rebase -i main
```

Change the operation of the commit with the equation to `d` (or `drop`). We get
a conflict again because this time our hello world commit is editing an empty
file.

```sh
nvim src/app.ts # Remove the equation
git add src
git rebase --continue

git log --oneline --all --graph
```

## Clean up

```sh
cd ..
rm -rf workshop
```


**Discussion**

- Use-cases for interactive rebase
- How to effectively use interactive rebase during a code review
- Is there any use for a PR squash if the team is comfortable with interactive rebases?
- How bad are force-pushes?

Next Module: [Cherry-pick](07_cherry_pick.md)  
Back to [README.md](README.md)

# Merge vs Rebase

When I was first taught how to use git, I was taught the following commands.

```sh
git pull
git push
git add
git commit
git merge
```

Those were also mostly the commands I used for the longest time. Then one day
I learned about rebase and since then I consider merging anywhere else than into
a main branch an anti-pattern.

## Setting the stage

Let us try it out. For it to be a bit more realistic i will create some real
branches and create a conflict.

```sh
git checkout main
git reset --hard HEAD~5
git checkout example
git reset --hard HEAD~6
clear
git log --oneline --all --graph

nvim src/app.ts # Add a type
git add src
git commit
git checkout -b example_merge
```

```markdown
Add type information to a

**Why** was the change needed?

I want to create a conflict with the main branch in order to demonstrate rebasing.
```

```sh
git log --oneline --all --graph
git checkout main

nvim src/app.ts # Change greeting
git add src
git commit

git checkout example
git log --oneline --all --graph
```

## Performing a merge

A merge goes chronologically through the commit history and tries to (in
a zipper-like pattern) find a common path to the same state as the branch that
is the target of the command.

```sh
git merge main
nvim src/app.ts # Notice that it automatically resolved the top line

git merge --abort # You can abort a merge at any point
```

Conflicts are a good thing! Git mostly is able to apply the changes
without help, but whenever it feels unsure about what you actually wanted, it
will ask you to clarify. The alternative would be much worse, where it would
randomly make a decision for you.

Sometimes when resolving a conflict it can be very hard to see how the two
pieces of code could have originated from the same line. I therefore recommend
the following setting.

```gitconfig
[merge]
  conflictstyle = diff3
```

You will see that it has now added a new section in the conflict diff. The
middle section is the code at the closest common parent in the commit tree.

```sh
git merge main
git add src
git merge --continue
```

## Performing a rebase

Rebase is for once a rather useful name for the git operation. What it does is
it puts the current branch pointer next to the branch you're rebasing on, and
then replays the changes from your branch on top of the other branch, creating
new commits as it goes.

```sh
git checkout example
git checkout -b example_rebase
git rebase main
git rebase --abort # If you're unhappy you can abort at any time
git status
git rebase main
git status

nvim src/app.ts # Works the same way (notice the diff changed order)
```

```sh
git add src
git rebase --continue

git log --oneline --all --graph
```

## Result of both patterns

```sh
git checkout main
git checkout -b main_with_rebase
git merge --no-ff example_rebase
git checkout main
git checkout -b main_with_merge
git merge --no-ff example_merge
```

Let's compare the results.

```sh
git log --oneline --all --graph

clear
git checkout main_with_merge
git log --oneline --graph
git log --oneline

git checkout main_with_rebase
git log --oneline --graph
git log --oneline
```

## Conclusion

Use rebase to get your branch up to date with another branch. Use merge to push your branch into a shared branch.

## Some useful related settings

```sh
git config --global --edit
```

### Enforce the conclusion above

These will take some getting used to, but they will improve your life over time.

```gitconfig
[merge]
  ff = only
[pull]
  ff = only
```

This makes it so that the default behavior of merge and pull is to bail if
fast-forward is not possible (i.e. you haven't rebased).

That means that from now on you will have to start using fetch when somebody
force pushes in your branch, and that you'll have to use rebase to update your
branch with the changes from another.

### Autostash

```gitconfig
[rebase]
  autostash = true
```

It's useful to turn on autostash, or you just make a throwaway commit with your
current workspace.

### Fetch prune

Remote branches can quickly crowd up your local branch list. The following
setting will remove the local ref to the remote branch when you run git fetch
/ pull.

```gitconfig
[fetch]
  prune = true
```

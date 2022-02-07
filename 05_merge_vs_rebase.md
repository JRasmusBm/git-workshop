# Merge vs Rebase

When first taught how to use git, many of us were taught the following commands
(assuming an initialized repository).

- `git pull`
- `git push`
- `git add`
- `git commit`
- `git merge`

Those were also mostly the commands I used for the longest time. In this section
we learn about rebase, and I will argue that merging from a shared branch into
a feature branch is an anti-pattern. That is what rebase is for.

## Setting the stage

Let us try it out. For it to be a bit more realistic i will create some real
branches and create a conflict.

```sh
mkdir workshop
cd workshop
git init
git switch -c root
git commit -m "First commit" --allow-empty
git switch -c main
mkdir src
nvim src/app.ts
```

We'll add a small TypeScript file.

```typescript
console.log("hello")

const a = "25"

console.log(`here's some information about ${a}`)
console.log({
        int: Number.parseInt(a),
        charLength: a.length
})
```

```sh
git add src
git commit
```

```gitcommit
Add a script with information about a number a

**Why** was the change needed?

I conducting a git workshop and want to show how to make a commit. The
code itself has no significance.
```

```sh
git switch -c example
nvim src/app.ts # Rename a to b
git add src
git commit
```

```gitcommit
Rename `a` to `b`

**Why** was the change needed?

To show what happens when we create a commit with changes to an
existing file.
```

```sh
git switch main
git log --oneline --all --graph
nvim src/app.ts # Add type information to a
git add src
git commit
```

```markdown
Add type information to a

**Why** was the change needed?

I want to create a conflict with the main branch in order to demonstrate rebasing.
```

```sh
git log --oneline --all --graph

nvim src/app.ts # Change greeting
git add src
git commit
```

```gitcommit
Update greeting to be more polite

**Why** was the change needed?

I want to demonstrate a change that git resolves automatically.
```

```sh
git switch example
git switch -c example_merge
git log --oneline --all --graph
```

## Performing a merge

A merge goes chronologically through the commit history and tries to (in
a zipper-like pattern) find a common path to the same state as the branch that
is the target of the command.

```sh
git merge main
nvim src/app.ts # Notice that it automatically resolved the top line

git merge --abort # We can abort a merge at any point
```

Conflicts are a good thing! Git mostly is able to apply the changes
without help, but whenever it feels unsure about what we actually wanted, it
will ask us to clarify. The alternative would be much worse, where it would
randomly make a decision for us.

Sometimes when resolving a conflict it can be very hard to see how the two
pieces of code could have originated from the same line. I therefore recommend
the following setting.

```sh
git config --global --edit
```

```gitconfig
[merge]
  conflictstyle = diff3
```

When we re-run the merge/rebase we see that it has now added a new section in
the conflict diff. The middle section is the code at the closest common parent
in the commit tree.

```sh
git merge main

nvim src/app.ts # Resolve the conflict

git add src
git merge --continue
git log --oneline --all --graph
```

## Performing a rebase

Rebase is for once a rather useful name for the git operation. What it does is
it puts the current branch pointer next to the branch we're rebasing on, and
then replays the changes from our branch on top of the other branch, creating
new commits as it goes.

```sh
git switch example
git switch -c example_rebase
git log --oneline --all --graph
git rebase main
git rebase --abort # If we're unhappy we can abort at any time
git status
git rebase main
git status

nvim src/app.ts # Resolve the conflict
```

```sh
git add src
git rebase --continue

git log --oneline --all --graph
```

## Result of both patterns

```sh
git switch main
git switch -c main_with_rebase
git merge --no-ff example_rebase
git switch main
git switch -c main_with_merge
git merge --no-ff example_merge
```

Let's compare the results.

```sh
clear
git log --oneline --all --graph
```

It's a bit crowded, let's look at them separately. Remember to look at the
changes from the perspective of the main branch.

```sh
clear
git switch main_with_merge
git log --oneline --graph
git switch main_with_rebase
git log --oneline --graph

clear
git switch main_with_merge
git log --oneline
git switch main_with_rebase
git log --oneline
```

When we compare the two approaches we can notice a difference in the order of
the commits. In the rebase version, the commits that were included in the final
merge are added as the last commits before the final merge. In the merge version
they are included chronologically.

This, along with the fact that the graph gets easier to take in with the rebase
approach is why rebase is often favored for updating our feature branches to
the state of a shared branch.

## Conclusion

Rebase replays commits on top of the other history, merge tries to join them
chronologically. We use rebase our branch up to date with changes in another
branch, and use merge to add our changes to a shared branch (i.e. main or
staging).

## Some useful related settings

```sh
git config --global --edit
```

### Enforce the conclusion above

These take some getting used to, but they really improve our life using git
over time.

```gitconfig
[merge]
  ff = only
[pull]
  ff = only
```

This makes it so that the default behavior of merge and pull is to bail if
fast-forward is not possible (i.e. we haven't rebased).

That means that from now on we will have to start using fetch when somebody
force pushes in our branch, and that we'll have to use rebase to update our
branch with the changes from another.

```sh
git switch example
git merge main # Disallowed
```

### Autostash

Often we have changes that we haven't committed, but we still want to perform
a rebase without having to manually stash or create extra commits. There is
a useful setting to tell git to do this on automatically on rebase.

```sh
git config --global --edit
```

```gitconfig
[rebase]
  autostash = true
```

### Fetch prune

Remote branches can quickly crowd up our local branch list. The following
setting will remove the local ref to the remote branch when we run git fetch
/ pull.

```sh
git config --global --edit
```

```gitconfig
[fetch]
  prune = true
```

## Clean-up

```sh
cd ..
rm -rf workshop
```

Next Module: [Maintaining a clean history](06_interactive_rebase.md)  
Back to [README.md](README.md)


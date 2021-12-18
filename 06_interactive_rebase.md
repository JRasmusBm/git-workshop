# Interactive Rebase

```sh
git rebase main
nvim src/app.ts
git add src
git rebase --continue
git log --oneline --all --graph
```

Notice that the commit message is now a bit confusing. We are still using the
old name of the variable. Let's fix that.

```sh
git rebase -i HEAD~2
git rebase -i HEAD~4

git checkout -b example_copy
git checkout example

git rebase -i main # Usually what we will be doing.
```

All the possible commands are available in the list.

To just change the commit message, `reword` is the handiest suggestion.

```sh
git rebase -i main 
git log --oneline --all --graph
```

After any kind of rebase, including an interactive one, at least one commit will
have been replayed and thus assigned a new commit hash.

That means that we will have to perform a force push to sync the branches. 

```sh
git push -f
```

Is equivalent to (if it would have been possible)

```sh
git checkout <remote-branch>
git reset --hard <local-branch>
git checkout <local-branch>
```

**Discussion**

- Use-cases for interactive rebase
- How to effectively use interactive rebase during a code review
- Is there any use for a PR squash if the team is comfortable with interactive rebases?

Next Module: [Cherry-pick](07_cherry_pick.md)  
Back to [README.md](README.md)

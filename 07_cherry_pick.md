# Cherry-pick

Another very useful git operation in the same vein as rebase is `git
cherry-pick`. It lets us take one or more commits and replay them on top of the
current branch.
 
Let's create some dummy commits. We'll create two branches, `main` and
`example`, each with 5 commits, and each adding their branch name to the file
along with an incremented counter.

```sh
git checkout main
git checkout -b example
for i in {1..5} ; do echo "example $i" > "$i" ; git add "$i" ; git commit -m "example $i" ; done
git checkout main
for i in {1..5} ; do echo  "main $i" > "$i" ; git add "$i" ; git commit -m "main $i" ; done
```

Let's check that we understand the structure:

```sh
git checkout main
ls
cat 4
git checkout example
ls
cat 4
git checkout main
```

We'll add an extra branch to represent where the main branch was before the
cherry-pick.

```sh
git checkout -b main_copy
git checkout main

git log --oneline --all --graph
```

To cherry-pick only the second-to-last commit in example we can do the
following.

```sh
git cherry-pick example~1
git status # Notice that we only have a conflict in 4
git cherry-pick --abort # We can always abort
```

To cherry-pick a range of commits we can do `git cherry-pick
<start-commit>^..<end-commit>`.

```sh
git cherry-pick example~3^..example~2 # I.e.
git status
```

From this point we resolve the commits one at a time in the same way we did with
rebases.

```sh
nvim 2 # Resolve the conflict as we see fit
git add 2
git cherry-pick --continue

nvim 3 # Resolve the conflict as we see fit
git add 3
git cherry-pick --continue

git log --oneline --all --graph
```

That was it, notice that only the selected commits were replayed on top of
`main_copy`. Also notice that they as always got new commit hashes and that the
original commits are still available in their original form as long as we can
find a reference to them.


Next Module: [Putting it all together for easier reviews](08_workflow.md)  
Back to [README.md](README.md)

# Cherry-pick

## Setting the stage

```sh
mkdir workshop
cd workshop
git init
git switch -c root
git commit -m "First commit" --allow-empty
git switch -c example
for i in {1..5} ; do echo "example $i" > "$i" ; git add "$i" ; git commit -m "example $i" ; done
git switch root
git switch -c main
for i in {1..5} ; do echo  "main $i" > "$i" ; git add "$i" ; git commit -m "main $i" ; done
```

## Understanding the repository

Another very useful git operation in the same vein as rebase is `git
cherry-pick`. It lets us take one or more commits and replay them on top of the
current branch.
 
In the setup we created two branches, `main` and `example`, each with 5 commits
adding a new file with the corresponding number.

Let's check that we understand the structure:

```sh
git log --oneline --all --graph
git switch example
git rebase main
git rebase --onto main 022895f example


git switch main
ls
cat 4
git switch example
ls
cat 4
git switch -d example~2
ls
cat 3
git switch main
```

## Performing a cherry-pick

We'll add an extra branch to represent where the main branch was before the
cherry-pick.

```sh
git switch -c main_copy
git switch main

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

That's it, notice that only the selected commits were replayed on top of
`main_copy`. Also notice that they as always got new commit hashes and that the
original commits are still available in their original form as long as we can
find a reference to them.

## Clean up

```sh
cd ..
rm -rf workshop
```

Next Module: [Putting it all together for easier reviews](08_workflow.md)  
Back to [README.md](README.md)

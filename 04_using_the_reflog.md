# Using the reflog

## Setting the stage

```sh
mkdir workshop
cd workshop
git init
git switch -c root
git commit -m "First commit" --allow-empty
git switch -c example
for i in {1..5} ; do git commit --allow-empty -m "example $i" ; done
git switch root
git switch -c main
for i in {1..5} ; do git commit --allow-empty -m "main $i" ; done
git log --oneline --all --graph

git branch -D example # Remove the example branch
git log --oneline --all --graph
```

We know that the commit tree is immutable, meaning that once a commit is created
it can not be changed or removed.

**Removing local branches is never scary! Deleting branches just removes a label.**

But how do we find our way back to a commit
if we removed all the refs pointing to it?

```sh
clear
git reflog
```

The reflog is a history of every commit HEAD has been pointing to since we cloned
/ created the repository, along with the operation performed. It only contains local changes, so if we remove the
local git repository we could lose the references to the commits.

Notice that the reflog reads from bottom to top, but is indexed from the latest
commit.

```sh
git switch -d HEAD@{7} # Go back to the commit we were at 7 changes ago
git log --oneline --all --graph
git switch -c example
git log --oneline --all --graph
```

Once we've learned to recover anything from the reflog our confidence using
git will skyrocket, since we can always recover whatever mistake we've made.

**Warning** only committed changes will appear in the reflog. Make sure to
always commit everything we care about (even if we don't feel happy with our
changes) before performing other git operations.

## Clean-up

```sh
cd ..
rm -rf workshop
```


Next Module: [Merge vs. Rebase](05_merge_vs_rebase.md)  
Back to [README.md](README.md)

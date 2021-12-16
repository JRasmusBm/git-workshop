 Using the reflog

We know that the commit tree is immutable, meaning that once a commit is created
it can not be changed or removed.

**Deleting branches just removes a label**

But how do you find your way back to a commit
if you removed all the refs pointing to it?

```sh
git reflog
```

The reflog is a history of every commit HEAD has been pointing to since you cloned
/ created the repository, along with the operation performed. It only contains local changes, so if you remove the
local git repository you could lose the references to the commits.

Notice that the reflog reads from bottom to top, but is indexed from the latest
commit.

```sh
git checkout HEAD@{1}
git log --oneline --all --graph
git checkout -b example
git log --oneline --all --graph
```

Once you've learned to recover anything from the reflog your confidence using
git will skyrocket, since you can always recover whatever mistake you've made.

**Warning** only committed changes will appear in the reflog. Make sure to
always commit everything you care about (even if you don't feel happy with your
changes) before performing other git operations.

Next Module: [Merge vs. Rebase](05_merge_vs_rebase.md)  
Back to [README.md](README.md)

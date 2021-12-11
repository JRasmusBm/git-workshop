# What is a commit

## Add a commit with a file

Now we're going to try to understand what a commit is. To do that we'll create
a new one.

```sh
mkdir src
nvim src/app.ts
```

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

## Add a second commit that updates the same file

```sh
nvim src/app.ts
```

Rename `a` to `b`

## What is a commit?

```sh
git show master
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

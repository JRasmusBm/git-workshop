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

Write a commit message (you can write whatever you would like).

```markdown
Add a script with information about a number a

**Why** was the change needed?

I conducting a git workshop and want to show how to make a commit. The
code itself has no significance.
```

When you close the file in your editor, given that you made any change to the
commit message, the contents of the file (minus the comments) will be added as
the commit message.

## Add a second commit that updates the same file

```sh
nvim src/app.ts
```

Rename `a` to `b`

```sh
git add src
git commit
```

```markdown
Rename `a` to `b`

**Why** was the change needed?

To show what happens when you create a commit with changes to an
existing file.
```

## What is a commit?

```sh
git log
git show main
git show main~1
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

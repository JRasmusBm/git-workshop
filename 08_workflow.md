# Putting it all together to code reviews easier

## Creating a repository

```sh
mkdir workshop
cd workshop
git init
git checkout -b root
git commit --allow-empty -m "First commit"
git checkout -b main
```

## Creating some files to work with

Let's say we're working in a TypeScript system that provides the core of a very
simple chat app.

```sh
mkdir src
nvim src/app.test.ts
```

```typescript
import { createApp } from "./app";

import { expect } from "chai";

describe("Application", () => {
  // ... A lot of other tests

  it("- chat adds a message to the chat log", async (): Promise<void> => {
    const app = createApp();
    app.chat("Hello world!");

    expect(app.getMessages()).to.deep.equal(["Hello world!"]);
  });

  // ... A lot of other tests
});
```

```sh
nvim src/app.ts
```

```typescript
export function createApp() {
  const state = { messages: [] };

  return {
    chat: (message: string) => {
      state.messages.push(message);
    },
    getMessages: () => {
      return state.messages;
    },
  };
}

export const app = createApp();
```

```sh
git add src
git commit -m "Build a basic chat messaging app"
```

## Setting the stage

So far we've only supported text messages, but now we have been asked to support
sending images as well. We don't have to deal with the upload itself, but we
need to store the URL to the uploaded image so that it can be displayed in the
chat flow.

We realize that we are going to have to refactor the code as we go along. In
order to make it easier for reviewers, we'd like to keep the refactoring
separate from the final review, so that the diffs in GitHub are easy to read and
work with.

We also know that we will be working on this during vacation when many in the
team are gone, so we'll have to wait roughly 2 weeks before we can get any
reviews.

We're lucky because there are tests, but with our change we're going to have to
add a number of more tests, and the test file is already very large and covers
a bunch of different functionality.

**What could we do to ensure that the code is easy to review when our teammates
come back?**

## Our solution

We decide immediately that we need to create a new test file, the big test file
since the old test file is getting difficult to work with.

We take the related tests and move them to a second file called
`message-handler.test.ts`.

```sh
git checkout -b add-support-for-uploads
nvim src/app.test.ts  # Copy the file and remove the test
nvim src/message-handler.test.ts  # Paste and remove unrelated tests
```

We're now going to add an additional test for the new upload behavior. We'll
also have to edit the existing test to adapt to the getMessages API change.

```typescript
it("- upload adds a message to the chat log", async (): Promise<void> => {
  //...
  expect(app.getMessages()).to.deep.equal([
    { type: "chat", text: "http://example.com" },
  ]);
  //...
});

it("- upload adds a message to the chat log", async (): Promise<void> => {
  const app = createApp();
  app.upload("http://example.com");

  expect(app.getMessages()).to.deep.equal([
    { type: "upload", url: "http://example.com" },
  ]);
});
```

We're also going to go add the new functionality to app.ts.

```sh
nvim src/app.ts
```

```typescript
    chat: (text: string) => {
      state.messages.push({ type: "chat", text });
    },
    upload: (url: string) => {
      state.messages.push({ type: "upload", url });
    },
```

We're now happy with our work. We support adding a url to the chat flow. We make
a commit.

```sh
git add src
git commit
```

```gitcommit
Add support for file uploads

**Why** was the change needed?

The chat app wasn't getting much traction and business thought that some images might make it more fun.
```

## Splitting the PR into two separate PRs

We open a pull request and notice in the diff view that it is very difficult to
see which test is the new test. 

```sh
git log --oneline --graph --all
git diff main
```

We want to split the changes up into two pull requests so that it's clear for
our colleagues what's going on. We also notice that we forgot to remove the
tests from the original test file.


```sh
git checkout -b extract-message-handler-tests
git reset --hard main
git log --oneline --graph --all
```

We repeat the steps of the first commit.

```sh
nvim src/app.test.ts  # Copy the whole file, and (!) remove the test
nvim src/message-handler.test.ts  # Paste and remove unrelated tests
git add src
git commit
```

```gitcommit
Extract separate test file for message handler

**Why** was the change needed?

We will want to iterate on the message handler creating new functionality. The
app.test.ts is already very large, so it makes se nse to extract a new test
file.
```

```sh
git log --oneline --graph --all
git checkout add-support-for-uploads
git log --oneline --graph --all
git rebase extract-message-handler-tests
nvim src/message-handler.test.ts # Resolve the conflict
git add src
git rebase --continue
git log --oneline --graph --all
```

We can now create a separate PR for the branch extract-message-handler-tests. We
go to our original PR and change its base to target
extract-message-handler-tests. We then get to see a nicer diff for our feature:

```sh
git diff extract-message-handler-tests
```

This means that when our colleagues come back from vacation, they can review the
file rename first, and then the feature change.

## Clean-up

```sh
cd ..
rm -rf
```

Back to [README.md](README.md)

# Setting up a git repository

## Creating a repository

```sh
clear;
mkdir workshop;
cd workshop;
git status;
git init;
git status;
ls -a;
rm -rf .git;
git status;
git init;
```

## Making a commit

```sh
git commit --allow-empty -m 'First commit' # Blows up!
git config --global --edit # Get comfortable editing this file! VCS!
git commit --allow-empty -m 'First commit' # Now it works (opens editor)
```

### Choosing an Editor

Installing the editor (Say I preferred `nano`):

```gitconfig
[core]
    editor = nano
```

```sh
sudo apt install nano -y
git config --global --edit # Opens in nano 😱
```

```gitconfig
[core]
    editor = nvim
#   editor = code --wait
```

## Writing a good commit message

I actually don't recommend using the -m flag on commits for anything that you
intend to push into a shared branch. It leads you into a false sense that commit
messages should be short, instead of informational. The first line has to
be lower than 56 characters.

### Setting up a template

When getting in the habit of writing better commit messages it's very helpful to
get prompted with some general questions about the commit, so that one gets
motivated to add good information when making the commit.

```gitconfig
[commit]
  template = ~/.gitmessage
```

```sh
nvim ~/.gitmessage
```

```markdown
**Why** was the change needed?

**What** does the commit do?

Are there any **concerns** or **side-effects** worth mentioning?
```

**Discussion on commit messages?**

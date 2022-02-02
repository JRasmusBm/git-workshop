# Editing Git configuration

## Creating a repository

We're going to start by creating a new folder `workshop/` inside which all the
experimentation of this workshop will live. We will remove this folder / docker
container after the workshop, so where we put it is not important.

```sh
clear;
mkdir workshop;
cd workshop;
ls -a;
```

Before we're able to run any `git` commands we first have to initialize git.

```sh
git status; # Read the error message
git init;
git status; # No error message
ls -a;
```

Notice that the `git init` command added a new hidden folder `.git` in the root
of the repository. That folder **is** the repository. If we remove that folder,
it is the same as completely removing `.git` from the folder. If we don't have
a back-up this cannot be undone!

```sh
rm -rf .git;
git status; # Same error message as before
git init;
git status; # No error message
```

## Making a commit

It's good practice when starting a new repository to choose the starting branch
name and make an empty first commit. That first commit allows us to have
a reference to a state before we started making adding any changes, which is
very useful when running any kind of branch operations later on.

```sh
git checkout -b 'main' # Set initial branch name, more about this later
git commit --allow-empty -m 'First commit' # Blows up!
```

If we are not on a fresh `git config` it is possible that the above command
went just fine, if so, it's still recommended to follow the configuration steps to see if we would
like to make any of the changes suggested.

We now open up the global git configuration file. In my experience it is
paramount to get comfortable editing the git configuration, since doing so will
allow us to tailor our git experience to our own workflows, which will make
us much more effective. It's also strongly recommended to add our configuration
files themselves to git, so that we can quickly get back up to speed when
switching systems.

The following command will open our global git settings. On Ubuntu these are
stored in `"$HOME/.gitconfig"`.

```sh
git config --global --edit
```

### Choosing an Editor

One of the biggest stumbling blocks preventing many people from using git is the
fact that it defaults to using an editor that they're not comfortable working
with. As our first change we will demonstrate how to change the default git
editor, which will impact all future changes.

Installing the editor (Say we preferred `nano`):

```sh
sudo apt install nano -y
```

We then edit the git config to set the editor to our favorite editor. For
demonstration purposes we switch the editor to use `nano`. We can also for
example set up vscode as our git editor, included here because it is a very
powerful and popular editor.

```sh
git config --global --edit
```

```gitconfig
[core]
    editor = nano
#   editor = nvim
#   editor = code --wait
```

We will now see that when we run the following command again, it now opens in the
editor we chose. That's neat! :smile: We are going to put it back to use `nvim`
(neovim) though, since that is my preferred editor.

```sh
git config --global --edit # Opens in nano 😱
```

```gitconfig
[core]
#   editor = nano
    editor = nvim
#   editor = code --wait
```

### Adding user information to the git config

Whenever we make a commit, our user information is added to the commit
metadata as `name <email>`. We need to set these values in our git config in
order to be able to make commits. In this workshop the actual values don't
matter, but otherwise it's good practice to be consistent with our git identity across
an organization. If we want to use a different `name` and `email` for a specific
repository we can set that information with `git config --edit`.

```sh
git config --global --edit
```

```gitconfig
[user]
    name = git-user
    email = git-user@example.com
```

Now we can try to create the commit again. (We can ignore this step if it worked the
first time).

```sh
git commit --allow-empty -m 'First commit' # Should work now
```

## Clean-up

```sh
cd ..
rm -rf workshop
```

## Writing a good commit message

Except for the first commit, I strongly advice against using the -m flag for commits that we
intend to push into a shared branch. It leads a false sense that commit
messages should be short, instead of informational. The recommended format for
a git commit message is as follows:

```gitcommit
# 50-character subject line 
#
# 72-character wrapped longer description. This should answer:
#
# * Why was this change necessary?
# * How does it address the problem?
# * Are there any side effects?
#
# Include a link to the ticket, if any.
#
# Add co-authors if we worked on this code with others:
#
# Co-authored-by: Full Name <email@example.com>
# Co-authored-by: Full Name <email@example.com>
```

It is good practice to use imperative language in commit messages. Avoid past
tense when necessary and instead finish the sentence `"This commit will ..."`.

### Setting up a template

When getting in the habit of writing better commit messages it's very helpful to
set up a commit message template. When used properly this can be just the prompt
we need to get started writing great commit messages.

```gitconfig
[commit]
  template = ~/.gitmessage
```

```sh
nvim ~/.gitmessage
```

My current message template can be found
[here](https://github.com/JRasmusBm/dotfiles/blob/main/git/gitmessage). The
strange `<++>` symbols are editor-specific markers that I use to jump around the commit
message.

Useful read: [Thoughtbot Blog: Write good commit messages by blaming others](https://thoughtbot.com/blog/write-good-commit-messages-by-blaming-others)

**Discussion on commit messages?**

Next Module: [What is a commit? How do I make a good one?](02_what_is_a_commit.md)  
Back to [README.md](README.md)

# Intermediate Git Workshop

Git is one of the main tools we as developers use everyday. Many of us were
never really taught how git works, but rather learned enough commands off by
heart to be dangerous.

This repository provides the basis for a git workshop that I have run a number
of times at various companies where I've worked, and which has always been
appreciated.

Though we exclusively use the git command line in the workshop I
recommend getting intimately familiar with the git integration available in your
editor of choice.

- [tpope/vim-fugitive](https://github.com/tpope/vim-fugitive) for vim/neovim
- [GitLens](https://marketplace.visualstudio.com/items?itemName=eamodio.gitlens)
  and [Git Graph](https://marketplace.visualstudio.com/items?itemName=mhutchie.git-graph) for Visual Studio Code.

The workshop contains the following modules:

0. (If you don't want to run on your own system): [Setting up a docker container to work in](00_setup_docker.md)
1. [Creating a basic git config](01_basic_git_config.md)
2. [What is a commit? How do I make a good one?](02_what_is_a_commit.md)
3. [What is a branch? How do I use them effectively?](03_what_is_a_branch.md)
4. [How do I recover from mistakes?](04_using_the_reflog.md)
5. [Merge vs. Rebase](05_merge_vs_rebase.md)
6. [Maintaining a clean history](06_interactive_rebase.md)
7. [Cherry-pick](07_cherry_pick.md)
8. [Putting it all together for easier reviews](08_workflow.md)

I will of course share nice configuration settings and tips along the way. If
the workshop takes longer than the alotted time I'll invite to a follow-up
meeting later on.

Welcome to my git workshop! :smile:

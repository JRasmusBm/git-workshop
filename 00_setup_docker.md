# Setting up the Docker container

## Creating the container

```sh
docker run --name=git-workshop -it ubuntu /bin/bash;
exit
docker start -ai git-workshop
# docker rm git-workshop;
set -o vi;
```

## Setting up sudo

```sh
apt update -y;
apt install -y sudo;
rm -rf /var/lib/apt/lists/*;
passwd; # set root password
```

## Add user

```sh
adduser git-user; # create user
usermod -a -G sudo git-user;
su - git-user;
set -o vi;
```

## Installing git and (n)vim

```sh
sudo apt update -y;
sudo apt install -y git neovim;
```

## Setting up a bin directory for aliases

```sh
mkdir bin
export PATH="$PATH:$HOME/bin"
```

### v == vim

```sh
nvim "$HOME/bin/v"
```

```sh
#!/bin/sh

nvim "$@"
```

### g == git

```sh
nvim "$HOME/bin/g"
```

```sh
#!/bin/sh

if test -n "$1" ; then
    git "$@"
else
    git status
fi
```

### c == clear

```sh
nvim "$HOME/bin/c"
```

```sh
#!/bin/sh

clear
```

### chmod

```sh
chmod +x bin/*
```

**Comments on my setup?**

1. [Creating a basic git config](01_basic_git_config.md)

# Setting up the Docker container

## Creating the container

```sh
docker run --name=git-workshop -it ubuntu /bin/bash;
docker rm git-workshop;
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

In `"$HOME/bin/v"`:

```sh
#!/bin/sh

nvim "$@"
```

### g == git

In `"$HOME/bin/g"`

```sh
#!/bin/sh

if test -n "$@" ; then
    git "$@"
else
    git status
fi
```

```sh
chmod +x bin/*
```

### c == clear

```sh
#!/bin/sh

clear
```

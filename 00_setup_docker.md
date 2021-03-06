# Setting up the Docker container

When on my own system, I work in an environment that is heavily customized to
fit my own use-case. This is great when developing but distracting when
teaching. For this reason I run the entire workshop in a docker container that
I create from scratch for the purpose.

If possible, I recommend that you follow the workshop on your own system. Though
it means that you might have to adapt some commands, you will be able to
immediately start using any tips or tricks learned in your daily work.

## Creating the container

We set up a docker container called `git-workshop`. When doing so we are dropped
into the container with root access. For demonstration purposes we then exit the
container and show how to re-attach to the same container. We also set up vi
keybindings in the shell (instead of Emacs which is the default).

```sh
docker run --name=git-workshop -it ubuntu /bin/bash;
exit
docker start -ai git-workshop
# docker rm git-workshop;
```

## Setting up sudo

Here we install the sudo command and set up a root password. Enter any password
you would like to have for the sudo command when prompted.

```sh
apt update -y;
apt install -y sudo;
rm -rf /var/lib/apt/lists/*;
passwd; # set root password
```

## Add user

Here we set up a normal user called `git-user` and give it access to sudo. To
set up the user, we choose a password, press enter through the rest of
the options (they are irrelevant for the workshop) and then submit with the
letter `y`.

```sh
adduser git-user; # create user
usermod -a -G sudo git-user;
su - git-user;
```

## Installing git and (n)vim

The next step is to install some dependencies. For this workshop we need an
editor (I use neovim) and git.

```sh
sudo apt update -y;
sudo apt install -y git neovim;
```

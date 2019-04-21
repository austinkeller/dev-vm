# My disposable development environment

My dev machine is a Windows 10 base with a Virtualbox linux VM where I do most
 of my work. It's reassuring to have a development environment that, if you hose,
 is just a couple of commands away from being refreshed like new.

### Dependencies

* [Vagrant](https://www.vagrantup.com)
* [Virtualbox](https://www.virtualbox.org)

### Usage

`vagrant up` to provision the machine. To ssh in from the host machine, run 
 `vagrant ssh-config` and add that as an entry to your `~/.ssh/config`.

#### Note

Some assumptions are made, like that there are `C:\`, `D:\`, and `G:\` drives
 available on the host. This also copies in a public key that would need to be
 replaced with a key for which you have access to the corresponding private key.

### Future Directions

Moving as much functionality as possible to an Ansible script would give me (1) a chance to learn an important DevOps tool and (2) allow me to effortlessly provision a comfortable development environment not just locally, but on any remote provider.

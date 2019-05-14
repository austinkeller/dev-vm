# My disposable development environment

My dev machine is a Windows 10 base with a Virtualbox linux VM where I do most
 of my work. It's reassuring to have a development environment that, if you hose,
 is just a couple of commands away from being refreshed like new.

### Dependencies

* [Vagrant](https://www.vagrantup.com)
* [Virtualbox](https://www.virtualbox.org)

### Usage

#### Windows host

Install Virtualbox and Vagrant on Windows and run `vagrant up` from the command-line/powershell within the repo directory. I like to place the repo in my %USERPROFILE% directory and rename it to `.vagrant-dev-vm`.

To access the dev machine from WSL, add the output of `vagrant ssh-config` to your `~/.ssh/config`.

#### Mac or Linux host

I use Windows as my exclusive host OS so I haven't tested on Mac or Linux. But you can install Vagrant and Virtualbox on each and follow a similar (probably even simpler) workflow to create nice isolated dev environments. You'll just need to adapt the host directory mount paths. And pulling ssh-config info won't be necessary since you'll be using the native terminal for all operations.

#### Note

Some assumptions are made, like that there are `C:\`, `D:\`, and `G:\` drives
 available on the host. This also copies in a public key that would need to be
 replaced with a key for which you have access to the corresponding private key.

### Future Directions

Moving as much functionality as possible to an Ansible script would give me (1) a chance to learn an important DevOps tool and (2) allow me to effortlessly provision a comfortable development environment not just locally, but on any remote provider.

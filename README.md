# DevOps Vagrant

It is my effort to create a vagrant file for DevOps tools. The tools that are most commonly in use, and I am familiar with.

It can be used for a basic workstation setup. It can also be sued for lab environemnts, demo's, tutorials and workshops. It uses a ubuntu 18.04 distro. You can find the following tools here:

- Python 3.6.9
- ansible 2.9.27
- aws-cli/2.7.16 (latest on 19.07.2022)  -- **You need to add your creds under ~/.aws/**
-  minikube version: v1.26.0
    - kubernetes 1.24.1
- Helm v 3.9.1

## Install Pre-requisites

Ensure you have vagrant installed. Tested on Windows 11

https://www.vagrantup.com/docs/installation/

### Arch
```
sudo pacman -S vagrant
```

### Ubuntu
```
sudo apt-get install vagrant
```

## Run it

Clone this repo then:

```
vagrant up
```

## SSH into the VM
```
vagrant ssh
```

## Access your code inside the VM

We automatically mount `/shared/vagrant/data` into `/home/vagrant/data`.

For example, you may want to `git clone` some kubernetes manifests into `/shared/vagrant/data` on your host-machine, then you can access them in the vagrant machine.

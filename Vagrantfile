# -*- mode: ruby -*-
# vi: set ft=ruby :

VAGRANTFILE_API_VERSION = "2"
Vagrant.require_version ">= 2.0.0"

# just a single node is required
NODES = ENV['NODES'] || 1

# Memory & CPUs
MEM = ENV['MEM'] || 4096
CPUS = ENV['CPUS'] || 2

# User Data Mount
#SRCDIR = ENV['SRCDIR'] || "/home/"+ENV['USER']+"/test"
SRCDIR = ENV['SRCDIR'] || "/shared/vagrant/data"
DSTDIR = ENV['DSTDIR'] || "/home/vagrant/data"

# Management
GROWPART = ENV['GROWPART'] || "true"

# Minikube Variables
KUBERNETES_VERSION = ENV['KUBERNETES_VERSION'] || "1.24.1"

# Common installation script
$installer = <<SCRIPT
#!/bin/bash

# Update apt and get dependencies
sudo apt-get -y update
sudo apt-mark hold grub
sudo apt-mark hold grub-pc
sudo apt-get -y upgrade
sudo apt-get install -y zip unzip curl wget socat ebtables git vim
sudo apt install -y qemu libvirt-daemon-system libvirt-clients libxslt-dev libxml2-dev libvirt-dev zlib1g-dev ruby-dev ruby-libvirt ebtables dnsmasq-base


SCRIPT

$docker = <<SCRIPT
#!/bin/bash

#curl -fsSL https://apt.dockerproject.org/gpg | sudo apt-key add -
#sudo apt-add-repository "deb https://apt.dockerproject.org/repo ubuntu-xenial main"
#sudo apt-get install -y docker-engine=17.03.1~ce-0~ubuntu-xenial

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
sudo apt-get -y update
sudo apt-get install -y docker-ce
sudo systemctl start docker

sudo usermod -a -G docker vagrant

SCRIPT

$growpart = <<SCRIPT
#!/bin/bash

if [[ -b /dev/vda3 ]]; then
  sudo growpart /dev/vda 3
  sudo resize2fs /dev/vda3
elif [[ -b /dev/sda3 ]]; then
  sudo growpart /dev/sda 3
  sudo resize2fs /dev/sda3
fi

SCRIPT

$minikubescript = <<SCRIPT
#!/bin/bash

#Install minikube
echo "Downloading Minikube"
curl -q -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 2>/dev/null
chmod +x minikube
sudo mv minikube /usr/local/bin/

#Install kubectl
echo "Downloading Kubectl"
curl -q -Lo kubectl https://storage.googleapis.com/kubernetes-release/release/v${KUBERNETES_VERSION}/bin/linux/amd64/kubectl 2>/dev/null
chmod +x kubectl
sudo mv kubectl /usr/local/bin/

# Install crictl
curl -qL https://github.com/kubernetes-sigs/cri-tools/releases/download/v1.16.1/crictl-v1.16.1-linux-amd64.tar.gz 2>/dev/null | tar xzvf -
chmod +x crictl
sudo mv crictl /usr/local/bin/

#Install stern
# TODO: Check sha256sum
echo "Downloading Stern"
curl -q -Lo stern https://github.com/wercker/stern/releases/download/1.10.0/stern_linux_amd64 2>/dev/null
chmod +x stern
sudo mv stern /usr/local/bin/

#Install kubecfg
# TODO: Check sha256sum
echo "Downloading Kubecfg"
curl -q -Lo kubecfg https://github.com/ksonnet/kubecfg/releases/download/v0.9.0/kubecfg-linux-amd64 2>/dev/null
chmod +x kubecfg
sudo mv kubecfg /usr/local/bin/

#Install Helm
echo "Downloading helm v 3"
sudo curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3
sudo chmod +x get_helm.sh
sudo ./get_helm.sh

#Setup minikube
echo "127.0.0.1 minikube minikube." | sudo tee -a /etc/hosts
mkdir -p $HOME/.minikube
mkdir -p $HOME/.kube
touch $HOME/.kube/config

export KUBECONFIG=$HOME/.kube/config

# Permissions
sudo chown -R $USER:$USER $HOME/.kube
sudo chown -R $USER:$USER $HOME/.minikube

export MINIKUBE_WANTUPDATENOTIFICATION=false
export MINIKUBE_WANTREPORTERRORPROMPT=false
export MINIKUBE_HOME=$HOME
export CHANGE_MINIKUBE_NONE_USER=true
export KUBECONFIG=$HOME/.kube/config

# Disable SWAP since is not supported on a kubernetes cluster
sudo swapoff -a

## Start minikube
sudo -E minikube start -v 4 --vm-driver none --kubernetes-version v${KUBERNETES_VERSION} --bootstrapper kubeadm

## Addons
sudo -E minikube addons  enable ingress

## Configure vagrant clients dir

printf "export MINIKUBE_WANTUPDATENOTIFICATION=false\n" >> /home/vagrant/.bashrc
printf "export MINIKUBE_WANTREPORTERRORPROMPT=false\n" >> /home/vagrant/.bashrc
printf "export MINIKUBE_HOME=/home/vagrant\n" >> /home/vagrant/.bashrc
printf "export CHANGE_MINIKUBE_NONE_USER=true\n" >> /home/vagrant/.bashrc
printf "export KUBECONFIG=/home/vagrant/.kube/config\n" >> /home/vagrant/.bashrc
printf "source <(kubectl completion bash)\n" >> /home/vagrant/.bashrc

# Permissions
sudo chown -R $USER:$USER $HOME/.kube
sudo chown -R $USER:$USER $HOME/.minikube

# Enforce sysctl
sudo sysctl -w vm.max_map_count=262144
sudo echo "vm.max_map_count=262144" | sudo tee -a /etc/sysctl.d/90-vm_max_map_count.conf

SCRIPT


$ansiblescript = <<SCRIPT
#!/bin/bash

#Install ansible
echo "Configuring ansible repo"
sudo add-apt-repository --yes --update ppa:ansible/ansible
sudo apt install ansible -y

SCRIPT

$awscli = <<SCRIPT

## Configure AWS client
mkdir -p $HOME/.aws
touch $HOME/.aws/config
touch $HOME/.aws/credentials
printf "\n\n ## AWS Alias ##\n"
printf "alias aws='docker run --rm -it -v ~/.aws:/root/.aws -v $(pwd):/aws amazon/aws-cli'\n" >> /home/vagrant/.bashrc

SCRIPT

$terraform = <<SCRIPT

## Install Terraform
curl -fsSL https://apt.releases.hashicorp.com/gpg | sudo apt-key add -
sudo apt-add-repository "deb [arch=amd64] https://apt.releases.hashicorp.com $(lsb_release -cs) main"
sudo apt update
sudo apt-get install terraform

wget https://github.com/gruntwork-io/terragrunt/releases/download/v0.38.6/terragrunt_linux_amd64
sudo mv terragrunt_linux_amd64 /usr/local/bin/terragrunt
sudo chmod +x /usr/local/bin/terragrunt

SCRIPT
# required_plugins = %w(vagrant-sshfs vagrant-vbguest vagvagr rant-libvirt)
required_plugins = %w(vagrant-sshfs vagrant-vbguest)

required_plugins.each do |plugin|
  need_restart = false
  unless Vagrant.has_plugin? plugin
    system "vagrant plugin install #{plugin}"
    need_restart = true
  end
  exec "vagrant #{ARGV.join(' ')}" if need_restart
end


def configureVM(vmCfg, hostname, cpus, mem, srcdir, dstdir)

  vmCfg.vm.box = "roboxes/ubuntu1804"

  vmCfg.vm.hostname = hostname
  vmCfg.vm.network "private_network", ip: "192.168.1.10", :model_type => "virtio", :autostart => true
/
  for i in 32000..33000
    vmCfg.vm.network :forwarded_port, guest: i, host: i
  end
/

  vmCfg.vm.synced_folder '.', '/vagrant', disabled: true
  # sync your laptop's development with this Vagrant VM
  vmCfg.vm.synced_folder srcdir, dstdir, type: "rsync", rsync__exclude: ".git/", create: true

  # First Provider - Libvirt
  vmCfg.vm.provider "libvirt" do |provider, override|
    provider.memory = mem
    provider.cpus = cpus
    provider.driver = "kvm"
    provider.disk_bus = "scsi"
    provider.machine_virtual_size = 64
    provider.video_vram = 64


    override.vm.synced_folder srcdir, dstdir, type: 'sshfs', ssh_opts_append: "-o Compression=yes", sshfs_opts_append: "-o cache=no", disabled: false, create: true
  end

  vmCfg.vm.provider "virtualbox" do |provider, override|
    provider.memory = mem
    provider.cpus = cpus
    provider.customize ["modifyvm", :id, "--cableconnected1", "on"]

    override.vm.synced_folder srcdir, dstdir, type: 'virtualbox', create: true
  end

  # ensure docker is installed # Use our script so we can get a proper support version
  vmCfg.vm.provision "shell", inline: $docker, privileged: false
  # Script to prepare the VM
  vmCfg.vm.provision "shell", inline: $installer, privileged: false
  vmCfg.vm.provision "shell", inline: $growpart, privileged: false if GROWPART == "true"
  vmCfg.vm.provision "shell", inline: $minikubescript, privileged: false, env: {"KUBERNETES_VERSION" => KUBERNETES_VERSION}
  vmCfg.vm.provision "shell", inline: $ansiblescript, privileged: false
  vmCfg.vm.provision "shell", inline: $awscli, privileged: false
  vmCfg.vm.provision "shell", inline: $terraform, privileged: false

  return vmCfg
end

# Entry point of this Vagrantfile
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.vbguest.auto_update = false

  1.upto(NODES.to_i) do |i|
    hostname = "minikube-vagrant-%02d" % [i]
    cpus = CPUS
    mem = MEM
    srcdir = SRCDIR
    dstdir = DSTDIR

    config.vm.define hostname do |vmCfg|
      vmCfg = configureVM(vmCfg, hostname, cpus, mem, srcdir, dstdir)
    end
  end

end

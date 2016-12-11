Vagrant.configure("2") do |obj|
	obj.vm.define "ChefServer" do |devbox|
		devbox.vm.box = "DevBox_Test1"
		devbox.vm.box_url = "http://puppet-vagrant-boxes.puppetlabs.com/ubuntu-server-12042-x64-vbox4210-nocm.box"
		devbox.vm.network "private_network", ip: "192.168.33.11"
		devbox.vm.network "public_network"
		devbox.ssh.forward_agent = true
		devbox.vm.synced_folder "/Users/soumitrakar/vagrant_DevBox/DevBox_data", "/vagrant_data", create:true
		devbox.vm.provider "virtualbox" do |virbox| 
  			# Don't boot with headless mode
  			virbox.gui = false
  			# Use VBoxManage to customize the VM. For example to change memory:
  			virbox.customize ["modifyvm", :id, "--memory", "1024"]
		end
#		devbox.vm.provision :shell, :path => "../git-hub/setup_mysite_env.sh"
#		devbox.vm.provision "chef_solo" do |chef|
#                        chef.cookbooks_path = "/Users/soumitrakar/Google\ Drive/Chef/chef-repo/cookbooks/"
#                        chef.add_recipe "yum"
#                end
	end 
	  
	obj.vm.define "AWSLab" do |devbox|
		devbox.vm.box = "DevBox_Test2"
                devbox.vm.box_url = "http://puppet-vagrant-boxes.puppetlabs.com/ubuntu-server-12042-x64-vbox4210-nocm.box"
                devbox.vm.network "private_network", ip: "192.168.33.12"
                devbox.vm.network "public_network"
                devbox.ssh.forward_agent = true
                devbox.vm.synced_folder "/Users/soumitrakar/vagrant_DevBox/DevBox_data", "/vagrant_data", create:true
               	devbox.vm.provider "virtualbox" do |virbox|
                	# Don't boot with headless mode
                	virbox.gui = false
                	# Use VBoxManage to customize the VM. For example to change memory:
                	virbox.customize ["modifyvm", :id, "--memory", "512"]
        	end
	end

        obj.vm.define "ChefWorkstation" do |devbox|
                devbox.vm.box = "DevBox_Test3"
                devbox.vm.box_url = "http://puppet-vagrant-boxes.puppetlabs.com/ubuntu-server-12042-x64-vbox4210-nocm.box"
                devbox.vm.network "private_network", ip: "192.168.33.13"
                devbox.vm.network "public_network"
                devbox.ssh.forward_agent = true
                devbox.vm.synced_folder "/Users/soumitrakar/vagrant_DevBox/DevBox_data", "/vagrant_data", create:true
                devbox.vm.provider "virtualbox" do |virbox|
                        # Don't boot with headless mode
                        virbox.gui = false
                        # Use VBoxManage to customize the VM. For example to change memory:
                        virbox.customize ["modifyvm", :id, "--memory", "512"]
                end
        end
	
        obj.vm.define "Jenkins" do |devbox|
                devbox.vm.box = "DevBox_Test4"
                devbox.vm.box_url = "http://puppet-vagrant-boxes.puppetlabs.com/ubuntu-server-12042-x64-vbox4210-nocm.box"
                devbox.vm.network "private_network", ip: "192.168.33.14"
                devbox.vm.network "public_network"
                devbox.ssh.forward_agent = true
                devbox.vm.synced_folder "/Users/soumitrakar/vagrant_DevBox/DevBox_data", "/vagrant_data", create:true
                devbox.vm.provider "virtualbox" do |virbox|
                        # Don't boot with headless mode
                        virbox.gui = false
                        # Use VBoxManage to customize the VM. For example to change memory:
                        virbox.customize ["modifyvm", :id, "--memory", "1024"]
                end
        end
	
	obj.vm.define "ChefClient" do |devbox|
                devbox.vm.box = "DevBox_Test5"
                devbox.vm.box_url = "http://puppet-vagrant-boxes.puppetlabs.com/ubuntu-server-12042-x64-vbox4210-nocm.box"
                devbox.vm.network "private_network", ip: "192.168.33.15"
                devbox.vm.network "public_network"
                devbox.ssh.forward_agent = true
                devbox.vm.synced_folder "/Users/soumitrakar/vagrant_DevBox/DevBox_data", "/vagrant_data", create:true
                devbox.vm.provider "virtualbox" do |virbox|
                        # Don't boot with headless mode
                        virbox.gui = false
                        # Use VBoxManage to customize the VM. For example to change memory:
                        virbox.customize ["modifyvm", :id, "--memory", "512"]
                end
        end
	obj.vm.define "AllInOne" do |devbox|
                devbox.vm.box = "DevBox_Test6"
                devbox.vm.box_url = "https://oss-binaries.phusionpassenger.com/vagrant/boxes/latest/ubuntu-12.04-amd64-vbox.box"
                devbox.vm.network "private_network", ip: "192.168.33.16"
                devbox.vm.network "public_network"
                devbox.ssh.forward_agent = true
                devbox.vm.synced_folder "/Users/soumitrakar/vagrant_DevBox/DevBox_data", "/vagrant_data", create:true
                devbox.vm.provider "virtualbox" do |virbox|
                        # Don't boot with headless mode
                        virbox.gui = false
                        # Use VBoxManage to customize the VM. For example to change memory:
                        virbox.customize ["modifyvm", :id, "--memory", "1024"]
                end
	end

end

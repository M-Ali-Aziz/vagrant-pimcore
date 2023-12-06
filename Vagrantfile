# -*- mode: ruby -*-
# vi: set ft=ruby :

#============================= VARIABLES =================================
# BOX use Ubuntu 23.04 LTS 64 bit
BOX = "ubuntu/lunar64"

# Private network ip number
IP = "192.168.56.21"

# Shared folder to the guest VM
HOST_FOLDER_PROJECT = "project"
GUEST_FOLDER_PROJECT = "/var/www/project"

# Customize VB
VB_NAME = "ehl_pimcore_6_to_11"
MEMORY = 5000
CPU = 5

# Provisioning paths
# SHELL_PATH = "provisioning/provision.sh"
ANSIBLE_PLAYBOOK_PATH = "provision/ansible/playbook.yml"

#============================= CONFIGURATION =================================
# All Vagrant configuration is done below.
Vagrant.configure("2") do |config|
  # The most common configuration options are documented and commented below.
  # For a complete reference, please see the online documentation at
  # https://docs.vagrantup.com.

  # Every Vagrant development environment requires a box. You can search for
  # boxes at https://vagrantcloud.com/search.
  config.vm.box = BOX

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  config.vm.network "private_network", ip: IP

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.
  config.vm.synced_folder HOST_FOLDER_PROJECT, GUEST_FOLDER_PROJECT, owner: "vagrant", group: "www-data", mount_options:['dmode=775', 'fmode=775']
#   config.vm.synced_folder HOST_FOLDER_PROJECT, GUEST_FOLDER_PROJECT, owner: "www-data", group: "www-data", mount_options:['dmode=777', 'fmode=777']
#   config.vm.synced_folder HOST_FOLDER_PROJECT, GUEST_FOLDER_PROJECT, type: "nfs", mount_options: ['actimeo=2']
  config.vm.synced_folder ".", "/vagrant", disabled: false

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # View the documentation for the provider you are using for more
  # information on available options.
  config.vm.provider "virtualbox" do |vb|
    # Virtual Machine Name
    vb.name = VB_NAME
    # Customize the amount of memory & cpu on the VM:
    vb.memory = MEMORY
    vb.cpus = CPU
  end

  # Enable provisioning with a shell script. Additional provisioners such as
  # Ansible, Chef, Docker, Puppet and Salt are also available. Please see the
  # documentation for more information about their specific syntax and use.
  # config.vm.provision :shell, path: SHELL_PATH
  ### ANSIBLE
  # Run Ansible from the Vagrant VM
  config.vm.provision :ansible_local do |ansible|
    ansible.playbook = ANSIBLE_PLAYBOOK_PATH
    ansible.extra_vars = { ansible_python_interpreter: "/usr/bin/python3" }
#     ansible.verbose = true
#     ansible.tags = ["common","vhost"]
  end
end

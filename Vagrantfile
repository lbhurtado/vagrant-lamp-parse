# -*- mode: ruby -*-
# vi: set ft=ruby :

BOX = "hashicorp/precise64"
# BOX = "boxesio/wheezy64-ansible"
HOSTNAME = "lamp"
DOMAIN = "local.com"
RAM = "512"
CPUs = "1"

$fix_vmware_tools_script = <<SCRIPT
sed -i.bak 's/answer AUTO_KMODS_ENABLED_ANSWER no/answer AUTO_KMODS_ENABLED_ANSWER yes/g' /etc/vmware-tools/locations
sed -i 's/answer AUTO_KMODS_ENABLED no/answer AUTO_KMODS_ENABLED yes/g' /etc/vmware-tools/locations
SCRIPT

Vagrant.configure(2) do |config|
  config.vm.box = BOX

  config.vm.network "forwarded_port", guest: 80, host: 8080

  config.vm.provider "vmware_fusion" do |v|
    v.gui = true
    v.vmx["memsize"] = RAM
    v.vmx["numvcpus"] = CPUs
  end

  ENV['VAGRANT_DEFAULT_PROVIDER'] = 'vmware_fusion'
  ENV['VAGRANT_VMWARE_CLONE_DIRECTORY'] = '~/Projects/DevOps/Clone/'

  config.vm.provision "shell", inline: $fix_vmware_tools_script
  config.vm.provision "shell", inline: "echo VAGRANT_VMWARE_CLONE_DIRECTORY=" + ENV['VAGRANT_VMWARE_CLONE_DIRECTORY']
  
  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "playbook.yml"
    ansible.sudo = "true"
    ansible.verbose = "vv"
  end
end

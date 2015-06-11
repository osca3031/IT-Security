# -*- mode: ruby -*-
# vi: set ft=ruby :

#ENV['DEFAULT_VAGRANT_PROVIDER'] = 'libvirt'

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  # Every Vagrant virtual environment requires a box to build off of.
  config.vm.box = "debian/jessie64"

  config.vm.provider :virtualbox do |v|
#    libvirt.host = ""
#    libvirt.nested = true
     v.gui = true
  end

  config.vm.synced_folder ".", "/vagrant"

  config.vm.provision "shell", inline:
    $script = <<SCRIPT
    sudo rm /etc/apt/apt.conf
    echo 'Acquire::http::Proxy "http://aptproxy:3142";' > /etc/apt/apt.conf
SCRIPT


  config.vm.provision "shell", inline: 
    $script = <<SCRIPT
    echo Shell provisioning...
    sudo apt-get install puppet -y
    puppet module install puppetlabs-apache

SCRIPT
  config.vm.provision "shell", path: "installModules.sh"

  config.vm.provision "puppet" do |puppet|
    puppet.manifests_path = "manifests"
    puppet.manifest_file = "default.pp"
    puppet.options = '--test'    # see the output
    puppet.synced_folder_type = ""
    puppet.facter = {
      "user" => "user",
      "password" => "passwd",
    }
  end
end

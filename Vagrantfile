# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

# Select the FIRST-priority interface on the dedicated server to bridge.
VM_INTERFACES = %x( VBoxManage list bridgedifs | grep ^Name ).gsub(/Name:\s+/, '').split("\n")
VM_MACS = %x( VBoxManage list bridgedifs | grep HardwareAddress | awk '{print $2}' | tr -d ':' ).split("\n")

# VBOX Name
VBOX_NAME = File.basename(Dir.getwd)

# Hostname for guest system
VBOX_HOSTNAME = "#{VBOX_NAME}-#{VM_MACS[0]}"

# Extra public/bridged ethernet interface for guest system
VBOX_NETIF = VM_INTERFACES[0]

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  # Using Ubuntu 14.04.1.
  #
  config.vm.box = "ffuenf/ubuntu-14.04.1-server-amd64"
  config.vm.hostname = VBOX_HOSTNAME

  # Forward port `14567` to Gollum (wiki)
  # 
  config.vm.network :forwarded_port, guest: 4567, host: 14567

  # Enable `vagrant-cachier` to cache APT packages for multiple machine
  # environment.
  #
  if Vagrant.has_plugin?("vagrant-cachier")
    config.cache.scope = :machine
    config.cache.auto_detect = true
  else
    # only display the tips on vagrant up
    if ARGV[0] == "up"
      puts "[information] recommended vagrant plugin 'vagrant-cachier' plugin was not found"
      puts "[information] 'vagrant-cachier' will speed up repeated provisioning operations"
      puts "[information] install the plugin with command 'vagrant plugin install vagrant-cachier'"
    end
  end

  # Provider-specific configuration so you can fine-tune various
  # backing providers for Vagrant. These expose provider-specific options.
  # Example for VirtualBox:
  #
  config.vm.provider "virtualbox" do |vb|
    # Don't boot with headless mode
    vb.gui = false
  end

  config.vm.provision :shell, inline: <<-EOF
    sudo apt-get update
    sudo apt-get install -y git ruby-full libicu-dev build-essential
    sudo gem install gollum
  EOF
end



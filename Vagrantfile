# -*- mode: ruby -*-
# vi: set ft=ruby :

project = 'Habitat'
vagrant_root = File.dirname(__FILE__)
local_serialization_root = "#{vagrant_root}/src"
remote_serialization_root = '/inetpub/wwwroot/sc9.local/App_Data/unicorn'

Vagrant.configure('2') do |config|
  config.vm.box = 'w16s-sc901'
  config.vm.box_url = ['d:/.projects/oss/packer/build/w16s-sc901/virtualbox-core/output/vagrant.box']

  # Disable automatic box update checking. If you disable this, then
  # boxes will only be checked for updates when the user runs
  # `vagrant box outdated`. This is not recommended.
  # config.vm.box_check_update = false

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine. In the example below,
  # accessing 'localhost:8080' will access port 80 on the guest machine.
  # NOTE: This will enable public access to the opened port
  # config.vm.network 'forwarded_port', guest: 80, host: 8080

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine and only allow access
  # via 127.0.0.1 to disable public access
  # config.vm.network 'forwarded_port', guest: 80, host: 8080, host_ip: '127.0.0.1'
  config.vm.network :forwarded_port, guest: 80, host: 80, auto_correct: true # http


  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  config.vm.network 'private_network', ip: '192.168.50.4', auto_config: true
  config.vm.hostname = "sc9.local"
  # config.hostsupdater.aliases = ["habitat.dev.local"]
  config.hostmanager.aliases = %w(sc9.local habitat.dev.local )
  
  config.hostmanager.enabled = true
  config.hostmanager.manage_host = true
  config.hostmanager.ignore_private_ip = false
  
  # Create a public network, which generally matched to bridged network.
  # Bridged networks make the machine appear as another physical device on
  # your network.
  # config.vm.network "public_network"

  config.vm.hostname = "#{ENV['COMPUTERNAME']}T#{project}"

  # config.vm.synced_folder "d:/.virtualization/.share", "/share"
  config.vm.synced_folder "#{local_serialization_root}", "#{remote_serialization_root}"

  config.vm.provider 'virtualbox' do |vb|
    vb.name = 'Habitat'

    # Display the VirtualBox GUI when booting the machine
    vb.gui = false
    vb.customize ['modifyvm', :id, '--vram', '100']

    # reduce size in case of multiple VMs
    vb.linked_clone = true

    # Customize the amount of memory on the VM:
    vb.cpus = 2
    vb.memory = '4096'

    # clipboard
    vb.customize ['modifyvm', :id, '--clipboard', 'bidirectional']
    vb.customize ['modifyvm', :id, '--draganddrop', 'bidirectional']

    # network
    vb.customize ['modifyvm', :id, '--natdnshostresolver1', 'on']

    # resolution
    vb.customize ['setextradata', :id, 'GUI/ScaleFactor', '2.00']
    vb.customize ['setextradata', :id, 'GUI/MaxGuestResolution', '1440,900']
  end
end

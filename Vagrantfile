# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.vm.define "dbasmnfs", primary: true do |dbasm|

    dbasm.vm.box = "OEL7_2-x86_64"
    dbasm.vm.box_url = "https://dl.dropboxusercontent.com/s/0yz6r876qkps68i/OEL7_2-x86_64.box"

    dbasm.vm.provider :vmware_fusion do |v, override|
      override.vm.box = "OEL7_2-x86_64-vmware"
      override.vm.box_url = "https://dl.dropboxusercontent.com/s/ymr62ku2vjjdhup/OEL7_2-x86_64-vmware.box"
    end

    dbasm.vm.hostname = "dbasmnfs.example.com"
    dbasm.vm.synced_folder ".", "/vagrant", :mount_options => ["dmode=777","fmode=777"]
    dbasm.vm.synced_folder "./software", "/software"

    dbasm.vm.network :private_network, ip: "10.10.10.7"

    dbasm.vm.provider :virtualbox do |vb|
      vb.customize ["modifyvm"     , :id, "--memory", "4096"]
      vb.customize ["modifyvm"     , :id, "--name"  , "dbasmnfs"]
      vb.customize ["modifyvm"     , :id, "--cpus"  , 2]
      vb.customize ["setextradata", :id, "VBoxInternal/CPUM/HostCPUID/Cache/Leaf"        , "0x4"]
      vb.customize ["setextradata", :id, "VBoxInternal/CPUM/HostCPUID/Cache/SubLeaf"     , "0x4"]
      vb.customize ["setextradata", :id, "VBoxInternal/CPUM/HostCPUID/Cache/eax"         , "0"]
      vb.customize ["setextradata", :id, "VBoxInternal/CPUM/HostCPUID/Cache/ebx"         , "0"]
      vb.customize ["setextradata", :id, "VBoxInternal/CPUM/HostCPUID/Cache/ecx"         , "0"]
      vb.customize ["setextradata", :id, "VBoxInternal/CPUM/HostCPUID/Cache/edx"         , "0"]
      vb.customize ["setextradata", :id, "VBoxInternal/CPUM/HostCPUID/Cache/SubLeafMask" , "0xffffffff"]

    end

    dbasm.vm.provision :shell, :inline => "mkdir -p /etc/puppet;ln -sf /vagrant/puppet/hiera.yaml /etc/puppet/hiera.yaml;rm -rf /etc/puppet/modules;ln -sf /vagrant/puppet/modules /etc/puppet/modules"

    dbasm.vm.provision :puppet do |puppet|

      puppet.environment_path     = "puppet/environments"
      puppet.environment          = "development"

      puppet.manifests_path       = "puppet/environments/development/manifests"
      puppet.manifest_file        = "site.pp"

      # puppet.manifests_path    = "puppet/manifests"
      # puppet.module_path       = "puppet/modules"
      # puppet.manifest_file     = "site.pp"
      #puppet.options           = "--verbose --hiera_config /vagrant/puppet/hiera.yaml"

      puppet.options           = [
                                  '--verbose',
                                  '--report',
                                  '--trace',
#                                  '--debug',
#                                  '--parser future',
                                  '--strict_variables',
                                  '--hiera_config /vagrant/puppet/hiera.yaml'
                                 ]

      puppet.facter = {
        "environment" => "development",
        "vm_type"     => "vagrant",
      }

    end

  end


  config.vm.define "dbasmraw", primary: true do |dbasm|

    dbasm.vm.box = "OEL7_2-x86_64"
    dbasm.vm.box_url = "https://dl.dropboxusercontent.com/s/0yz6r876qkps68i/OEL7_2-x86_64.box"

    dbasm.vm.provider :vmware_fusion do |v, override|
      override.vm.box = "OEL7_2-x86_64-vmware"
      override.vm.box_url = "https://dl.dropboxusercontent.com/s/ymr62ku2vjjdhup/OEL7_2-x86_64-vmware.box"
    end

    dbasm.vm.hostname = "dbasmraw.example.com"
    dbasm.vm.synced_folder ".", "/vagrant", :mount_options => ["dmode=777","fmode=777"]
    dbasm.vm.synced_folder "./software", "/software"

    dbasm.vm.network :private_network, ip: "10.10.10.7"

    dbasm.vm.provider :virtualbox do |vb|
      vb.customize ["modifyvm"     , :id, "--memory", "4096"]
      vb.customize ["modifyvm"     , :id, "--name"  , "dbasmraw"]
      vb.customize ["modifyvm"     , :id, "--cpus"  , 2]
      vb.customize ["setextradata", :id, "VBoxInternal/CPUM/HostCPUID/Cache/Leaf"        , "0x4"]
      vb.customize ["setextradata", :id, "VBoxInternal/CPUM/HostCPUID/Cache/SubLeaf"     , "0x4"]
      vb.customize ["setextradata", :id, "VBoxInternal/CPUM/HostCPUID/Cache/eax"         , "0"]
      vb.customize ["setextradata", :id, "VBoxInternal/CPUM/HostCPUID/Cache/ebx"         , "0"]
      vb.customize ["setextradata", :id, "VBoxInternal/CPUM/HostCPUID/Cache/ecx"         , "0"]
      vb.customize ["setextradata", :id, "VBoxInternal/CPUM/HostCPUID/Cache/edx"         , "0"]
      vb.customize ["setextradata", :id, "VBoxInternal/CPUM/HostCPUID/Cache/SubLeafMask" , "0xffffffff"]

      index = 1
      number_of_files = 4
      (1..number_of_files).each do | index|
        file = "./disk#{index}.vdi"
        unless File.exist?(file)
          vb.customize ['createhd', '--filename', file, '--size', 10*1024, '--variant', 'Fixed']
          vb.customize ['storageattach', :id, '--storagectl', 'SATA Controller', '--port', index, '--device', 0, '--type', 'hdd', '--medium', file]
        end
      end

    end

    dbasm.vm.provision :shell, :inline => "mkdir -p /etc/puppet;ln -sf /vagrant/puppet/hiera.yaml /etc/puppet/hiera.yaml;rm -rf /etc/puppet/modules;ln -sf /vagrant/puppet/modules /etc/puppet/modules"

    dbasm.vm.provision :puppet do |puppet|

      puppet.environment_path     = "puppet/environments"
      puppet.environment          = "development"

      puppet.manifests_path       = "puppet/environments/development/manifests"
      puppet.manifest_file        = "site.pp"

      # puppet.manifests_path    = "puppet/manifests"
      # puppet.module_path       = "puppet/modules"
      # puppet.manifest_file     = "site.pp"
      #puppet.options           = "--verbose --hiera_config /vagrant/puppet/hiera.yaml"

      puppet.options           = [
                                  '--verbose',
                                  '--report',
                                  '--trace',
#                                  '--debug',
#                                  '--parser future',
                                  '--strict_variables',
                                  '--hiera_config /vagrant/puppet/hiera.yaml'
                                 ]

      puppet.facter = {
        "environment" => "development",
        "vm_type"     => "vagrant",
      }

    end

  end



end

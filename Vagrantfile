# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
  # config.vm.network "forwarded_port", guest: 80, host: 8080

  # Create a forwarded port mapping which allows access to a specific port
  # within the machine from a port on the host machine and only allow access
  # via 127.0.0.1 to disable public access
  # config.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"

  # Create a private network, which allows host-only access to the machine
  # using a specific IP.
  # config.vm.network "private_network", ip: "192.168.33.10"

  # Share an additional folder to the guest VM. The first argument is
  # the path on the host to the actual folder. The second argument is
  # the path on the guest to mount the folder. And the optional third
  # argument is a set of non-required options.

  ENV['VAGRANT_DEFAULT_PROVIDER'] = 'hyperv'

  config.vm.define "win_server_vpn" do |wserver|
    wserver.vm.box = "gusztavvargadr/windows-server"
    # wserver.vm.network "public_network",  ip: "192.168.0.55", auto_config: false
    wserver.vm.network "public_network",  auto_config: true
    wserver.vm.box_check_update = false

    wserver.vm.synced_folder "./", "/vagrant_data", disabled: true

    wserver.vm.provider :hyperv do |wsv, override|
      wsv.linked_clone = false
      wsv.enable_virtualization_extensions = true
      wsv.maxmemory = 2048
      wsv.memory = 2048
      wsv.vmname = "win-opnenvpn-client"
      wsv.cpus = 1
    end

    wserver.vm.provider "virtualbox" do |wsvb|
      wsvb.name = "win-opnenvpn-client"
      wsvb.memory = 2048
      wsvb.cpus = 1
    end
  end

  config.vm.define "win_10" do |v|
    v.vm.box = "gusztavvargadr/windows-10"
    # wserver.vm.network "public_network",  ip: "192.168.0.55", auto_config: false
    v.vm.network "public_network",  auto_config: true
    v.vm.box_check_update = false

    v.vm.synced_folder "./", "/vagrant_data", disabled: true

    v.vm.provider :hyperv do |hv, override|
      hv.linked_clone = false
      hv.enable_virtualization_extensions = true
      hv.maxmemory = 2048
      hv.memory = 2048
      hv.cpus = 1
    end

    v.vm.provider "virtualbox" do |vb|
      vb.name = "win_client"
      vb.memory = 2048
      vb.cpus = 1
    end
  end  

  config.vm.define "win_server" do |wserver|
    wserver.vm.box = "gusztavvargadr/windows-server"
    # wserver.vm.network "public_network",  ip: "192.168.0.55", auto_config: false
    wserver.vm.network "public_network",  auto_config: true
    wserver.vm.box_check_update = false

    wserver.vm.synced_folder "./", "/vagrant_data", disabled: true

    wserver.vm.provider :hyperv do |wsv, override|
      wsv.linked_clone = false
      wsv.enable_virtualization_extensions = true
      wsv.maxmemory = 2048
      wsv.memory = 2048
      wsv.cpus = 1
    end

    wserver.vm.provider "virtualbox" do |wsvb|
      wsvb.name = "win_client"
      wsvb.memory = 2048
      wsvb.cpus = 1
    end
  end

  config.vm.define "server" do |server|
    server.vm.box = "generic/ubuntu2004"
    # server.vm.network "public_network",  ip: "192.168.0.56", auto_config: true
    server.vm.network "public_network",  auto_config: true
    server.vm.box_check_update = false

    server.vm.synced_folder "./", "/vagrant_data", disabled: true

    server.vm.provider :hyperv do |sv, override|
      sv.linked_clone = false
      sv.enable_virtualization_extensions = true
      sv.maxmemory = 1024
      sv.memory = 1024
      sv.cpus = 1
    end

    server.vm.provider "virtualbox" do |svb|
      svb.name = "server"
      svb.memory = 1024
      svb.cpus = 1
    end

    server.vm.provision "shell", inline: <<-SHELL
      apt-get update
      yes | apt-get upgrade
      yes | apt-get install wireguard wireguard-tools wireguard-dkms \
                            linux-headers-$(uname --kernel-release) \
                            git isc-dhcp-server \
                            wpasupplicant 
      update-alternatives --set editor /usr/bin/vim.basic
    SHELL
  end

  config.vm.define "client" do |client|
    client.vm.box = "generic/ubuntu2004"
    # client.vm.network "public_network",  ip: "192.168.0.57", auto_config: false
    client.vm.network "public_network", auto_config: true
    client.vm.box_check_update = false

    client.vm.synced_folder "./", "/vagrant_data", disabled: true

    client.vm.provider :hyperv do |cl, override|
      cl.linked_clone = false
      cl.enable_virtualization_extensions = true
      cl.maxmemory = 1024
      cl.memory = 1024
      cl.cpus = 1
    end

    client.vm.provider "virtualbox" do |cvb|
      cvb.name = "client"
      cvb.memory = 1024
      cvb.cpus = 1
    end

    client.vm.provision "shell", inline: <<-SHELL
      apt-get update
      yes | apt-get upgrade
      yes | apt-get install wireguard wireguard-tools wireguard-dkms \
                            linux-headers-$(uname --kernel-release) \
                            git isc-dhcp-server \
                            wpasupplicant 
      update-alternatives --set editor /usr/bin/vim.basic
    SHELL
  end

end

# -*- mode: ruby -*-
# vi: set ft=ruby :

home_disk = './home.vdi'
docker_disk = './docker.vdi'
Vagrant.configure("2") do |config|
  config.vm.box = "debian/contrib-stretch64"
  config.vm.hostname = "debian"

  # Port forwardings
  config.vm.network 'forwarded_port', guest: 8080, host: 8080
  config.vm.network 'forwarded_port', guest: 3000, host: 3000
  config.vm.network 'forwarded_port', guest: 3001, host: 3001
  config.vm.network 'forwarded_port', guest: 3002, host: 3002
  config.vm.network 'forwarded_port', guest: 3003, host: 3003
  config.vm.network 'forwarded_port', guest: 9000, host: 9000

  # Shared Folders
  config.vm.synced_folder 'E:/work', '/work'

  config.vm.provider "virtualbox" do |vb|
    vb.name = "vagrant-debian-kde"

    # Memory
    vb.customize ["modifyvm", :id, "--memory", "8192"]
    # Display / GUI
    vb.customize ["modifyvm", :id, "--vram", "128"]
    vb.customize ["modifyvm", :id, "--accelerate3d", "on"]
    vb.gui = true
    # VirtualBox Guest Additions
    vb.customize ['modifyvm', :id, '--clipboard', 'bidirectional']
    vb.customize ["modifyvm", :id, "--draganddrop", "bidirectional"]
    # Dual core
    vb.customize ["modifyvm", :id, "--cpus", 2]
    vb.cpus = 2
    # Only use 50% of host CPU:
	  vb.customize ["modifyvm", :id, "--cpuexecutioncap", "50"]

    unless File.exist?(home_disk)
      vb.customize ['createhd', '--filename', home_disk, '--size', 40 * 1024]
    end
    unless File.exist?(docker_disk)
      vb.customize ['createhd', '--filename', docker_disk, '--size', 100 * 1024]
    end

    vb.customize ['storageattach', :id, '--storagectl', 'SATA Controller', '--port', 1, '--device', 0, '--type', 'hdd', '--medium', home_disk]
    vb.customize ['storageattach', :id,  '--storagectl', 'SATA Controller', '--port', 2, '--device', 0, '--type', 'hdd', '--medium', docker_disk]
  end

  config.vm.provision "ansible_local" do |ansible|
    ansible.become = true
    ansible.version = "latest"
    ansible.install_mode = "pip"
    ansible.playbook = "provisioning/playbook.yml"
  end
end

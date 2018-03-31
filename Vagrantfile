# -*- mode: ruby -*-
# vi: set ft=ruby :

home_disk = './home.vdi'
docker_disk = './docker.vdi'
Vagrant.configure("2") do |config|

  config.vm.box = "debian/contrib-stretch64"

  config.vm.provider "virtualbox" do |vb|
    vb.gui = true
    vb.memory = "8192"
    vb.name = "devbox"
    vb.cpus = 8
    vb.customize ["modifyvm", :id, "--vram", "128"]
    vb.customize ["modifyvm", :id, "--accelerate3d", "on"]

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
    ansible.playbook = "provisioning/playbook.yml"
  end
end

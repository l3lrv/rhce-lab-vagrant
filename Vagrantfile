Vagrant.configure(2) do |config|

  config.vm.define "control" do |node|
  config.vm.provision "shell", inline: <<-'SHELL'
    sed -i 's/^#* \(PermitRootLogin\)\(.\)$/\1 yes/' /etc/ssh/sshd_config
    sed -i 's/^#* \(PasswordAuthentication\)\(.\)$/\1 yes/' /etc/ssh/sshd_config
    systemctl restart sshd.service
    echo -e "vagrant\nvagrant" | (passwd vagrant)
    echo -e "root\nroot" | (passwd root)
  SHELL
#  config.vm.box_check_update = false
  config.disksize.size = '15GB'
#  config.vm.provision :reload
#  config.vm.synced_folder ".", "/vagrant", disabled: true
#  config.vm.synced_folder ".", "/home/vagrant/provision", type: "rsync"
#
    node.vm.box               = "rockylinux/8"
    node.vm.hostname          = "control.realmX.example.com"

    node.vm.network "private_network", ip: "192.168.56.50"
    node.vbguest.installer_options = { allow_kernel_upgrade: true }
  
    node.vm.provider :virtualbox do |v|
      v.name    = "control"
      v.memory  = 1024
      v.cpus    = 1
    end
  
  end

  NodeCount = 5

  (1..NodeCount).each do |i|

    config.vm.define "node#{i}" do |node|

      node.vm.box               = "rockylinux/8"
      node.vm.hostname          = "node#{i}realmX.example.com"

      node.vm.network "private_network", ip: "192.168.56.5#{i}"
      node.vbguest.installer_options = { allow_kernel_upgrade: true }

      node.vm.provider :virtualbox do |v|
        v.name    = "node#{i}"
        v.memory  = 1024
        v.cpus    = 1
        file_to_disk2 = "/home/raoul/VirtualBox VMs/node#{i}/disk2.vdi"
        unless File.exist?("/home/raoul/VirtualBox VMs/node#{i}/disk2.vdi")
          v.customize ['createhd', '--filename', "/home/raoul/VirtualBox VMs/node#{i}/disk2.vdi", '--size', 10 * 1024]
          v.customize ['storageattach', :id, '--storagectl', 'IDE Controller', '--port', 1, '--device', 0, '--type', 'hdd','--medium', "/home/raoul/VirtualBox VMs/node#{i}/disk2.vdi"]
        end

      end

    end

  end

end

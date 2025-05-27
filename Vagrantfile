Vagrant.configure("2") do |config|
    config.vm.box = "debian/bookworm64"
    config.vm.box_version = "12.20250126.1"
  nodes = [
    { name: "k8s-control-plane-01", ip: "172.20.20.11" },
    { name: "k8s-worker-01", ip: "172.20.20.21" },
    { name: "k8s-worker-02", ip: "172.20.20.22" }
  ]

  nodes.each do |node|

    config.vm.define node[:name] do |vm|
      vm.vm.hostname = node[:name]
      vm.vm.network "private_network", ip: node[:ip]

      vm.vm.provider :libvirt do |libvirt|
        libvirt.memory = 2048
        libvirt.cpus = 2

        # Create disk storage
        libvirt.storage :file, size: '20G', name: "#{node[:name]}-disk.qcow2", type: 'qcow2'

        # Set boot order: cdrom first, then disk
        libvirt.boot 'cdrom'
        libvirt.boot 'hd'
      end

      vm.vm.provision "shell", inline: <<-SHELL
        apt update && apt install -y sudo python3 python3-apt curl vim net-tools openssh-server

        useradd -m -s /bin/bash ansible
        echo "ansible:ansible" | chpasswd
        echo "ansible ALL=(ALL) NOPASSWD:ALL" > /etc/sudoers.d/ansible

        mkdir -p /home/ansible/.ssh
        cp /home/vagrant/.ssh/authorized_keys /home/ansible/.ssh/
        chown -R ansible:ansible /home/ansible/.ssh
        chmod 700 /home/ansible/.ssh
        chmod 600 /home/ansible/.ssh/authorized_keys

        echo "127.0.0.1   localhost" > /etc/hosts
        echo "#{node[:ip]}   #{node[:name]}" >> /etc/hosts
        echo "172.20.20.11  k8s-control-plane-01" >> /etc/hosts
        echo "172.20.20.21  k8s-worker-01" >> /etc/hosts
        echo "172.20.20.22  k8s-worker-02" >> /etc/hosts
      SHELL
    end
  end
end

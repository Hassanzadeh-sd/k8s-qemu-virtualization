
sudo apt install -y qemu-kvm libvirt-daemon-system libvirt-clients bridge-utils virt-manager vagrant vagrant-libvirt


vagrant box add debian/bookworm64 --provider=libvirt

vagrant init debian/bookworm64 --box-version 12.20250126.1

Vagrant.configure("2") do |config|
  config.vm.box = "debian/bookworm64"
  config.vm.box_version = "12.20250126.1"
end

vagrant box list


vagrant status

sudo vagrant ssh k8s-worker-01

sudo virsh list --all
sudo virsh dominfo virtualization_k8s-worker-02

sudo varish shutdown <vn-name>

sudo varish start <vm-name>


Vagrant.configure("2") do |config|
  # Base VM
  config.vm.box = "ubuntu/jammy64"
  config.vm.hostname = "hadoop-ansible"

  # Networking
  config.vm.network "private_network", ip: "192.168.33.10"

  # VM resources
  config.vm.provider "virtualbox" do |vb|
    vb.gui = false
    vb.memory = 12288   # 12 GB
    vb.cpus   = 4
    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
  end

  # # Sync current directory (Vagrantfile + bigdata.yml) into guest
  # config.vm.synced_folder ".", "/vagrant"

  # 1) Install Ansible inside the VM
  config.vm.provision "shell", inline: <<-SHELL
    set -eux
    sudo apt-get update -y
    sudo apt-get install -y python3 python3-pip software-properties-common
    sudo apt-get install -y ansible
  SHELL

  # 2) Use ansible_local to run your playbook
  config.vm.provision "ansible_local" do |ansible|
    ansible.playbook = "bigdata.yml"   # playbook must exist next to the Vagrantfile
    ansible.install  = false           # Ansible already installed above
  end
end

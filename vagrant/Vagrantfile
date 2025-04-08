ENV['VAGRANT_DEFAULT_PROVIDER'] = 'virtualbox'

Vagrant.configure("2") do |config|
    config.vm.box = "ubuntu/focal64"
    config.vm.synced_folder ".", "/vagrant"
  
    # Nodo Manager
    config.vm.define "manager" do |manager|
      manager.vm.hostname = "manager"
      manager.vm.network "private_network", ip: "192.168.56.10"
      manager.vm.network "forwarded_port", guest: 22, host: 2222, id: "ssh", auto_correct: true
  # Provisionador shell para instalar Ansible
      manager.vm.provision "shell", inline: <<-SHELL
        apt-get update -y -qq
        apt-get install -y -qq python3 python3-pip
        pip3 install ansible==2.9
      SHELL

      # Provisionador ansible_local
      manager.vm.provision "ansible_local" do |ansible|
        ansible.playbook = "provision-manager.yml"
        ansible.install = false # Desactivamos la instalación automática de Ansible
        ansible.verbose = "vvv"
      end
    end
  
    # Nodo Worker 1
    config.vm.define "worker1" do |worker1|
      worker1.vm.hostname = "worker1"
      worker1.vm.network "private_network", ip: "192.168.56.11"
      worker1.vm.network "forwarded_port", guest: 22, host: 2223, id: "ssh", auto_correct: true
      # Provisionador shell para instalar Ansible
      worker1.vm.provision "shell", inline: <<-SHELL
      apt-get update -y -qq
      apt-get install -y -qq python3 python3-pip
      pip3 install ansible==2.9
    SHELL

      # Provisionador ansible_local
      worker1.vm.provision "ansible_local" do |ansible|
        ansible.playbook = "provision-worker.yml"
        ansible.install = false # Desactivamos la instalación automática de Ansible
        ansible.verbose = "vvv"
      end
    end
  
    # Nodo Worker 2
    config.vm.define "worker2" do |worker2|
      worker2.vm.hostname = "worker2"
      worker2.vm.network "private_network", ip: "192.168.56.12"
      worker2.vm.network "forwarded_port", guest: 22, host: 2224, id: "ssh", auto_correct: true
      # Provisionador shell para instalar Ansible
      worker2.vm.provision "shell", inline: <<-SHELL
      apt-get update -y -qq
      apt-get install -y -qq python3 python3-pip
      pip3 uninstall -y ansible || true
      apt-get remove -y ansible || true
      rm -rf /usr/local/lib/python3.8/dist-packages/ansible*
      pip3 install ansible==2.9
    SHELL

      # Provisionador ansible_local
      worker2.vm.provision "ansible_local" do |ansible|
        ansible.playbook = "provision-worker.yml"
        ansible.install = false # Desactivamos la instalación automática de Ansible
        ansible.verbose = "vvv"
      end
    end
  end
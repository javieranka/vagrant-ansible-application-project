# -*- mode: ruby -*-
# vi: set ft=ruby :


Vagrant.configure("2") do |config|

  config.vm.box = "ubuntu/focal64"

  # ---------------------------------------------
  # Ustawienia dla maszyny db
  # ---------------------------------------------
  config.vm.define "db" do |db|
    db.vm.hostname = "db"
    db.vm.network "private_network", ip: "192.168.50.11"

    db.vm.provision "shell", inline: <<-SHELL
      echo '192.168.50.10 ansible-server' | sudo tee -a /etc/hosts
      echo '192.168.50.11 db' | sudo tee -a /etc/hosts
      echo '192.168.50.12 backend' | sudo tee -a /etc/hosts
      echo '192.168.50.13 frontend' | sudo tee -a /etc/hosts
    SHELL
  end

  # ---------------------------------------------
  # Ustawienia dla maszyny backend
  # ---------------------------------------------
  config.vm.define "backend" do |backend|
    backend.vm.hostname = "backend"
    backend.vm.network "private_network", ip: "192.168.50.12"

    backend.vm.provision "shell", inline: <<-SHELL
      echo '192.168.50.10 ansible-server' | sudo tee -a /etc/hosts
      echo '192.168.50.11 db' | sudo tee -a /etc/hosts
      echo '192.168.50.12 backend' | sudo tee -a /etc/hosts
      echo '192.168.50.13 frontend' | sudo tee -a /etc/hosts
    SHELL
  end

  # ---------------------------------------------
  # Ustawienia dla maszyny frontend
  # ---------------------------------------------
  config.vm.define "frontend" do |frontend|
    frontend.vm.network "forwarded_port", guest: 3000, host: 8080
    frontend.vm.network "private_network", ip: "192.168.50.13"
    frontend.vm.hostname = "frontend"

    frontend.vm.provision "shell", inline: <<-SHELL
      echo '192.168.50.10 ansible-server' | sudo tee -a /etc/hosts
      echo '192.168.50.11 db' | sudo tee -a /etc/hosts
      echo '192.168.50.12 backend' | sudo tee -a /etc/hosts
      echo '192.168.50.13 frontend' | sudo tee -a /etc/hosts
    SHELL
  end

  # ---------------------------------------------
  # Ustawienia dla maszyny ansible-server
  # ---------------------------------------------
  config.vm.define "ansible-server" do |ansible|
    ansible.vm.hostname = "ansible-server"
    ansible.vm.network "private_network", ip: "192.168.50.10"

    ansible.vm.provision "shell", inline: <<-SHELL

      # Instalacja ansible na ansible-server
      sudo apt update
      sudo apt install -y ansible

      # Aktualizacja /etc/hosts
      echo '192.168.50.10 ansible-server' | sudo tee -a /etc/hosts
      echo '192.168.50.11 db' | sudo tee -a /etc/hosts
      echo '192.168.50.12 backend' | sudo tee -a /etc/hosts
      echo '192.168.50.13 frontend' | sudo tee -a /etc/hosts

      # Provisioning z użyciem głównego playbooka
      echo "Running final Ansible playbook..."
      cd /vagrant/ansible
      ansible-playbook playbooks/main.yml -i inventory/vamos-hosts.ini
    SHELL
  end

end

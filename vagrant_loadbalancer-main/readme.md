# VAGRANT + WEBSERVERS AND NGINX LOADBALANCER

### Structure
> Load balance <br />
> Webservers <br />
> Controller Node ( where Ansible is installed) <br/>
> Provider - Virtual box
> Provisioning - Ansible

### Basic Requirements

0. Install [Vagrant](https://www.vagrantup.com/)
0. Install Virtual Box

1. Steps to reproduce

0. Vagrantbox Image https://app.vagrantup.com/ubuntu/boxes/bionic64 
    ```sh
    vagrant init ubuntu/bionic64
    ```

0. Configure  Vagrant file :

    ```sh
    Vagrant.configure("2") do |config|
    config.vm.box = "ubuntu/ubuntu-18.04"
 
    config.vm.define "lb" do |machine|
        machine.vm.network "private_network", ip: "172.17.177.21"
        machine.vm.network "forwarded_port", guest: 80, host: 8080, host_ip: "127.0.0.1"
    end
 
    config.vm.define "web1" do |machine|
        machine.vm.network "private_network", ip: "172.17.177.22"
    end
 
    config.vm.define "web2" do |machine|
        machine.vm.network "private_network", ip: "172.17.177.23"
    end
 
    config.vm.define "controller" do |machine|
        machine.vm.network "private_network", ip: "172.17.177.11"
 
        machine.vm.provision "ansible_local" do |ansible|
            ansible.playbook = "provisioning/playbook.yml"
            ansible.limit = "all"
            ansible.inventory_path = "provisioning/hosts"
            ansible.config_file = "provisioning/ansible.cfg"
        end
 
        machine.vm.synced_folder ".", "/vagrant", mount_options: [ "umask=077" ]
    end
end
    ```

0. Start Flask development server:
    ```sh
    cd my-project-name
    make run
  
  ### Requirements
  0. Install Vagrant  (Vagrantbox Image https://app.vagrantup.com/ubuntu/boxes/bionic64)
  0. add a  Virtual box https://www.virtualbox.org/wiki/Downloads
  0. Create a new instance using the vagrant init ubuntu/bionic64
  0. Add a Virtual box (vb) -- vagrant box add ubuntu/bionic64
 
  

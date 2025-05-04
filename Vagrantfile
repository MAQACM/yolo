Vagrant.configure("2") do |config|
  config.vm.box="geerlingguy/ubuntu2004"

  #Networking
  config.vm.network "private_network", ip: "192.168.56.10" #internal network
  config.vm.network "forwarded_port", guest: 3000, host: 3000 #frontend
  config.vm.network "forwarded_port", guest: 4000, host: 4000 #backend
  config.vm.network "forwarded_port", guest: 27017, host: 27017 #mongodb

  #Resource specific configuration
  config.vm.provider "virtualbox" do |vb|
    vb.memory = "4048"
    vb.cpus = 2
    vb.name = "yolomy-ecommerce"
  end

  #Provisioning
  config.vm.provision "ansible" do |ansible|
    ansible.playbook ="playbook.yml"
    ansible.become = true
    ansible.compatibility_mode = "2.0"
  end

end
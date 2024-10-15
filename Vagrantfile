Vagrant.configure("2") do |config|
    # Configuración de la máquina virtual
    config.vm.box = "ubuntu/bionic64"
  
    # Configuración de red
    config.vm.network "private_network", type: "dhcp"
  
    # Provisionamiento
    config.vm.provision "shell", inline: <<-SHELL
      apt-get update
      apt-get install -y apache2
    SHELL
  end
  
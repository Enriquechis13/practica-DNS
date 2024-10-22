Vagrant.configure("2") do |config|

  # Máquina 1: Debian texto (venus)
  config.vm.define "venus" do |venus|
    venus.vm.box = "debian/bookworm64"
    venus.vm.hostname = "venus.sistema.test"
    venus.vm.network "private_network", ip: "192.168.57.102"
  end

  # Máquina 2: Debian texto (tierra)
  config.vm.define "tierra" do |tierra|
    tierra.vm.box = "debian/bookworm64"
    tierra.vm.hostname = "tierra.sistema.test"
    tierra.vm.network "private_network", ip: "192.168.57.103"
    
    tierra.vm.provision "shell", inline: <<-SHELL
      sudo apt-get update
      sudo apt-get install -y bind9 bind9utils bind9-doc
      sudo cp -v /vagrant/named /etc/default/named
      sudo systemctl restart named
    SHELL
  end
end


Vagrant.configure("2") do |config|
  config.vm.network "private_network", type: "dhcp"

  # Máquina 1: Debian texto (venus)
  config.vm.define "venus" do |venus|
    venus.vm.box = "debian/bookworm64"
    venus.vm.hostname = "venus.sistema.test"
    venus.vm.network "private_network", ip: "192.168.57.102"
  end

  # Máquina 2: Debian texto (tierra)
  config.vm.define "tierra" do |tierra|
    tierra.vm.box = "debian/buster64"
    tierra.vm.hostname = "tierra.sistema.test"
    tierra.vm.network "private_network", ip: "192.168.57.103"
  end
end


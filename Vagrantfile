Vagrant.configure("2") do |config|

  # Máquina 1: Debian texto (venus)
  config.vm.define "venus" do |venus|
    venus.vm.box = "debian/bookworm64"
    venus.vm.hostname = "venus.sistema.test"
    venus.vm.network "private_network", ip: "192.168.57.102"

    venus.vm.provision "shell", inline: <<-SHELL
      sudo apt-get update
      sudo apt-get install -y bind9 bind9utils bind9-doc
      sudo cp -v /vagrant/named /etc/default/named
      sudo systemctl restart named
    SHELL
  end

  # Máquina 2: Debian texto (tierra)
  config.vm.define "tierra" do |tierra|
    tierra.vm.box = "debian/bookworm64"
    tierra.vm.hostname = "tierra.sistema.test"
    tierra.vm.network "private_network", ip: "192.168.57.103"

    tierra.vm.provision "shell", inline: <<-SHELL
      sudo apt-get update
      
      sudo apt-get install -y bind9 bind9utils bind9-doc
      
      echo 'acl "allowed_clients" {' | sudo tee -a /etc/bind/named.conf.options
      echo '    127.0.0.0/8;' | sudo tee -a /etc/bind/named.conf.options
      echo '    192.168.57.0/24;' | sudo tee -a /etc/bind/named.conf.options
      echo '};' | sudo tee -a /etc/bind/named.conf.options
      echo '' | sudo tee -a /etc/bind/named.conf.options
      echo 'options {' | sudo tee -a /etc/bind/named.conf.options
      echo '    directory "/var/cache/bind";' | sudo tee -a /etc/bind/named.conf.options
      echo '    dnssec-validation yes;' | sudo tee -a /etc/bind/named.conf.options
      echo '    recursion yes;' | sudo tee -a /etc/bind/named.conf.options
      echo '    allow-recursion { "allowed_clients"; };' | sudo tee -a /etc/bind/named.conf.options
      echo '    auth-nxdomain no;' | sudo tee -a /etc/bind/named.conf.options
      echo '    listen-on-v6 { any; };' | sudo tee -a /etc/bind/named.conf.options
      echo '};' | sudo tee -a /etc/bind/named.conf.options

      echo 'zone "sistema.test" {' | sudo tee -a /etc/bind/named.conf.local
      echo '    type master;' | sudo tee -a /etc/bind/named.conf.local
      echo '    file "/etc/bind/db.sistema.test";' | sudo tee -a /etc/bind/named.conf.local
      echo '};' | sudo tee -a /etc/bind/named.conf.local
      echo '' | sudo tee -a /etc/bind/named.conf.local
      echo 'zone "57.168.192.in-addr.arpa" {' | sudo tee -a /etc/bind/named.conf.local
      echo '    type master;' | sudo tee -a /etc/bind/named.conf.local
      echo '    file "/etc/bind/db.192.168.57";' | sudo tee -a /etc/bind/named.conf.local
      echo '};' | sudo tee -a /etc/bind/named.conf.local

      sudo systemctl restart bind9
    SHELL
  end
end



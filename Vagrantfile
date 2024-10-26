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
      sudo mkdir -p /var/named/slaves
      sudo chown bind:bind /var/named/slaves

      echo 'zone "sistema.test" {' | sudo tee -a /etc/bind/named.conf.local
      echo '    type slave;' | sudo tee -a /etc/bind/named.conf.local
      echo '    file "/var/named/slaves/db.sistema.test";' | sudo tee -a /etc/bind/named.conf.local
      echo '    masters { 192.168.57.103; };' | sudo tee -a /etc/bind/named.conf.local
      echo '};' | sudo tee -a /etc/bind/named.conf.local

      echo 'zone "57.168.192.in-addr.arpa" {' | sudo tee -a /etc/bind/named.conf.local
      echo '    type slave;' | sudo tee -a /etc/bind/named.conf.local
      echo '    file "/var/named/slaves/db.192.168.57";' | sudo tee -a /etc/bind/named.conf.local
      echo '    masters { 192.168.57.103; };' | sudo tee -a /etc/bind/named.conf.local
      echo '};' | sudo tee -a /etc/bind/named.conf.local
      
      sudo systemctl restart bind9
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
      sudo cp -v /vagrant/named.conf.options /etc/bind/named.conf.options

      echo 'acl "allowed_clients" {' | sudo tee -a /etc/bind/named.conf.options
      echo '    127.0.0.0/8;' | sudo tee -a /etc/bind/named.conf.options
      echo '    192.168.57.0/24;' | sudo tee -a /etc/bind/named.conf.options
      echo '};' | sudo tee -a /etc/bind/named.conf.options
      echo 'options {' | sudo tee -a /etc/bind/named.conf.options
      echo '    directory "/var/cache/bind";' | sudo tee -a /etc/bind/named.conf.options
      echo '    dnssec-validation yes;' | sudo tee -a /etc/bind/named.conf.options
      echo '    recursion yes;' | sudo tee -a /etc/bind/named.conf.options
      echo '    allow-recursion { "allowed_clients"; };' | sudo tee -a /etc/bind/named.conf.options
      echo '    auth-nxdomain no;' | sudo tee -a /etc/bind/named.conf.options
      echo '    listen-on-v6 { any; };' | sudo tee -a /etc/bind/named.conf.options
      echo '    forwarders { 208.67.222.222; };' | sudo tee -a /etc/bind/named.conf.options
      echo '    forward only;' | sudo tee -a /etc/bind/named.conf.options
      echo '};' | sudo tee -a /etc/bind/named.conf.options

      echo 'zone "sistema.test" {' | sudo tee -a /etc/bind/named.conf.local
      echo '    type master;' | sudo tee -a /etc/bind/named.conf.local
      echo '    file "/etc/bind/db.sistema.test";' | sudo tee -a /etc/bind/named.conf.local
      echo '};' | sudo tee -a /etc/bind/named.conf.local

      echo 'zone "57.168.192.in-addr.arpa" {' | sudo tee -a /etc/bind/named.conf.local
      echo '    type master;' | sudo tee -a /etc/bind/named.conf.local
      echo '    file "/etc/bind/db.192.168.57";' | sudo tee -a /etc/bind/named.conf.local
      echo '};' | sudo tee -a /etc/bind/named.conf.local

      echo '$TTL    86400' | sudo tee /etc/bind/db.sistema.test
      echo '@       IN      SOA     tierra.sistema.test. admin.sistema.test. (' | sudo tee -a /etc/bind/db.sistema.test
      echo '                          2023102401 ; Serial' | sudo tee -a /etc/bind/db.sistema.test
      echo '                          3600       ; Refresh' | sudo tee -a /etc/bind/db.sistema.test
      echo '                          1800       ; Retry' | sudo tee -a /etc/bind/db.sistema.test
      echo '                          604800     ; Expire' | sudo tee -a /etc/bind/db.sistema.test
      echo '                          86400 )    ; Negative Cache TTL' | sudo tee -a /etc/bind/db.sistema.test
      echo '' | sudo tee -a /etc/bind/db.sistema.test
      echo '; Servidores de nombre' | sudo tee -a /etc/bind/db.sistema.test
      echo '@       IN      NS      ns1.sistema.test.' | sudo tee -a /etc/bind/db.sistema.test
      echo '@       IN      NS      ns2.sistema.test.' | sudo tee -a /etc/bind/db.sistema.test
      echo '@       IN      MX 10   marte.sistema.test.' | sudo tee -a /etc/bind/db.sistema.test
      echo 'ns1     IN      A       192.168.57.103' | sudo tee -a /etc/bind/db.sistema.test
      echo 'ns2     IN      A       192.168.57.102' | sudo tee -a /etc/bind/db.sistema.test
      echo 'venus   IN      A       192.168.57.102' | sudo tee -a /etc/bind/db.sistema.test
      echo 'tierra  IN      A       192.168.57.103' | sudo tee -a /etc/bind/db.sistema.test
      echo 'marte   IN      A       192.168.57.104' | sudo tee -a /etc/bind/db.sistema.test

      sudo chown bind:bind /etc/bind/db.sistema.test
      sudo chmod 644 /etc/bind/db.sistema.test

      sudo systemctl restart bind9
    SHELL
  end
end

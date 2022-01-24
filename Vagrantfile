$script_mysql = <<-SCRIPT
    apt-get update && \
    apt-get install -y mysql-server-5.7 && \
    mysql -e "create user 'phpuser'@'%' identified by 'pass';"
SCRIPT


Vagrant.configure("2") do |config|
   config.vm.box = "ubuntu/bionic64"
   config.vm.network "forwarded_port", guest: 80, host: 8089    
   config.vm.network "public_network", bridge: "wlp2s0", ip:"192.168.15.5"

   config.vm.provision "shell", 
       inline: "cat /configs/id_bionic.pub >> .ssh/authorized_keys"
   config.vm.provision "shell", inline: $script_mysql
   config.vm.provision "shell", 
       inline: "cat /configs/mysqld.cnf > /etc/mysql/mysql.conf.d/mysqld.cnf"
   config.vm.provision "shell", 
       inline: "service mysql restart"

    config.vm.synced_folder "./configs", "/configs"
    config.vm.synced_folder ".", "/vagrant", disabled: true
  end
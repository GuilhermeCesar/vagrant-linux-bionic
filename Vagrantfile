$script_mysql = <<-SCRIPT
    apt-get update && \
    apt-get install -y mysql-server-5.7 && \
    mysql -e "create user 'phpuser'@'%' identified by 'pass';"
SCRIPT


Vagrant.configure("2") do |config|
   config.vm.box = "ubuntu/bionic64"

   config.vm.define "mysqldb" do |mysql|    
    mysql.vm.network "public_network", bridge: "wlp2s0", ip:"192.168.15.5"
 
    mysql.vm.provision "shell", 
        inline: "cat /configs/id_bionic.pub >> .ssh/authorized_keys"
    mysql.vm.provision "shell", inline: $script_mysql
    mysql.vm.provision "shell", 
        inline: "cat /configs/mysqld.cnf > /etc/mysql/mysql.conf.d/mysqld.cnf"
    mysql.vm.provision "shell", 
        inline: "service mysql restart"
 
    mysql.vm.synced_folder "./configs", "/configs"
    mysql.vm.synced_folder ".", "/vagrant", disabled: true
   end

 
  config.vm.define "phpweb" do |phpweb|
    phpweb.vm.network "forwarded_port", guest: 80, host: 8089    
    phpweb.vm.network "public_network", bridge: "wlp2s0", ip:"192.168.15.6"
  end

end
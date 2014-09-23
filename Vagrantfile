# -*- mode: ruby -*-
# vi: set ft=ruby :

$is_script = <<ISSCRIPT
wget --no-check-certificate https://github.com/aglover/ubuntu-equip/raw/master/equip_java7_64.sh && bash equip_java7_64.sh
java -version
sudo apt-get install unzip
sudo mkdir /opt/wso2
sudo cp /vagrant/packs/wso2is-4.6.0.zip /opt/wso2
cd /opt/wso2
unzip wso2is-4.6.0.zip
cp -rf /vagrant/config/is/* wso2is-4.6.0/
wget http://repo1.maven.org/maven2/mysql/mysql-connector-java/5.1.32/mysql-connector-java-5.1.32.jar -P wso2is-4.6.0/repository/components/lib/
cd wso2is-4.6.0
sudo JAVA_HOME=/usr/lib/jvm/jdk1.7.0_65 ./bin/wso2server.sh start
ISSCRIPT

$as_script = <<ASSCRIPT
wget --no-check-certificate https://github.com/aglover/ubuntu-equip/raw/master/equip_java7_64.sh && bash equip_java7_64.sh
java -version
sudo apt-get install unzip
sudo mkdir /opt/wso2
sudo cp /vagrant/packs/wso2as-5.2.1.zip /opt/wso2
cd /opt/wso2
unzip wso2as-5.2.1.zip
cp -rf /vagrant/config/as/* wso2as-5.2.1/
wget http://repo1.maven.org/maven2/mysql/mysql-connector-java/5.1.32/mysql-connector-java-5.1.32.jar -P wso2as-5.2.1/repository/components/lib/
cd wso2as-5.2.1
sudo JAVA_HOME=/usr/lib/jvm/jdk1.7.0_65 ./bin/wso2server.sh start
ASSCRIPT

$mysql_script = <<MYSQL
sudo debconf-set-selections <<< 'mysql-server-5.5 mysql-server/root_password password root'
sudo debconf-set-selections <<< 'mysql-server-5.5 mysql-server/root_password_again password root'
sudo apt-get update
sudo apt-get -y install mysql-server-5.5

sudo sed -i 's/127.0.0.1/0.0.0.0/' /etc/mysql/my.cnf

sudo /etc/init.d/mysql restart

if [ ! -f /var/log/databasesetup ];
then
    echo "CREATE DATABASE ssotestregistrydb" | mysql -uroot -proot
    echo "CREATE DATABASE ssotestuserdb" | mysql -uroot -proot
    echo "GRANT ALL ON *.* TO root@'%' IDENTIFIED BY 'root'" | mysql -uroot -proot
    echo "FLUSH PRIVILEGES" | mysql -uroot -proot
    touch /var/log/databasesetup
    
    if [ -f /vagrant/config/mysql/mysql.sql ];
    then
        mysql -uroot -proot ssotestregistrydb < /vagrant/config/mysql/mysql.sql
        mysql -uroot -proot ssotestuserdb < /vagrant/config/mysql/mysql.sql
    fi
fi
MYSQL

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

    config.vm.box = "hashicorp/precise64"

    config.hostmanager.enabled = true
    config.hostmanager.manage_host = true
    config.hostmanager.include_offline = true
    config.hostmanager.ignore_private_ip = false

    config.vm.define "mysql" do |mysql|
      mysql.vm.provider :virtualbox do |n|
          n.name = "mysql.test.wso2.com"
          n.memory = 512
      end
      mysql.vm.hostname = "mysql.test.wso2.com"
      mysql.vm.network :private_network, ip: "100.100.1.10"
      mysql.vm.provision :shell, :inline => $mysql_script
    end    

    config.vm.define "is" do |is|
      is.vm.provider :virtualbox do |n|
          n.name = "is.test.wso2.com"
          n.memory = 1536
      end
      is.vm.hostname = "is.test.wso2.com"
      is.vm.network :private_network, ip: "100.100.1.11"
      is.vm.provision :shell, :inline => $is_script
    end

    config.vm.define "as" do |as|
      as.vm.provider :virtualbox do |n|
        n.name = "as.test.wso2.com"
        n.memory = 1536
      end
      as.vm.hostname = "as.test.wso2.com"
      as.vm.network :private_network, ip: "100.100.1.12"
      as.vm.provision :shell, :inline => $as_script
    end

end

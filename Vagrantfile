# -*- mode: ruby -*-
# vi: set ft=ruby :

$is_script = <<ISSCRIPT
wget --no-check-certificate https://github.com/aglover/ubuntu-equip/raw/master/equip_java7_64.sh && bash equip_java7_64.sh
java -version
sudo apt-get install unzip
sudo mkdir /opt/wso2

sudo cp /vagrant/packs/wso2is-5.0.0.zip /opt/wso2
sudo cp /vagrant/packs/wso2dss-3.5.0.zip /opt/wso2
sudo cp /vagrant/packs/wso2esb-4.9.0.zip /opt/wso2
cd /opt/wso2

unzip wso2is-5.0.0.zip
unzip wso2dss-3.5.0.zip
unzip wso2esb-4.9.0.zip
rm wso2is-5.0.0.zip
rm wso2dss-3.5.0.zip
rm wso2esb-4.9.0.zip

cp -rf /vagrant/config/is/* wso2is-5.0.0/
cp -rf /vagrant/config/dss/* wso2dss-3.5.0/
cp -rf /vagrant/config/esb/* wso2esb-4.9.0/
wget http://repo1.maven.org/maven2/mysql/mysql-connector-java/5.1.32/mysql-connector-java-5.1.32.jar -P wso2is-5.0.0/repository/components/lib/
wget http://repo1.maven.org/maven2/mysql/mysql-connector-java/5.1.32/mysql-connector-java-5.1.32.jar -P wso2dss-3.5.0/repository/components/lib/
wget http://repo1.maven.org/maven2/mysql/mysql-connector-java/5.1.32/mysql-connector-java-5.1.32.jar -P wso2esb-4.9.0/repository/components/lib/
chown -R vagrant:vagrant /opt/wso2/wso2dss-3.5.0
chown -R vagrant:vagrant /opt/wso2/wso2is-5.0.0
chown -R vagrant:vagrant /opt/wso2/wso2esb-4.9.0

sudo cp /vagrant/config/wso2is /etc/init.d/wso2is
sudo chmod 755 /etc/init.d/wso2is
sudo /etc/init.d/wso2is start

sudo cp /vagrant/config/wso2dss /etc/init.d/wso2dss
sudo chmod 755 /etc/init.d/wso2dss
sudo /etc/init.d/wso2dss start

sudo cp /vagrant/config/wso2esb /etc/init.d/wso2esb
sudo chmod 755 /etc/init.d/wso2esb
sudo /etc/init.d/wso2esb start
ISSCRIPT

$as_script = <<ASSCRIPT
wget --no-check-certificate https://github.com/aglover/ubuntu-equip/raw/master/equip_java7_64.sh && bash equip_java7_64.sh
java -version
sudo apt-get install unzip

sudo mkdir /opt/wso2

sudo cp /vagrant/packs/wso2as-5.2.1.zip /opt/wso2
cd /opt/wso2
unzip wso2as-5.2.1.zip
rm wso2as-5.2.1.zip

cp -rf /vagrant/config/as/* wso2as-5.2.1/
chown -R vagrant:vagrant wso2as-5.2.1
wget http://repo1.maven.org/maven2/mysql/mysql-connector-java/5.1.32/mysql-connector-java-5.1.32.jar -P wso2as-5.2.1/repository/components/lib/

sudo cp /vagrant/packs/wso2das-3.0.0.zip /opt/wso2
cd /opt/wso2
unzip wso2das-3.0.0.zip
rm wso2das-3.0.0.zip

cp -rf /vagrant/config/das/* wso2das-3.0.0/
wget http://repo1.maven.org/maven2/mysql/mysql-connector-java/5.1.32/mysql-connector-java-5.1.32.jar -P wso2das-3.0.0/repository/components/lib/
chown -R vagrant:vagrant wso2das-3.0.0

sudo cp /vagrant/packs/wso2am-1.9.1.zip /opt/wso2
cd /opt/wso2
unzip wso2am-1.9.1.zip
rm wso2am-1.9.1.zip
cp -rf /vagrant/config/am/* wso2am-1.9.1/
wget http://repo1.maven.org/maven2/mysql/mysql-connector-java/5.1.32/mysql-connector-java-5.1.32.jar -P wso2am-1.9.1/repository/components/lib/
chown -R vagrant:vagrant wso2am-1.9.1
cd /opt/wso2

sudo cp /vagrant/config/wso2as /etc/init.d/wso2as
sudo chmod 755 /etc/init.d/wso2as
sudo /etc/init.d/wso2as start

sudo cp /vagrant/config/wso2das /etc/init.d/wso2das
sudo chmod 755 /etc/init.d/wso2das
sudo /etc/init.d/wso2das start

sudo cp /vagrant/config/wso2am /etc/init.d/wso2am
sudo chmod 755 /etc/init.d/wso2am
sudo /etc/init.d/wso2am start

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
          n.name = "mysql-master.tmwsystems.com"
          n.memory = 512
      end
      mysql.vm.hostname = "mysql-master.tmwsystems.com"
      mysql.vm.network :private_network, ip: "192.168.11.10"
      mysql.vm.provision :shell, :inline => $mysql_script
    end    

    config.vm.define "is" do |is|
      is.vm.provider :virtualbox do |n|
          n.name = "is-master.tmwsystems.com"
          n.memory = 4096
      end
      is.vm.hostname = "is-master.tmwsystems.com"
      is.vm.network :private_network, ip: "192.168.11.11"
      is.vm.provision :shell, :inline => $is_script
    end

    config.vm.define "as" do |as|
      as.vm.provider :virtualbox do |n|
        n.name = "as-master.tmwsystems.com"
        n.customize ["modifyvm", :id, "--memory", "4096"]
      	n.customize ["modifyvm", :id, "--cpus", "1"]
      	n.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
      	n.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
      	n.customize ["modifyvm", :id, "--ostype", "Ubuntu_64"]
      	n.customize ["modifyvm", :id, "--acpi", "on"]
      	n.customize ["modifyvm", :id, "--ioapic", "on"]
      end
      as.vm.hostname = "as-master.tmwsystems.com"
      as.vm.network :private_network, ip: "192.168.11.12"
      as.vm.provision :shell, :inline => $as_script
    end

end

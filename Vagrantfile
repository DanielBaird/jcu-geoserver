# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|

  config.vm.box = "ubuntu/trusty64"

  config.vm.network "public_network", bridge: "en2: Thunderbolt Ethernet"
  config.vm.network "forwarded_port", guest: 80, host: 8080

  # config.vm.synced_folder "../data", "/vagrant_data"

  config.vm.provider "virtualbox" do |vb|
    # vb.gui = true
    vb.memory = "3000"
  end

  config.vm.provision "shell", inline: <<-SHELL
    apt-get -y -q update
    apt-get -y -q install software-properties-common htop

    # get oracle java 7
    add-apt-repository ppa:webupd8team/java
    apt-get -y -q update
    echo oracle-java7-installer shared/accepted-oracle-license-v1-1 select true | sudo /usr/bin/debconf-set-selections
    apt-get -y -q install oracle-java7-installer
    # add JAVA_HOME
    echo " " >> ~/.profile
    echo "export JAVA_HOME=/usr/lib/jvm/java-7-oracle" >> ~/.profile

    # get geoserver
    wget -q http://downloads.sourceforge.net/project/geoserver/GeoServer/2.7.1.1/geoserver-2.7.1.1-bin.zip?r=http%3A%2F%2Fsourceforge.net%2Fprojects%2Fgeoserver%2Ffiles%2FGeoServer%2F2.7.1.1%2F&ts=1438140277 -o geoserver.zip
    mkdir /usr/share/geoserver
    # chown `whoami`:`whoami` /usr/share/geoserver
    unzip geoserver.zip -d /usr/share/geoserver
    # add GEOSERVER_HOME
    echo " " >> ~/.profile
    echo "export GEOSERVER_HOME=/usr/share/geoserver/geoserver-2.7.1.1" >> ~/.profile

    # install postgres and postgis
    add-apt-repository -y ppa:ubuntugis/ubuntugis-unstable
    apt-get -y -q update
    apt-get -y -q install postgresql postgresql-contrib postgis



  SHELL
end

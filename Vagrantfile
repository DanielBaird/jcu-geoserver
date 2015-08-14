# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|

  # config.vm.box = "chef/centos-6.6"
  config.vm.box = "puppetlabs/centos-6.6-64-nocm"

  config.vm.network "public_network", bridge: "en2: Thunderbolt Ethernet"
  config.vm.network "forwarded_port", guest: 80, host: 7070

  # config.vm.synced_folder "../data", "/vagrant_data"

  config.vm.provider "virtualbox" do |vb|
    # vb.gui = true
    vb.memory = "3000"
  end

  config.vm.provision "shell", inline: <<-SHELL

    yum -y check-update # apt-get -y -q update
    # apt-get -y -q install software-properties-common htop

    # -- make the geoserver and eresearch users -----
    useradd geoserver
    useradd eresearch

    # # -- get oracle java 7 -----
    # # Yes there's other Java options but Geoserver prefers Oracle Java 7.
    mkdir /var/installers
    wget --no-cookies --no-check-certificate --header "Cookie: gpw_e24=http%3A%2F%2Fwww.oracle.com%2F; oraclelicense=accept-securebackup-cookie" \
        "http://download.oracle.com/otn-pub/java/jdk/7u80-b15/jre-7u80-linux-x64.tar.gz" \
        -O /var/installers/jre-7u80.tgz
    tar xvf /var/installers/jre-7u80.tgz -C /opt
    chown -R root: /opt/jre1.7.0_80
    # tell system about the java binary
    sudo alternatives --install /usr/bin/java java /opt/jre1.7.0_80/bin/java 1

    # add JAVA_HOME env var for users
    echo " " >> /home/eresearch/.profile
    echo "export JAVA_HOME=/opt/jre1.7.0_80" >> /home/eresearch/.profile

    echo " " >> /home/geoserver/.profile
    echo "export JAVA_HOME=/opt/jre1.7.0_80" >> /home/geoserver/.profile

    # -- get geoserver -----
    wget -q http://downloads.sourceforge.net/project/geoserver/GeoServer/2.7.1.1/geoserver-2.7.1.1-bin.zip?r=http%3A%2F%2Fsourceforge.net%2Fprojects%2Fgeoserver%2Ffiles%2FGeoServer%2F2.7.1.1%2F&ts=1438140277 \
        -O /var/installers/geoserver.zip
    mkdir /usr/share/geoserver
    # chown `whoami`:`whoami` /usr/share/geoserver
    unzip geoserver.zip -d /usr/share/geoserver
    # add GEOSERVER_HOME env var for us and geoserver
    echo " " >> ~/.profile
    echo " " >> ~/../geoserver/.profile
    echo "export GEOSERVER_HOME=/usr/share/geoserver/geoserver-2.7.1.1" >> ~/.profile
    echo "export GEOSERVER_HOME=/usr/share/geoserver/geoserver-2.7.1.1" >> ~/../geoserver/.profile

    # # -- get postgres and postgis -----
    # add-apt-repository -y ppa:ubuntugis/ubuntugis-unstable
    # apt-get -y -q update
    # apt-get -y -q install postgresql postgresql-client postgresql-contrib postgis

    # # -- set up postgres / postgis -----
    # # eresearch superuser
    # sudo -u postgres -- createuser -s eresearch
    # # make a geoserver user, and db, and get it ready to GIS
    # sudo -u eresearch -- createuser geoserver
    # sudo -u eresearch -- createdb geoserver
    # sudo -u eresearch -- psql -d geoserver -c 'create extension postgis;'

  SHELL
end

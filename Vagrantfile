# -*- mode: ruby -*-
# vi: set ft=ruby :

# All Vagrant configuration is done below. The "2" in Vagrant.configure
# configures the configuration version (we support older styles for
# backwards compatibility). Please don't change it unless you know what
# you're doing.
Vagrant.configure("2") do |config|
    # The most common configuration options are documented and commented below.
    # For a complete reference, please see the online documentation at
    # https://docs.vagrantup.com.

    # Every Vagrant development environment requires a box. You can search for
    # boxes at https://vagrantcloud.com/search.
    config.vm.box = "kkdt/g7dev"
    config.vm.box_version = "0.1"
    config.vm.define "g7dev"
    config.vm.hostname = "g7dev"

    config.vm.provider "virtualbox" do |vb|
        vb.gui = false
        vb.name = "g7dev"
        vb.memory = 1024
        vb.cpus = 1
    end

    # NGINX will be running on port 3000 on the guest. This will be the reverse
    # proxy into all the backend services deployed on the guest.
    config.vm.network "forwarded_port", host: 8000, guest: 3000

    if(File.exist?('aws.cfadmin'))
        config.vm.provision "aws", type: "shell", inline: <<-SHELL
            downloadLocation=$HOME/downloads/aws
            mkdir -p $downloadLocation
            awszip="https://s3.amazonaws.com/aws-cli/awscli-bundle.zip"

            echo "Downloading AWS CLI"
            wget -N $awszip --directory-prefix=$downloadLocation -nv

            echo "Extracting AWS CLI $filename to /opt"
            filename=$(ls $downloadLocation | grep zip)
            unzip $downloadLocation/$filename -d $downloadLocation

            echo "Installing AWS CLI to /usr/local/aws and /opt/bin/aws"
            $downloadLocation/awscli-bundle/install -i /usr/local/aws -b /opt/bin/aws
            aws --version

            cat /vagrant/aws.cfadmin >> ~vagrant/.bashrc
        SHELL
    end

    # Update new software/cots as needed rather than rebuilding the box image.
    config.vm.provision "software", type: "shell", inline: <<-SHELL
        #npm -g install express
        #npm -g install http-server
    SHELL

end
# -*- mode: ruby -*-
# vi set ft=ruby :

Vagrant.configure(2) do |config|

    config.vm.provider "virtualbox"
    config.vm.provider "virtualbox" do |v|
        v.memory = 3072
        v.cpus = 2
    end

    config.vm.box = "bento/centos-7.1"
    config.vm.network "forwarded_port", guest: 8080, host: 5555, host_ip: "127.0.0.1"
    config.vm.network "forwarded_port", guest: 80, host: 6666, host_ip: "127.0.0.1"
    config.vm.synced_folder ".", "/home/vagrant/shared"
    config.vm.provision "shell", inline: <<-SHELL

        sudo yum update -y
        sudo yum install -y wget

        sudo wget https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
        sudo yum install epel-release-latest-7.noarch.rpm -y
        sudo yum update -y

        sudo yum install curl vim git unzip python python-devel python-pip openssl ansible -y

        sudo pip install "markupsafe==1.1.1"
        sudo pip install "Jinja2==2.8"

        # Clone repo and start the app
        sudo rm -rf /home/centos/jenkins
        sudo git clone https://github.com/aws-samples/aws-install-sap-with-jenkins-ansible.git /home/centos/jenkins

        # Run playbook
        sudo ansible-playbook /home/centos/jenkins/jenkins-as-code/site.yml
        
        # Wait for jenkins to restart
        sleep 30s

        # Check if the service is up, running and responding
        sudo bash /home/centos/jenkins/jenkins-as-code/general-test.sh
    SHELL
end
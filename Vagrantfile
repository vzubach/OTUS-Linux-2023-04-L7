# -*- mode: ruby -*-
# vi: set ft=ruby : vsa

Vagrant.configure(2) do |config| 
 config.vm.box = "centos/8" 
 config.vm.box_version = "2011.0" 
 config.vm.provider "virtualbox" do |v| 
 v.memory = 1024
 v.cpus = 2 
 end 
 config.vm.define "repos" do |repos| 
 repos.vm.network "private_network", ip: "192.168.56.10"
 repos.vm.hostname = "repos" 
 end
   config.vm.provision "file", source: "nginx-1.24.0-1.el8.ngx.x86_64.rpm", destination: "/home/vagrant/nginx-1.24.0-1.el8.ngx.x86_64.rpm"
   config.vm.provision "file", source: "percona-orchestrator-3.2.6-2.el8.x86_64.rpm", destination: "/home/vagrant/percona-orchestrator-3.2.6-2.el8.x86_64.rpm"
   config.vm.provision "shell", inline: <<-SHELL
      cd /etc/yum.repos.d/
      sed -i 's/mirrorlist/#mirrorlist/g' /etc/yum.repos.d/CentOS-*
      sed -i 's|#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g' /etc/yum.repos.d/CentOS-*
      yum install -y createrepo
      sudo yum localinstall -y /home/vagrant/nginx-1.24.0-1.el8.ngx.x86_64.rpm
      sudo mkdir /usr/share/nginx/html/repos
      sudo mv /home/vagrant/nginx-1.24.0-1.el8.ngx.x86_64.rpm /usr/share/nginx/html/repos/
      sudo mv /home/vagrant/percona-orchestrator-3.2.6-2.el8.x86_64.rpm /usr/share/nginx/html/repos/
      sudo createrepo /usr/share/nginx/html/repos/
      sudo sed -i '9 G' /etc/nginx/conf.d/default.conf
      sudo sed -i '10 s/^/\tautoindex on\;/' /etc/nginx/conf.d/default.conf  
      sudo systemctl start nginx
      sudo touch /etc/yum.repos.d/otus-lesson-7.repo
      sudo su -c "cat >> /etc/yum.repos.d/otus-lesson-7.repo << EOF
[otus-lesson-7]
name=otus-linux
baseurl=http://localhost/repos
gpgcheck=0
enabled=1
EOF"
      echo "yum repolist enabled | grep otus"
      yum repolist enabled | grep otus
      sleep 3
      echo "yum list | grep otus"
      yum list | grep otus
   SHELL
end

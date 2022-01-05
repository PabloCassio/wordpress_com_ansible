
Vagrant.configure("2") do |config|
  #box vm
  config.vm.box = "ubuntu/bionic64"
  #link da vm (problema de ssl)
  config.vm.box_download_insecure = " https://vagrantcloud.com/ubuntu/boxes/bionic64/versions/20211216.0.0/providers/virtualbox.box"

  config.vm.provider "virtualbox" do |v|
	v.memory = 1024
  end

  config.vm.define "wordpress" do |m|
	m.vm.network "public_network", ip: "192.168.101.30"
  m.vm.provision "shell",
    inline: "sudo cp /vagrant/id_wdps.pub /home/vagrant"
  m.vm.provision "shell",
      inline: "cat /home/vagrant/id_wdps.pub >> ~/.ssh/authorized_keys "
  end


  config.vm.define "mysql" do |mysql|
  mysql.vm.network "public_network", ip: "192.168.101.32"
  mysql.vm.provision "shell",
    inline: "sudo cp /vagrant/id_msql.pub /home/vagrant"
  mysql.vm.provision "shell",
      inline: "cat /home/vagrant/id_msql.pub >> ~/.ssh/authorized_keys "
  end

  config.vm.define "ansible" do |ansible|
  ansible.vm.network "public_network", ip: "192.168.101.31"
  ansible.vm.provision "shell",
    inline: "sudo cp /vagrant/.vagrant/machines/mysql/virtualbox/private_key /home/vagrant/id_mysql"
  ansible.vm.provision "shell",
    inline: "sudo cp /vagrant/.vagrant/machines/wordpress/virtualbox/private_key /home/vagrant/id_wordpress"
  ansible.vm.provision "shell",
      inline: "sudo chmod 700 id_mysql"
  ansible.vm.provision "shell",
          inline: "sudo chmod 700 id_wordpress"

  ansible.vm.provision "shell",
      inline: "apt-get update && \
                 apt-get install -y software-properties-common && \
                 apt-add-repository --yes --update ppa:ansible/ansible && \
                 apt-get install -y ansible"

  end

end

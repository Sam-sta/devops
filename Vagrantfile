Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/focal64"
  config.vm.synced_folder ".", "/vagrant", disabled: true
  config.vm.box_check_update = true
  config.vm.network "forwarded_port", guest: 8080, host: 8090
  config.vm.network "forwarded_port", guest: 8081, host: 8091
  config.vm.provider "virtualbox" do |vb|
    vb.gui = false
    vb.name = "ATLatamVM"
    vb.cpus = 1
    vb.memory = "2048"
  end
  config.vm.provision "shell", inline: <<-SHELL
    sudo sysctl -w vm.max_map_count=262144
    sudo curl -L "https://github.com/docker/compose/releases/download/1.29.2/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
    sudo chmod +x /usr/local/bin/docker-compose
    cd Documents
    git clone https://github.com/Sam-sta/devops.git
    cd devops
    sudo docker-compose up
  SHELL
end

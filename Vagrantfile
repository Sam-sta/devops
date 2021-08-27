Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/focal64"
  config.vm.synced_folder ".", "/vagrant", disabled: true
  config.vm.box_check_update = true

  #Port to check http requests from app
  config.vm.network "forwarded_port", guest: 3000, host: 3000

  #Ports for jenkins
  config.vm.network "forwarded_port", guest: 8080, host: 8080
  config.vm.network "forwarded_port", guest: 50000, host 50000
  config.vm.provider "virtualbox" do |vb|
    vb.gui = false
    vb.name = "MSMjenkins"
    vb.cpus = 2
    vb.memory = "8190"
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

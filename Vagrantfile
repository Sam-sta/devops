Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/focal64"
  config.vm.synced_folder ".", "/vagrant", disabled: true
  config.vm.box_check_update = true

  #Port to check http requests from app
  config.vm.network "forwarded_port", guest: 3000, host: 3000

  #Ports for jenkins
  config.vm.network "forwarded_port", guest: 8080, host: 8080
  config.vm.network "forwarded_port", guest: 50000, host: 50000

  #Port for sonarqube
  config.vm.network "forwarded_port", guest: 8090, host: 8090

  #Ports for nexus
  config.vm.network "forwarded_port", guest: 8081, host: 8081
  config.vm.network "forwarded_port", guest: 8082, host: 8082
  config.vm.network "forwarded_port", guest: 8083, host: 8083

  config.vm.provider "virtualbox" do |vb|
    vb.gui = false
    vb.name = "MSMjenkins"
    vb.cpus = 2
    vb.memory = "8190"
  end

  config.vm.provision "compose", type: "file", source: "docker-compose.yml", destination: "$HOME/stack/docker-compose.yml"

  config.vm.provision "shell", path: "provision-dockerinstall.sh"

  config.vm.provision "shell", path: "provision-dockerCompose-install.sh"

  config.vm.provision "compose-up", type: "shell", path: "provision-dockerCompose-up.sh"

  config.vm.provision "java-install", type:"shell", path: "provision-javaInstall.sh"

  #Uncomment and add container name to search password
  #config.vm.provision "jenkinslogs", type: "shell", inline: "docker logs -f stack_jenkins_1"
end
# -*- mode: ruby -*-
# vi: set ft=ruby :

ENV['VAGRANT_NO_PARALLEL'] = 'yes'

VAGRANT_BOX         = "generic/ubuntu2204"
VAGRANT_BOX_VERSION = "4.2.10"
CPUS_JENKINS_NODE   = 2
CPUS_DOCKER_NODE    = 2
MEMORY_JENKINS_NODE  = 4096
MEMORY_DOCKER_NODE  = 4096
DOCKER_NODES_COUNT  = 2


Vagrant.configure("2") do |config|
  # Jenkins VM configuration
  config.vm.define "jenkins" do |jenkins|
    jenkins.vm.box = VAGRANT_BOX
    jenkins.vm.box_version = VAGRANT_BOX_VERSION
    jenkins.vm.network "forwarded_port", guest: 8080, host: 8080
    jenkins.vm.network "forwarded_port", guest: 3000, host: 3000
    jenkins.vm.network "forwarded_port", guest: 3100, host: 3100
    jenkins.vm.provider :virtualbox do |vb|
      vb.gui = true
      vb.cpus = CPUS_JENKINS_NODE   #4
      vb.linked_clone = false
    end
    jenkins.vm.provision "shell" do |shell|
      shell.path = "jenkins.sh"
    end
  end

  # Docker VM configuration
  config.vm.define "docker" do |docker|
    docker.vm.box = VAGRANT_BOX
    docker.vm.box_version = VAGRANT_BOX_VERSION
    docker.vm.synced_folder "shared/", "/shared"
    docker.vm.network "forwarded_port", guest: 8080, host: 8081 # Changed host port to avoid conflict with Jenkins
    # open docker remote API port and hostport range 
    docker.vm.provider :virtualbox do |vb|
      vb.gui = true
      vb.cpus = CPUS_DOCKER_NODE   #4
      vb.linked_clone = false
    end
    docker.vm.provision "shell" do |shell|
      shell.path = "docker.sh"
    end
  end
end

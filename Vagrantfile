# ##################### Vagrantfile #################################
#
# This is the main Vagrantfile that automates the provisioning of
# VMs and services
#
# ###################################################################

# ##### Set required versions ######

Vagrant.require_version ">= 2.0.0"
require 'yaml'
settings = YAML.load_file 'vagrant.yml'

# ###### Bring up base box #########

Vagrant.configure("2") do |config|
    
    ####### VM 1 Setup  ########
    config.vm.define "basevm1" do |basevm1|
       
        basevm1.vm.box = "bento/ubuntu-18.04"
        basevm1.vm.hostname =  settings['basevm1_name']
        basevm1.vm.provider :virtualbox do |vb|
              vb.customize ["modifyvm", :id, "--memory", "2048", "--cpus", "2"]
        end
        basevm1.vm.network :private_network, ip: settings['basevm1ip_address']

        # Inject templates to the VM
        basevm1.vm.provision "file", source: "./templates/", destination: "/var/tmp/elk"

        basevm1.vm.provision "ansible_local" do |ansible|
            #https://github.com/hashicorp/vagrant/issues/9796
            ansible.install_mode = "pip"
            ansible.verbose = true
            ansible.playbook = "provision/playbook.yml"
            ansible.limit = "basevm1"
        end

	end

end

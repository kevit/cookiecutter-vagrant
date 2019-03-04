# original idea from https://github.com/raulmartinezm/cookiecutter-vagrant
#
require 'yaml'

# Specify Vagrant version and Vagrant API version
Vagrant.require_version ">= 1.6.0"
VAGRANTFILE_API_VERSION = "2"

#DEFAULT_MEMORY = {{ cookiecutter.default_memory }}
#DEFAULT_CPUS = {{ cookiecutter.default_cpus }}
DEFAULT_MEMORY = 1024
DEFAULT_CPUS = 1
ENV['VAGRANT_NO_PARALLEL'] = 'yes'

# Load plugins definition
plugins_config = File.expand_path('./Vagrantfile.plugins', __FILE__)
load plugins_config if File.exists?(plugins_config)

# Load machine definition
machines = YAML.load_file('machine_inventory.yml')
HOSTS = ["all", "hosts"]


# Vagrant configuration section.
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
    machines.dig(*HOSTS).each do |machine|
        config.vm.define machine[0] do |m|
            # Configure virtual machine.
            m.vm.box = machine[1]['box']
            m.vm.network :private_network, ip: machine[1]["ansible_host"]

#            m.vm.provider :virtualbox do |vb|
#                vb.memory = machine[1]['memory'] ||= DEFAULT_MEMORY
#                vb.cpus = machine[1]['cpus'] ||= DEFAULT_CPUS
#                vb.customize ["modifyvm", :id, "--cableconnected1", "on"]
#                vb.customize ["modifyvm", :id, "--nictype1", "Am79C973"]
#            end
            m.vm.provider :libvirt do |lv|
             lv.memory = machine[1]['memory'] ||= DEFAULT_MEMORY
           end

            m.vm.provision "ansible" do |ansible|
#more details here https://www.vagrantup.com/docs/provisioning/ansible_common.html
              ansible.playbook = machine[1]['playbook']
              ansible.galaxy_roles_path = "roles"
#            ansible.inventory_path = "scripts/vagrant.py" 
              ansible.inventory_path = "machine_inventory.yml"
         #  ansible.verbose = "vvv"
              ansible.become = true
            end
        end

    end

end

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
machines = YAML.load_file('machine_definition.yml')


# Vagrant configuration section.
Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
    machines.each do |machine|
        config.vm.define machine['name'] do |m|
            # Configure virtual machine.
            m.vm.box = machine['box']
            m.vm.network :private_network, ip: machine["ip"]
            m.vm.provider :virtualbox do |vb|
                vb.memory = machine['memory'] ||= DEFAULT_MEMORY
                vb.cpus = machine['cpus'] ||= DEFAULT_CPUS
                vb.customize ["modifyvm", :id, "--cableconnected1", "on"]
                vb.customize ["modifyvm", :id, "--nictype1", "Am79C973"]
            end

            m.vm.provision "ansible" do |ansible|
#more details here https://www.vagrantup.com/docs/provisioning/ansible_common.html
            ansible.playbook = machine['playbook']
            ansible.galaxy_roles_path = "roles"
            ansible.inventory_path = "scripts/vagrant.py" 
         #  ansible.verbose = "vvv"
            ansible.become = true
            end
        end
    end
end

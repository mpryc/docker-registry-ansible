Vagrant.require_version ">= 1.7.0"

ENV["LC_ALL"] = "en_US.UTF-8"

API_VERSION = "2"

Vagrant.configure(API_VERSION) do |config|
    config.vm.define :docker_registry do |docker_registry|

        docker_registry.vm.box = "fedora/23-cloud-base"
        docker_registry.vm.hostname = "docker-registry.vm"
        docker_registry.vm.network :private_network, 
          :ip => "192.168.124.124",
          :libvirt__network_name => "default"

        docker_registry.vm.provider :libvirt do |libvirt|
            libvirt.driver = "kvm"
            libvirt.graphics_type = "vnc"
            libvirt.connect_via_ssh = false
            libvirt.username = "root"
            libvirt.cpus = 2
            libvirt.memory = 2048
        end
        
        docker_registry.ssh.insert_key = false

        # Disable the default /vagrant share
        docker_registry.vm.synced_folder "./", "/vagrant", disabled:true

    end

    # Install packages that are required for ansible
    # Prefer to have it in vagrant rather then ansible
    # as this is really per platform specific requirement.
    #
    # See: https://fedorahosted.org/cloud/ticket/126
    config.vm.provision :shell do |shell|
        shell.inline = "sudo $1"
        shell.args = "'dnf install -y python python-dnf polkit'"
    end

    #
    # Run Ansible from the Vagrant Host
    #
    config.vm.provision :ansible do |ansible|
        ansible.playbook = "docker-registry.yml"
    end

end

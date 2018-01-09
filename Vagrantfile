# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure(2) do |config|

    # Ubuntu 16.04 Xenial LTS 64-bit
    config.vm.box = "apolloclark/ubuntu1604-64"
    # config.vm.box_version = "20170926"

    # VirtualBox Provider-specific configuration
    config.vm.provider "virtualbox" do |vb, override|

        # set the VM name
        vb.name = "packer-aws-webapp"

        # set the CPU, memory, graphics
        # @see https://www.virtualbox.org/manual/ch08.html
        vb.cpus = 1
        vb.memory = "2048"
        vb.gui = false

        # Share a folder to the guest VM, types: docker, nfs, rsync, smb, virtualbox
        # Windows supports: smb
        # Mac supports: rsync, nfs
        # override.vm.synced_folder host_folder.to_s, guest_folder.to_s, type: "smb"
        override.vm.synced_folder "./ansible", "/vagrant"
        override.vm.synced_folder "./data", "/var/www/html"
            # owner: "vagrant", group: "vagrant",
            # mount_options: ['dmode=777','fmode=777']

        # Create a forwarded port mapping which allows access to a specific port
        # within the machine from a port on the host machine. In the example below,
        # accessing "localhost:8080" will access port 80 on the guest machine.
        # https://www.vagrantup.com/docs/networking/forwarded_ports.html
        override.vm.network "forwarded_port", guest: 80,   host: 8080, auto_correct: true
        override.vm.network "forwarded_port", guest: 3306, host: 3306, auto_correct: true
        override.vm.network "forwarded_port", guest: 5601, host: 5601, auto_correct: true

        # setup local apt-get cache
        if Vagrant.has_plugin?("vagrant-cachier")
            # Configure cached packages to be shared between instances of the same base box.
            # https://github.com/fgrehm/vagrant-cachier
            # More info on the "Usage" link above
            override.cache.scope = :box
        end
    end
      
    # Configure the box with Ansible, running within the Guest Machine
    # https://www.vagrantup.com/docs/provisioning/ansible.html
    # https://www.vagrantup.com/docs/provisioning/ansible_common.html
    config.vm.provision "ansible" do |ansible|
        ansible.galaxy_role_file = "./ansible/requirements.yml"
        ansible.playbook = "./ansible/playbook.yml"
    end
end
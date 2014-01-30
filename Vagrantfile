# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"
static_ip = "192.168.111.188"
box_name = "node"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|

  config.vm.box = "node"
  #config.vm.box_url = "http://brennovich.s3.amazonaws.com/saucy64_vmware_fusion.box"

  #config.ssh.forward_agent = true

    config.vm.network :forwarded_port, guest: 80, host: 3000
    config.vm.network :forwarded_port, guest: 8080, host: 8080
    config.vm.network :forwarded_port, guest: 1080, host: 3001
    config.vm.network :forwarded_port, guest: 443, host: 443
    config.vm.network :forwarded_port, guest: 2525, host: 2525

    #might we need this for nfs?
    config.vm.network :private_network, ip: "#{static_ip}"

    #copy user settings folder at some point

    # share a local code folder for editing in a native env
    config.vm.synced_folder "/code/vagrant/#{box_name}/code", "/code", :nfs => true

    #TODO: include the node module path, too

    # share your rvm path so we can search gem file code with vim
    config.vm.synced_folder "/code/vagrant/#{box_name}/vm-rvm-path", "/home/vagrant/.rvm", :nfs => true

    # The environment key to set
    # this lets the vm know where the files are for the better_errors gem.
    config.host_path.env_key = "http_VAGRANT_HOST_PATH"

    config.host_path.path_file = "/tmp/.vagrant-host-path"

    # Profile script path
    config.host_path.profile_path = "/etc/profile.d/vagrant-host-path.sh"

    #provisions the environment
    config.vm.provision "ansible" do |ansible|
      #ansible.raw_arguments = "-i provisioning/hosts"
      ansible.playbook = "provisioning/mean.yml"
      ansible.inventory_path = "provisioning/hosts"
      ansible.verbose = "vvv"
      ansible.extra_vars = { vagrant_static_ip: "#{static_ip}" }
    end
end

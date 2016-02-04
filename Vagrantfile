Vagrant.configure("2") do |config|
  config.vm.box       = "onemanstartup/ubuntu14"
  config.vm.box_url   = "onemanstartup/ubuntu14"
  config.vm.host_name = "production"
  # config.ssh.private_key_path = "~/.ssh/id_rsa"
  config.ssh.forward_agent = true

  private_ip = "192.168.13.37"
  config.vm.network(:private_network, :ip => private_ip)

  config.vm.provision :ansible do |ansible|
    ansible.extra_vars = {
      ansible_connection: 'ssh',
      ansible_ssh_args: '-o ForwardAgent=yes'
    }
    ansible.verbose = 'vvv'
    ansible.playbook = "ansible/rails-app-local.yml"
    ansible.inventory_path = 'ansible/production'
    ansible.limit = 'all'
    ansible.tags = 'monitoring_new'
  end

  if defined?(VagrantPlugins::HostsUpdater)
    config.hostsupdater.aliases = ["local-seer.slickage.com"]
  end
end

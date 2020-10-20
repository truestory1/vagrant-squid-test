# VAGRANT_DISABLE_VBOXSYMLINKCREATE = "1"
servers=[
  {
    :hostname => "client",
    :ip => "192.168.55.1",
    :ram => 1024,
    :cpu => 1,
    :playbook => "playbooks/client.yml"
  },
  {
    :hostname => "server",
    :ip => "192.168.55.2",
    :ram => 1024,
    :cpu => 2,
    :playbook => "playbooks/server.yml"
  },
  {
    :hostname => "proxy",
    :ip => "192.168.55.3",
    :ram => 1024,
    :cpu => 1,
    :playbook => "playbooks/proxy.yml"
  },
]

Vagrant.configure(2) do |config|
  # Use same SSH key for each machine
  config.ssh.insert_key = false
  config.vm.box_check_update = false
  config.vm.synced_folder ".", "/vagrant", type: "rsync"
  servers.each do |machine|
    config.vm.define machine[:hostname] do |node|
      node.vm.box = "centos/8"
      node.vm.hostname =  machine[:hostname]
      node.vm.network "private_network", ip: machine[:ip]
      node.vm.provision :ansible_local do |ansible|
        # ansible.install_mode = "pip"
        ansible.playbook = machine[:playbook]
        ansible.galaxy_role_file = "playbooks/requirements.yml"
        ansible.galaxy_command = 'ansible-galaxy collection install -r %{role_file}'
      end #vm.provision ansible

      node.vm.provider "virtualbox" do |vb|
        vb.memory = machine[:ram]
        vb.cpus = machine[:cpu]
      end #vb
    end #node
  end #machine
end #config

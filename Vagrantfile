Vagrant.configure(2) do |config|

  config.vm.define "node1" do |node1|
      node1.vm.box = "chef/centos-7.0"
      node1.vm.network  "private_network", ip: "192.168.60.10"
      node1.vm.network  "forwarded_port", guest: 80, host: 8080
      node1.vm.hostname = "node1"
  end
 
  config.vm.define "node2" do |node2|
      node2.vm.box = "chef/centos-7.0"
      node2.vm.network  "private_network", ip: "192.168.60.11"
      node2.vm.hostname = "node2"
  end

  config.vm.define "node3" do |node3|
      node3.vm.box = "chef/centos-7.0"
      node3.vm.network  "private_network", ip: "192.168.60.12"
      node3.vm.hostname = "node3"
  end

  config.vm.provision "ansible" do |ansible|
    ansible.playbook = "site.yml"
    ansible.groups = {
      "server" => ["node1"],
      "proxy"  => ["node2"],
      "agent"  => ["node3"],
    }
  end
end

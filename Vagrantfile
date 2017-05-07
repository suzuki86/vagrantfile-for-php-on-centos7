Vagrant.configure("2") do |config|
  config.hostmanager.ip_resolver = proc do |machine|
    result = ""
    machine.communicate.execute("ip addr show eth1") do |type, data|
      result << data if type == :stdout
    end
    (ip = /inet (\d+\.\d+\.\d+\.\d+)/.match(result)) && ip[1]
  end

  config.hostmanager.enabled = true
  config.hostmanager.manage_host = true
  config.hostmanager.ignore_private_ip = false
  config.hostmanager.include_offline = false

  config.vm.box = "centos/7"

  config.vm.define "YOUR_MACHINE_NAME" do |node|
    node.vm.hostname = "YOUR_HOST_NAME"
    node.vm.network "private_network", type: "dhcp"
    node.vm.provision "ansible_local" do |ansible|
      ansible.playbook = "ansible/main.yml"
      ansible.install = true
      ansible.verbose = "vvv"
    end
  end

  config.vm.provider "virtualbox"
end

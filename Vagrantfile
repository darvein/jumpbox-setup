nodes = [
  { :hostname => "bastion"   , :box => "ubuntu/trusty64" , :ip0 => "10.10.10.101" , :ip1 => "192.168.57.201"  , :ip2 => "192.168.58.201" } ,
  { :hostname => "wordpress" , :box => "ubuntu/trusty64" , :ip0 => "10.10.10.102" , :ip1 => "192.168.57.202"} ,
  { :hostname => "spring"    , :box => "ubuntu/trusty64" , :ip0 => "10.10.10.103" , :ip1 => "192.168.57.203"} ,
  { :hostname => "bob"       , :box => "ubuntu/trusty64" , :ip0 => "10.10.10.104" , :ip1 => "192.168.58.202"} ,
]

Vagrant.configure(2) do |config|
  nodes.each do |node|
    config.ssh.insert_key = false
    config.vm.provision "file",
      source: "~/.ssh/id_rsa.pub", 
      destination: "~/.ssh/authorized_keys"

    config.vm.define node[:hostname] do |box|
      box.vm.box = node[:box]
      box.vm.hostname = node[:hostname]
      box.vm.network :private_network, ip: node[:ip0]
      box.vm.network :private_network, ip: node[:ip1],
        virtualbox__intnet: true

      if node[:hostname] == "bastion"
        box.vm.network :private_network, ip: node[:ip2],
          virtualbox__intnet: true
      end
    end
  end

  config.vm.provision "shell", inline: <<-SHELL
    sudo echo "192.168.58.202 bob" >> /etc/hosts
    sudo echo "192.168.57.203 spring" >> /etc/hosts
    sudo echo "192.168.57.202 wordpress" >> /etc/hosts
    sudo echo "192.168.57.201 bastion" >> /etc/hosts
    sudo echo "192.168.58.201 bastion" >> /etc/hosts
    echo "set -o vi" >> /home/vagrant/.bashrc
  SHELL
end

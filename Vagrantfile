# -*- mode: ruby -*-
# vi: set ft=ruby :

# Primary namenodes range 101 - 110
# Jobtracker range 111 - 120
# Datanodes range 121 - 180
# Generally the tasktrackers will live on the datanodes

Vagrant::Config.run do |config|
  1.upto(1) do |n|
    id = n.to_s.rjust(3, '0')
    config.vm.define "primary_namenode_#{id}" do |nn|
      nn.vm.customize [ "modifyvm", :id, "--memory", 512 ]
      nn.vm.box = "centos_6_3"
      nn.vm.box_url = "https://dl.dropbox.com/u/7225008/Vagrant/CentOS-6.3-x86_64-minimal.box"
      nn.vm.network :hostonly, "192.168.33.#{100 + n}"
      nn.vm.host_name = "primary-namenode-#{id}.hadoop.dev"
      #nn.vm.provision :shell, :path => "script/bootstrap-vm.sh"
      nn.vm.provision :puppet, :module_path => "modules" do |p|
	p.manifests_path = "manifests"
	p.manifest_file  = "site.pp"
      end
    end
  end

  1.upto(3) do |n|
    id = n.to_s.rjust(3, '0')
    config.vm.define "datanode_#{id}" do |dn|
      dn.vm.customize [ "modifyvm", :id, "--memory", 512 ]
      dn.vm.box = "centos_6_3"
      dn.vm.box_url = "https://dl.dropbox.com/u/7225008/Vagrant/CentOS-6.3-x86_64-minimal.box"
      dn.vm.network :hostonly, "192.168.33.#{120+n}"
      dn.vm.host_name = "datanode-#{id}.hadoop.dev"
      #dn.vm.provision :shell, :path => "script/bootstrap-vm.sh"
      dn.vm.provision :puppet, :module_path => "modules" do |p|
	p.manifests_path = "manifests"
	p.manifest_file  = "site.pp"
      end
    end
  end
end

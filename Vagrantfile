# -*- mode: ruby -*-
# vi: set ft=ruby :

# Hadoop will run between .101 and .180:
# Primary namenodes range 101 - 110
# Jobtracker range 111 - 120
# Datanodes range 121 - 180
# Generally the tasktrackers will live on the datanodes

def define_vm config, role, index, ip
  id = (index + 1).to_s.rjust(3, '0')
  config.vm.define "#{role}_#{id}" do |box|
    box.vm.customize [ "modifyvm", :id, "--memory", 512 ]
    box.vm.box = "centos_6_3"
    box.vm.box_url = "https://dl.dropbox.com/u/7225008/Vagrant/CentOS-6.3-x86_64-minimal.box"
    box.vm.network :hostonly, "192.168.33.#{ip}", :netmask => "255.255.255.0"
    box.vm.host_name = "#{role.downcase.gsub(/[^a-z0-9]+/, '-')}-#{id}.hadoop.dev"
    #box.vm.provision :shell, :path => "script/bootstrap-vm.sh"
    box.vm.provision :puppet, :module_path => "modules" do |p|
      p.manifests_path = "manifests"
      p.manifest_file  = "site.pp"
    end
  end
end

roles = {
  'primary_namenode' => 101..101,
  'job_tracker'      => 111..111,
  'datanode'         => 121..123,
}

Vagrant::Config.run do |config|
  roles.each do |name, range|
    range.to_a.each do |n|
      define_vm config, name, n - range.first, n
    end
  end
end

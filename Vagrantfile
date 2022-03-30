disk = './sdc.vdi'
Vagrant.configure("2") do |config|
    node_name = "lvm"
    config.vm.synced_folder ".", "/vagrant", disabled: true
    config.vm.define node_name do |lvm|
        lvm.vm.box = "centos/7"
        lvm.vm.network "private_network", ip: "192.168.56.244"
        lvm.vm.hostname = node_name
        lvm.vm.provider "virtualbox" do |v|
            v.memory = 2048
            v.cpus = 2
            unless File.exist?(disk)
                v.customize ['createhd', '--filename', disk, '--variant', 'Fixed', '--size', 2 * 1024]  
            end
            v.customize ['storageattach', :id,  '--storagectl', 'IDE', '--port', 1, '--device', 0, '--type', 'hdd', '--medium', disk]
        end

        lvm.vm.provision :ansible do |ansible|
            ansible.limit = "all"
            ansible.become = true
            ansible.playbook = "lvm.yml"
        end
    end
end
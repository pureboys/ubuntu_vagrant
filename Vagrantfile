vm_list = [
    {
        :name => "master",
        :eth1 => "192.168.56.10",
        :mem => "8192",
        :cpu => "4",
        # :sshport => 22231
    },
    {
        :name => "node1",
        :eth1 => "192.168.56.11",
        :mem => "4096",
        :cpu => "2",
        # :sshport => 22232
    },
    {
      :name => "node2",
      :eth1 => "192.168.56.12",
      :mem => "4096",
      :cpu => "2",
      # :sshport => 22232
    }
]

Vagrant.configure(2) do |config|
    config.vm.box = "jammy"
    # 遍历虚拟机列表
    vm_list.each do |item|
        # 创建虚拟机级别的配置对象
        config.vm.define item[:name] do |vm_config|
            vm_config.vm.hostname = item[:name]
            vm_config.vm.network "private_network", ip: item[:eth1]
            # 禁用掉默认的SSH服务转发端口
            # vm_config.vm.network "forwarded_port", guest: 22, host: 2222, id: "ssh", disabled: "true"
            # vm_config.vm.network "forwarded_port", guest: 22, host: item[:sshport]
            vm_config.vm.provider "virtualbox" do |vb|
                vb.name = item[:name];
                vb.memory = item[:mem];
                vb.cpus = item[:cpu];
            end
        end
    end
end

Vagrant.configure('2') do |config|
	box_name = 'centos/7'
	#box_url = 'https://atlas.hashicorp.com/centos/boxes/7/versions/1607.01/providers/virtualbox.box'
	#config.vbguest.auto_update = false
	#config.vbguest.no_remote = true
	
	server_list = {
		'ans-tst-01' => {
			:ip => '192.168.250.11',
			:bridge_if => 'en4: Thunderbolt-Ethernet'
		},
		'ans-tst-02' => {
			:ip => '192.168.250.12',
			:bridge_if => 'en4: Thunderbolt-Ethernet'
		}
	}

	hosts_patch=''
	server_list.each do |s,o|
		hosts_patch << "#{o[:ip]},#{s};"
	end

	server_list.each do |server_name, options| 
		config.vm.define server_name do |server|
			server.vm.box = box_name
			#server.vm.box_url = box_url
			server.vm.hostname = server_name
			server.vm.network :public_network, ip: options[:ip], bridge: options[:bridge_if]
			server.vm.provider 'virtualbox' do |v|
				v.customize ['modifyvm', :id, '--memory', '1024']
				v.customize ['modifyvm', :id, '--cpus', '2']
				v.customize ['modifyvm', :id, '--ioapic', 'on']
				#v.gui = 'false'
			end
			server.vm.provision :shell, :path => 'bootstrap.sh', :args => "'#{hosts_patch}'"
			#server.vm.synced_folder "src/", "/data/src", mount_options: ["dmode=777","fmode=666"]
		end
	end
end

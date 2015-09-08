Vagrant.configure('2') do |config|
	box_name = 'opscode-centos-6.7'
	box_url = 'http://opscode-vm-bento.s3.amazonaws.com/vagrant/virtualbox/opscode_centos-6.7_chef-provisionerless.box'
	#config.vbguest.auto_update = false
	#config.vbguest.no_remote = true
	
	server_list = {
		'test-dev-1' => {
			:ip => '172.27.0.10'
		}
	}

	hosts_patch=''
	server_list.each do |s,o|
		hosts_patch << "#{o[:ip]},#{s};"
	end

	server_list.each do |server_name, options| 
		config.vm.define server_name do |server|
			server.vm.box = box_name
			server.vm.box_url = box_url
			server.vm.hostname = server_name
			server.vm.network :private_network, ip: options[:ip]
			server.vm.provider 'virtualbox' do |v|
				v.customize ['modifyvm', :id, '--memory', '1024']
				v.customize ['modifyvm', :id, '--cpus', '2']
				v.customize ['modifyvm', :id, '--ioapic', 'on']
				#v.gui = 'false'
			end
			server.vm.provision :shell, :path => 'bootstrap.sh', :args => "'#{hosts_patch}'"
			server.vm.synced_folder "src/", "/data/src", mount_options: ["dmode=777","fmode=666"]
		end
	end
end

# -*- mode: ruby -*-
# vim: set ft=ruby :

MACHINES = {
  :disktest => {
        :box_name => "centos/7",
        :ip_addr => '192.168.56.101',
	:disks => {
		:sata1 => {
			:dfile => './sata1.vdi',
			:size => 100,
			:port => 1
		},
		:sata2 => {
                        :dfile => './sata2.vdi',
                        :size => 100,
			:port => 2
		},
                :sata3 => {
                        :dfile => './sata3.vdi',
                        :size => 100,
                        :port => 3
                },
                :sata4 => {
                        :dfile => './sata4.vdi',
                        :size => 100,
                        :port => 4
                },
                :sata5 => {
                        :dfile => './sata5.vdi',
                        :size => 100,
                        :port => 5
                },
                :sata6 => {
                        :dfile => './sata6.vdi',
                        :size => 100,
                        :port => 6
                },
                :sata7 => {
                        :dfile => './sata7.vdi',
                        :size => 100,
                        :port => 7
                }

	}

		
  },
}

Vagrant.configure("2") do |config|

  MACHINES.each do |boxname, boxconfig|

      config.vm.define boxname do |box|

          box.vm.box = boxconfig[:box_name]
          box.vm.host_name = boxname.to_s

          #box.vm.network "forwarded_port", guest: 3260, host: 3260+offset
		  box.vm.network "private_network", ip: boxconfig[:ip_addr]
		  
		  box.vm.network "public_network", type: "dhcp", adapter: 2
		  box.vm.synced_folder "vmshare/", "/vagrant"
          box.vm.provider :virtualbox do |vb|
            	  vb.customize ["modifyvm", :id, "--memory", "1024"]
                  needsController = false
		  boxconfig[:disks].each do |dname, dconf|
			  unless File.exist?(dconf[:dfile])
				vb.customize ['createhd', '--filename', dconf[:dfile], '--variant', 'Fixed', '--size', dconf[:size]]
                                needsController =  true
                          end

		  end
                  if needsController == true
                     vb.customize ["storagectl", :id, "--name", "SATA", "--add", "sata" ]
                     boxconfig[:disks].each do |dname, dconf|
                         vb.customize ['storageattach', :id,  '--storagectl', 'SATA', '--port', dconf[:port], '--device', 0, '--type', 'hdd', '--medium', dconf[:dfile]]
                     end
                  end
          end
 	  box.vm.provision "shell", inline: <<-SHELL
	      mkdir -p ~root/.ssh
          cp ~vagrant/.ssh/auth* ~root/.ssh
                repo_file=/etc/yum.repos.d/CentOS-Base.repo
                cp ${repo_file} ~/CentOS-Base.repo.backup
                sudo sed -i s/#baseurl/baseurl/ ${repo_file}
                sudo sed -i s/mirrorlist.centos.org/vault.centos.org/ ${repo_file}
                sudo sed -i s/mirror.centos.org/vault.centos.org/ ${repo_file}
                sudo yum clean all
                yum install -y mdadm smartmontools hdparm gdisk
  	  SHELL

      end
  end
end


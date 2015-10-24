Vagrant.require_version ">= 1.7.4"

HOSTS = [
    { :name => "lb"  , :ip => "10.0.0.10",
      :mem => "256"  , :box => "bento/centos-7.1",
      :ports => [ { :guest => 1936, :host => 1936 } ]
    },
    { :name => "app1", :ip => "10.0.0.20",
      :mem => "256"  , :box => "ubuntu/trusty64"
    },
    { :name => "app2", :ip => "10.0.0.21",
      :mem => "256"  , :box => "ubuntu/trusty64"
    },
    { :name => "app3", :ip => "10.0.0.22",
      :mem => "256"  , :box => "ubuntu/trusty64"
    },
    { :name => "db"  , :ip => "10.0.0.30",
      :mem => "256"  , :box => "bento/centos-7.1"
    }
]


Vagrant.configure(2) do |config|

    HOSTS.each do |opts|

        config.vm.define opts[:name] do |node|

            node.vm.hostname = opts[:name]
            node.vm.box      = opts[:box]

            node.vm.network "private_network", ip: opts[:ip]
            if (opts.has_key?(:ports))
                opts[:ports].each do |port|
                    node.vm.network "forwarded_port",
                        guest: port[:guest], host: port[:host]
                end
            end
#            node.vm.provision "hosts" do |provisioner|
#                HOSTS.each do |opts|
#                    provisioner.add_host opts[:ip],
#                       [ opts[:name], (opts[:name] + '.com') ]
#                end
#            end


            node.vm.provision "ansible" do |ansible|
                ansible.inventory_path = "hosts-vagrant"
                ansible.playbook = "playbook.yml"
                #ansible.sudo    = true
                #ansible.verbose = "vvv"
                #ansible.limit   = 'all'

                # for other details...
                # @see http://docs.vagrantup.com/v2/provisioning/ansible.html
            end


            node.vm.provider "virtualbox" do |vb|
                vb.name = opts[:name]
                vb.customize ["modifyvm", :id, "--memory", opts[:mem]]
            end

        end

    end

end

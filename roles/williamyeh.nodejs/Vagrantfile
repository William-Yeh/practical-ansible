Vagrant.configure(2) do |config|

    # main & default: normal OS series...
    config.vm.define "main", primary: true do |node|
        node.vm.box = "ubuntu/trusty64"
        #node.vm.box = "ubuntu/precise64"
        #node.vm.box = "debian/jessie64"
        #node.vm.box = "debian/wheezy64"
        #node.vm.box = "chef/centos-7.0"
        #node.vm.box = "chef/centos-6.6"

        node.vm.provision "ansible" do |ansible|
            ansible.playbook = "test.yml"
        end
    end


    # docker: for auto build & testing (e.g., Travis CI)
    config.vm.define "docker" do |node|
        node.vm.box = "williamyeh/ubuntu-trusty64-docker"

        node.vm.provision "shell", inline: <<-SHELL
            cd /vagrant
            docker build  -f test/Dockerfile-ubuntu14.04  -t nodejs_trusty   .
            docker build  -f test/Dockerfile-ubuntu12.04  -t nodejs_precise  .
            docker build  -f test/Dockerfile-debian8      -t nodejs_jessie   .
            docker build  -f test/Dockerfile-debian7      -t nodejs_wheezy   .
            docker build  -f test/Dockerfile-centos7      -t nodejs_centos7  .
            docker build  -f test/Dockerfile-centos6      -t nodejs_centos6  .
        SHELL
    end

end

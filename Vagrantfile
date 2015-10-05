VAGRANTFILE_API_VERSION = "2"

Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
  if ENV["BF_ACCEPT"] == 'true'
    config.vm.box = "bento/centos-7.1"


    config.vm.provider "virtualbox" do |v, override|
      v.customize ["modifyvm", :id, "--cpus", 2]
      # base box hasn't enough memory for DB2
      v.customize ["modifyvm", :id, "--memory", 4096]
    end

    # put box on the Virtualbox private network when OHANA is set
    OHANA=ENV["OHANA"] || false
    if OHANA
      config.vm.network :private_network, type: "dhcp"
    end

    # turn off the vbguest auto update, it slows provisioning
    config.vbguest.auto_update = false

    config.vm.provision "shell" do |s|
      s.path = "./scripts/vagrant-provision-svr.sh"
      ARGS = ENV["DOCKER_EMAIL"] + ' ' + ENV["DOCKER_USERNAME"] \
        + ' ' + ENV["DOCKER_PASSWORD"] + ' ' + ENV["BES_VERSION"]
      s.args = ARGS
    end
  end
end
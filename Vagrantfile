Vagrant.configure("2") do |config|
  config.vm.define "practicaCluster" do |p|
      p.vm.box = "debian/bookworm64"
      p.vm.hostname = "practicaCluster"
      p.vm.network "forwarded_port", guest: 8080, host: 8080
      p.vm.network "private_network", ip: "192.168.37.37"
      p.vm.provision "shell", inline: <<-SHELL
        apt update
      SHELL
    end
  end
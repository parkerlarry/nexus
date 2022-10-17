$docker_install = <<-EOF

curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
sudo usermod -aG docker vagrant
rm -f get-docker.sh
EOF

$nexus_install = <<-EOF
chown $(id -u vagrant):$(id -g vagrant) /home/vagrant/nexus
docker container run --detach \
--restart always \
--publish 8081:8081 \
--publish 8082:8082 \
--name nexus \
--mount type=bind,src=/home/vagrant/nexus/nexus.vmoptions,dst=/opt/sonatype/nexus/bin/nexus.vmoptions \
sonatype/nexus3:3.42.0
EOF

Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/focal64"
  config.vm.provider "virtualbox" do |v|
    v.memory = 4096
    v.cpus = 1
  end

  config.vm.provision "file", source: "~/nexus", destination: "/home/vagrant/nexus"

  config.vm.network "private_network", ip: "192.168.60.7"
  config.vm.network "forwarded_port", guest: 8081, host: 36081 # rememeber open host port on firewall

  config.vm.provision "shell", inline: $docker_install
  config.vm.provision "shell", inline: $nexus_install
end

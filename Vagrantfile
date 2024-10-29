# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile para una VM Debian 
Vagrant.configure("2") do |config|
  # Define la imagen de Debian
  config.vm.box = "debian/bullseye64" 

  # Configuración de red
  config.vm.network "private_network", ip: "192.168.56.10"

  # Provisión para instalar paquetes
  config.vm.provision "shell", inline: <<-SHELL
    apt-get update
    apt-get install -y bind9 dnsutils apache2
  SHELL
end

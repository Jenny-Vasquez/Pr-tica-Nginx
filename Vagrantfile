# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|
  # Define la imagen de Debian
  config.vm.box = "debian/bullseye64"

  # Configuración de red
  config.vm.network "private_network", ip: "192.168.56.10"

  # Provisión para instalar paquetes y configurar Nginx
  config.vm.provision "shell", inline: <<-SHELL
    # Actualizar los repositorios
    apt-get update -y
    
    # Instalar Nginx
    apt-get install -y nginx

     
    # Habilitar el sitio de Nginx
    sudo ln -s /etc/nginx/sites-available/jenny /etc/nginx/sites-enabled/
    
    # Verificar la configuración de Nginx
    sudo nginx -t

    # Reiniciar Nginx para aplicar cambios
    sudo systemctl restart nginx
  SHELL
end


Vagrant.configure("2") do |config|
  # Definir la imagen de Debian
  config.vm.box = "debian/bullseye64"

  # Configuración de red
  config.vm.network "private_network", ip: "192.168.56.10"

  # Aprovisionamiento con shell script
  config.vm.provision "shell", inline: <<-SHELL
    # Actualizar el sistema e instalar Nginx y vsftpd para FTPS
    sudo apt-get update
    sudo apt-get install -y nginx vsftpd openssl

    # Crear directorio para el sitio web
    sudo mkdir -p /var/www/jenny/html

    # Copiar archivos de configuración desde /vagrant
    # Configuración de nginx
    sudo cp /vagrant/provision/nginx.conf /etc/nginx/nginx.conf
    sudo cp /vagrant/provision/jenny.conf /etc/nginx/sites-available/jenny
    sudo cp /vagrant/provision/index.html /var/www/jenny/html/index.html
    
    # Configuración de /etc/hosts
    sudo cp /vagrant/provision/host.conf /etc/hosts
    
    # Configuración de vsftpd para FTPS
    sudo cp /vagrant/provision/vsftpd.conf /etc/vsftpd.conf

    # Crear enlace simbólico en sites-enabled
    sudo ln -s /etc/nginx/sites-available/jenny /etc/nginx/sites-enabled/

    # Configurar permisos para el sitio web
    sudo chown -R www-data:www-data /var/www/jenny
    sudo chmod -R 755 /var/www/jenny

    # Generar certificados SSL para FTPS
    sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/vsftpd.key -out /etc/ssl/certs/vsftpd.crt

    # Verificar configuración de Nginx y reiniciar servicios
    sudo nginx -t && sudo systemctl restart nginx
    sudo systemctl restart vsftpd
  SHELL
end


Vagrant.configure("2") do |config|
  # Definir la imagen de Debian
  config.vm.box = "debian/bullseye64"

  # Configuración de red
  config.vm.network "private_network", ip: "192.168.56.10"

  # Aprovisionamiento con shell script
  config.vm.provision "shell", inline: <<-SHELL
    #INSTALAR NGINX VSFTPD
  # Actualizar el sistema e instalar Nginx y vsftpd para FTPS
  sudo apt-get update
  sudo apt-get install -y nginx vsftpd openssl

  # Crear directorio para el sitio web
  sudo mkdir -p /var/www/jenny/html

  # Copiar archivos de configuración desde /vagrant
  #Configuración de nginx
  sudo cp /vagrant/provision/nginx.conf /etc/nginx/nginx.conf
  sudo cp /vagrant/provision/jenny.conf /etc/nginx/sites-available/jenny
  sudo cp /vagrant/provision/index.html /var/www/jenny/html/index.html
  #Configuración de /etc/hosts
  sudo cp /vagrant/provision/host.conf /etc/hosts
  # Crear enlace simbólico en sites-enabled
  sudo ln -s /etc/nginx/sites-available/jenny /etc/nginx/sites-enabled/
  # Establecemos www-data en el mismo grupo de jenny, para que cuando metamos archivos
  sudo usermod -aG jenny www-data
   # Ajustamos permisos en la carpeta de inicio del usuario
  sudo chown -R www-data:www-data /var/www/jenny
  sudo chmod -R 755 /var/www/jenny

#INSTALACION FTP

  # Actualizamos los repositorios
  sudo apt-get update
  # Creamos un usuario
  sudo adduser jenny
  echo "jenny:1234" | sudo chpasswd
  # Creamos directorio de FTP del usuario
  sudo mkdir -p /home/jenny/ftp

  # Ajustamos permisos en la carpeta de inicio del usuario
  sudo chown -R jenny:jenny /home/jenny
  sudo chmod -R 775 /home/jenny
  
  # Generar certificados SSL para FTPS
  sudo openssl req -x509 -nodes -days 365 -newkey rsa:2048 -keyout /etc/ssl/private/vsftpd.key -out /etc/ssl/certs/vsftpd.crt
  # Configuración de vsftpd para FTPS
  sudo cp /vagrant/provision/vsftpd.conf /etc/vsftpd.conf

  # Reiniciar servicios
  sudo systemctl restart vsftpd
  sudo nginx -t
  sudo systemctl restart nginx
  SHELL
end


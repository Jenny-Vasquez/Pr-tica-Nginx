# Prática-Nginx
## Configuración de un Nuevo Dominio en Nginx con FTPS

Este documento describe cómo configurar un nuevo dominio para un sitio web en Nginx, transferir los archivos mediante FTPS y establecer los permisos adecuados en un servidor Debian.

### Requisitos Previos
- **Nginx** y **vsftpd** instalados en el servidor Debian.
- Acceso a un cliente FTPS para la transferencia de archivos.
- Certificados SSL configurados para FTPS.

---
## TAREA 5
### Paso 1: Configuración de Nginx para el Nuevo Sitio Web

1. **Crear la Carpeta del Sitio Web**
   - Crea la estructura de directorios en `/var/www` para el nuevo sitio.
     ```bash
     sudo mkdir -p /var/www/jenny/html
     ```

2. **Transferir Archivos al Servidor mediante FTPS**
   - Abre un cliente FTP compatible con FTPS (como **Filezilla**).
   - Conéctate al servidor usando la IP, el puerto 21, y selecciona **FTPS explícito** para transferir de forma segura.
   - Navega a `/var/www/jenny/html` en el servidor y sube los archivos de tu sitio web.

3. **Establecer Permisos de la Carpeta**
   - Cambia la propiedad de la carpeta al usuario `www-data`, que usa Nginx:
     ```bash
     sudo chown -R www-data:www-data /var/www/jenny
     ```
   - Establece permisos de acceso adecuados:
     ```bash
     sudo chmod -R 755 /var/www/jenny
     ```

4. **Crear el Bloque de Servidor en Nginx**
   - Crea un archivo de configuración en **sites-available**:
     ```bash
     sudo nano /etc/nginx/sites-available/jenny
     ```
   - Agrega la configuración del servidor:
     ```nginx
     server {
       listen 80;
       listen [::]:80;
       root /var/www/jenny/html;
       index index.html index.htm;
       server_name jenny;

       location / {
         try_files $uri $uri/ =404;
       }
     }
     ```

5. **Habilitar el Nuevo Sitio en Nginx**
   - Crea un enlace simbólico en **sites-enabled**:
     ```bash
     sudo ln -s /etc/nginx/sites-available/jenny /etc/nginx/sites-enabled/
     ```
   - Reinicia Nginx para aplicar la configuración:
     ```bash
     sudo systemctl restart nginx
     ```

6. **Comprobar el Funcionamiento**
   - Añade la IP y el dominio al archivo **hosts** de tu máquina local para resolver manualmente:
     ```plaintext
     192.168.56.10 jenny
     ```
   - Accede a `http://jenny` desde un navegador para verificar el funcionamiento.
---
## CUESTIONES FINALES

- **¿Qué pasa si no hago el enlace simbólico entre `sites-available` y `sites-enabled` de mi sitio web?**
  - Si no creas el enlace simbólico desde `sites-available` a `sites-enabled`, Apache no reconocerá ni cargará la configuración del nuevo sitio. Esto significa que, aunque hayas configurado el archivo en `sites-available`, el sitio no estará accesible desde el dominio configurado.

- **¿Qué pasa si no le doy los permisos adecuados a `/var/www/nombre_web`?**
  - Si los permisos no son correctos, Apache puede tener problemas para acceder y servir los archivos del sitio, resultando en errores de acceso como el 403 "Forbidden", que indica que el servidor no tiene permiso para acceder a esos archivos.


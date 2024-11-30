# Imagen base
FROM ubuntu:latest

# Instalar Apache y herramientas necesarias
RUN apt update && apt install -y apache2 openssl apache2-utils && \
    a2enmod ssl auth_basic

# Configurar Apache
COPY virtualhost.conf /etc/apache2/sites-available/
COPY .htpasswd /etc/apache2/
COPY hosts /etc/

# Crear directorios y copiar ficheros
RUN mkdir -p /var/www/html/iarista/privado
COPY html/ /var/www/html/

# Crear certificados SSL
RUN openssl req -x509 -nodes -days 365 -newkey rsa:2048 \
    -keyout /etc/ssl/private/apache-selfsigned.key \
    -out /etc/ssl/certs/apache-selfsigned.crt \
    -subj "/CN=www.iarista.eus"

# Habilitar sitios y reiniciar Apache
RUN a2ensite virtualhost.conf && a2enmod ssl && \
    service apache2 restart

# Exponer puertos HTTP y HTTPS
EXPOSE 80 443

# Comando por defecto
CMD ["apachectl", "-D", "FOREGROUND"]

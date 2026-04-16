# Laboratorio-8-SO3
Documentación de prácticas de  Instalacion y configuracion de Docker, Instalacion de Portainer , IDespliegue de contenedor de Wordpress utilizando Docker-Compose en Oracle Linux.
Prácticas de Docker y Orquestación
 Práctica 1: Instalación y Configuración de Docker
Objetivo: Desplegar un servidor NGINX con persistencia de datos.

Instalar Docker:

Aplicar:
sudo dnf install -y dnf-utils
sudo dnf config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
sudo dnf install -y docker-ce docker-ce-cli containerd.io

sudo systemctl start docker
sudo systemctl enable docker

docker --version
2. DESCARGAR NGINX
docker pull nginx

docker pull nginx
3. CREAR VOLUMEN PERSISTENTE
mkdir -p /home/website

mkdir -p /home/website
4. CREAR PÁGINA HTML

nano /home/website/index.html
Escribe:
<html>
<head><title>Mi Página</title></head>
<body>
<h1>Nombre: TU NOMBRE</h1>
<h2>Matricula: TU MATRICULA</h2>
</body>
</html>

5. CREAR CONTENEDOR NGINX

docker run -d \
-p 8888:80 \
-v /home/website:/usr/share/nginx/html \
--name nginx-container \
nginx
6. CONFIGURAR o Confirmar Firewall

sudo firewall-cmd --add-port=8888/tcp --permanent
sudo firewall-cmd --reload
Abre:
http://127.0.0.1:8888












 Práctica 2: Instalación de Portainer
Objetivo: Gestión gráfica de contenedores.


PRÁCTICA 2: PORTAINER
 1. Descargar imagen de Portainer
sudo docker pull portainer/portainer-ce
2. Crear volumen para Portainer
sudo docker volume create portainer_data
3. Crear contenedor Portainer
sudo docker run -d \
-p 8000:8000 \
-p 9443:9443 \
--name portainer \
--restart=always \
-v /var/run/docker.sock:/var/run/docker.sock \
-v portainer_data:/data \
portainer/portainer-ce
4. Acceder a Portainer

Abre:

https://127.0.0.1:9443

Crear usuario administrador.

Selecciona:
Local Docker Environment

5. Detener NGINX desde Portainer

Dentro de Portainer:

Ve a Containers
Busca mi_nginx
Click en Stop
6. Verificar en navegador

vuelve a:

http://127.0.0.1:8888

Ya NO debe cargar → prueba de que el contenedor se detuvo.








 Práctica 3: WordPress con Docker-Compose
Objetivo: Despliegue multicontenedor automatizado.

sudo dnf install -y yum-utils
sudo dnf config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
sudo dnf install docker-ce docker-ce-cli containerd.io -y
sudo systemctl start docker
sudo systemctl enable docker
docker --version


 3. INSTALAR DOCKER COMPOSE
sudo dnf install docker-compose-plugin -y
docker compose version


 4. CREAR PROYECTO
mkdir ~/wordpress-compose
cd ~/wordpress-compose

 5. CREAR docker-compose.yml
nano docker-compose.yml
📄 PEGAR ESTO:
services:
  db:
    image: mysql:5.7
    container_name: wordpress_db
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: Oraclelinux
      MYSQL_DATABASE: wordpress
      MYSQL_USER: wp_user
      MYSQL_PASSWORD: wp_pass
    volumes:
      - db_data:/var/lib/mysql

  wordpress:
    image: wordpress:latest
    container_name: wordpress_app
    depends_on:
      - db
    ports:
      - "8081:80"
    restart: always
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_USER: wp_user
      WORDPRESS_DB_PASSWORD: wp_pass
      WORDPRESS_DB_NAME: wordpress
    volumes:
      - wp_data:/var/www/html

volumes:
  db_data:
  wp_data:


6. DESPLEGAR CONTENEDORES
sudo docker compose up -d



7. VERIFICAR
sudo docker ps


8. CONFIGURAR FIREWALL
sudo firewall-cmd --add-port=8081/tcp --permanent
sudo firewall-cmd --reload

9. ACCEDER A WORDPRESS

 En navegador:

http://IP_DEL_SERVIDOR:8081


10. CONFIGURAR WORDPRESS


 11. LOGIN


http://IP_DEL_SERVIDOR:8081/wp-admin

 DICES:

“Ingreso al panel de administración de WordPress.”

 12. CREAR CONTENIDO


Contenido:

Nombre: Angel Rodriguez
Matrícula: 20252201
Práctica Docker WordPress
🔹 13. VER SITIO

http://IP_DEL_SERVIDOR:8081

Este es el enlace a la lista de reproduccion en youtube:https://www.youtube.com/playlist?list=PLpcZGYtY2vZZs8LUfLYM161r9WZCaaWwf

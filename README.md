# Laboratorio-8-SO3
Documentación de prácticas de  Instalacion y configuracion de Docker, Instalacion de Portainer , IDespliegue de contenedor de Wordpress utilizando Docker-Compose en Oracle Linux.
Prácticas de Docker y Orquestación
 Práctica 1: Instalación y Configuración de Docker
Objetivo: Desplegar un servidor NGINX con persistencia de datos.

Instalar Docker:

Bash
sudo dnf install -y docker-ce docker-ce-cli containerd.io
sudo systemctl enable --now docker
Descargar imagen y crear volumen:

Bash
docker pull nginx
mkdir -p /home/website
Crear el contenedor con redirección de puerto y volumen:

Bash
docker run -d --name mi-nginx -p 8888:80 -v /home/website:/usr/share/nginx/html nginx
Crear la página personalizada:

Bash
echo "<h1>Angel - Matrícula: [TU_MATRICULA]</h1>" > /home/website/index.html
Prueba: Abre el navegador en http://127.0.0.1:8888

 Práctica 2: Instalación de Portainer
Objetivo: Gestión gráfica de contenedores.

Crear el volumen para los datos de Portainer:

Bash
docker volume create portainer_data
Desplegar el contenedor (Mapeo total de puertos):

Bash
docker run -d -p 8000:8000 -p 9443:9443 --name portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce:latest
Configuración:

Accede a https://127.0.0.1:9443.

Crea tu usuario admin.

Vincular: Selecciona el entorno "Local" para ver el Docker Engine del host.

Acción: Busca el contenedor mi-nginx en la lista y dale a Stop. Refresca el navegador en el puerto 8888 para verificar que ya no carga.

 Práctica 3: WordPress con Docker-Compose
Objetivo: Despliegue multicontenedor automatizado.

Instalar Docker-Compose:

Bash
sudo dnf install -y docker-compose-plugin
Crear archivo docker-compose.yml:

YAML
services:
  db:
    image: mariadb:10.6
    volumes:
      - db_data:/var/lib/mysql
    environment:
      MYSQL_ROOT_PASSWORD: root_password
      MYSQL_DATABASE: wordpress
      MYSQL_USER: user_wp
      MYSQL_PASSWORD: password_wp

  wordpress:
    depends_on:
      - db
    image: wordpress:latest
    ports:
      - "8080:80"
    environment:
      WORDPRESS_DB_HOST: db
      WORDPRESS_DB_USER: user_wp
      WORDPRESS_DB_PASSWORD: password_wp
      WORDPRESS_DB_NAME: wordpress

volumes:
  db_data:
Despliegue:

Bash
docker compose up -d
Configuración Final:

Ingresa a http://127.0.0.1:8080.

Sigue el asistente de instalación de WordPress (idioma, título del sitio y usuario).

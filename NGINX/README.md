# Docker para montar entornos en local

Configuración para desplegar un proyecto php.
Basado en docker nginx + php-fpm

## Uso

### Instalación

Escoger la configuración dependiendo del proyecto que vayamos a desplegar
y copiarla en la carpeta `./docker/nginx/conf/`
- default.conf
- default-wordpress.conf
- default-prestashop.conf

Generar certidicados autofirmados ejecutando el script ./generate-selfsigned-certificate.sh

Clonar el proyecto en el subdirectorio `./public_html/`
mount --make-rshared /

```bash
git clone git@git.dominio.com:abc123/abc123.git ./public_html/

# evitamos trackear cambios de permisos en archivos en git
git config core.filemode false
```

Ajustamos los permisos del proyecto

```bash
sudo find ./public_html/ -type f -exec chmod 664 {} \;
sudo find ./public_html/ -type d -exec chmod 775 {} \;

sudo chown -R :www-data ./public_html/

```

Configuramos el archivo `.env` a nuestro gusto

### Prestashop

Modificamos la configuración de nginx docker/nginx/conf/default.conf añadiendo la carpeta admin correcta de prestashop

```
# [REQUIRED EDIT] Change this block to your admin folder
    location /adminABCD1234/ {
        if (!-e $request_filename) {
            rewrite ^/.*$ /adminABCD1234/index.php last;
        }
    }
```

### Wordpress

### Laravel

docker exec [container] composer create-project laravel/laravel example-app
docker exec [container] php example-app/artisan serve --port=8080 --host=127.0.0.1

### Ejecutar comandos

docker exec [container] [comand]

### Ejecución

Finalmente ejecutamos

```
docker-compose up -d
```
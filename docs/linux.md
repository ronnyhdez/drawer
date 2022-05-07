# Installing RStudio in Ubuntu

Instalacion de R en ubuntu 20.10 desde binario de RStudio

## Instalar gdebi

```
sudo apt update
sudo apt install gdebi-core
```

##Especificar version de R

```
export R_VERSION=4.1.1
```

## Descargar binario de R 

```
curl -O https://cdn.rstudio.com/r/ubuntu-2004/pkgs/r-${R_VERSION}_1_amd64.deb
```
## Instalar binario de R

```
sudo gdebi r-${R_VERSION}_1_amd64.deb
```
## Validar version de R

```
/opt/R/${R_VERSION}/bin/R --version
```

## Crear los symlink

```
sudo ln -s /opt/R/${R_VERSION}/bin/R /usr/local/bin/R
sudo ln -s /opt/R/${R_VERSION}/bin/Rscript /usr/local/bin/Rscript
```

## Instalaci'on de RStudio en ubuntu 20.10

Descarga RStudio desktop desde el sitio https://www.rstudio.com/products/rstudio/download/#download

Validar en que carpeta de la computadora quedo el archivo comprimido

## instalar con gdebi rstudio

```
sudo gdebi Downloads/rstudio-1.4.1717-amd64.deb 
```

## Actualizar paquetes.

Al tener la nueva instalacion, los paquetes que tenia instalados, 
ya no los tengo. En lugar de ir instalando cada uno por separado,
lo que hago es que el directorio donde tenia las instalaciones de los
paquetes, la copio en la nueva direccion.

```
/home/ronny/R/x86_64-pc-linux-gnu-library/4.1
```

Luego, abro RStudio, en el panel de paquetes selecciono la opcion `update`, 
luego `Select all` y por ultimo `Install Updates`.  Asi se instalaran todos 
los paquetes que tenia anteriormente.

# Apache configuration steps for shiny server

 -  Para saber si un archivo es un symlink: file archivo

## Instalar dependencias:

```
sudo apt-get install apache2
sudo apt-get install libapache2-mod-proxy-html
sudo apt-get install libxml2-dev
```

## Habilitar modulos

```
sudo a2enmod proxy
sudo a2enmod proxy_http
sudo a2enmod proxy_wstunnel
sudo a2enmod rewrite
```

## Configurar archivo /etc/apache2/sites-available/000-default.conf

Dejar lo siguiente: (se puede borrar todo lo demas)

```
<VirtualHost *:80>

  <Proxy *>
    Allow from localhost
  </Proxy>

 RewriteEngine on
 RewriteCond %{HTTP:Upgrade} =websocket
 RewriteRule /(.*) ws://localhost:3838/$1 [P,L]
 RewriteCond %{HTTP:Upgrade} !=websocket
 RewriteRule /(.*) http://localhost:3838/$1 [P,L]
 ProxyPass / http://localhost:3838/
 ProxyPassReverse / http://localhost:3838/
 ProxyRequests Off

</VirtualHost>
```

# Reiniciar apache

```
sudo systemctl restart apache2
```

# Ir a revisar la direccion ip a ver si ya no tenemos que usar la puerta

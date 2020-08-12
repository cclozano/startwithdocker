# Una breve introduccion con docker
## Cuando desarrollamos generalmente nos vemos obligados a usar multiples herramientas tales como motores de bases de datos, adminitradores de contenido, colas de mensajes, en fin.. un sinnumero de programas que no necesariamente deben ser instalados en nuestra maquina host. Para ello les dejo algunos scripts que pueden ser de utilidad para que puedan instalar docker en linux y sobre el desplegar algunos contenedores

## Para este tutorial usaremos ubuntu como sistema operativo si necesita instalar en otra distribucion puedes revisar la documentacion oficial en el siguiente enlace:
https://docs.docker.com/engine/install/

### Primero abrimos una terminal y actualizamos el indice de paquetes
```
$ sudo apt-get update
```
### instale paquetes para permitir que apt use un repositorio sobre HTTPS:
```
$ sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common
```
### Agrega la clave GPG oficial de Docker
```
 $ curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add - 
 ```
### Utilice el siguiente comando para configurar el repositorio estable.
```
 $ sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
```
### Actualizamos nuevamente el indice de paquetes
```
 $ sudo apt-get update
```
### Hacemos la instalacion del motor de docker
```
 $ sudo apt-get install docker-ce docker-ce-cli containerd.io
```
### Agregamos el grupo de usuarios 
```
 $ sudo groupadd docker
```
### Agregamos nuestro usuario al grupo creado
```
 $ sudo usermod -aG docker $USER
 ```
### en algunos casos es necesario reiniciar el sistema para que los cambios de permisos tengan efecto
 ```
 $ sudo reboot
 ```
## Hasta aqui hemos instalado el motor de docker y ya podemos comenzar a desplegar nuestros contenedores, para ello vamos a comenzar desplegado el motor de mongodb
### creamos la red para nuestros contenedores
```
$ docker network create mongo
```
### con el siguiente script desplegamos nuestro contenedor de mongo
### como podemos ver, el usuario y password los estamos seteando con "admin"
```
$ docker run -d --network mongo --name mongo \
    -p 27017:27017 \
    -e MONGO_INITDB_ROOT_USERNAME=admin \
    -e MONGO_INITDB_ROOT_PASSWORD=admin \
    mongo
```

### en el siguiente codigo desplegamos el contenedor de mongo-express que nos permitira administrar nuestro motor
```
$ docker run --network mongo \
  -e ME_CONFIG_MONGODB_SERVER=mongo \
  -p 8081:8081 \
  -e ME_CONFIG_MONGODB_ADMINUSERNAME=admin \
  -e ME_CONFIG_MONGODB_ADMINPASSWORD=admin \
  mongo-express
```
### Si todo se instalo correctamente lo verificamos ingresando desde nuestro navegador en http://localhost:8081


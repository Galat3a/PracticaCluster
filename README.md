# 1 Primero sin Cluster
Creamos una maquina virtual a traves de vagrant para realizar la practica.

```bash
Vagrant.configure("2") do |config|
  config.vm.define "practicaCluster" do |p|
      p.vm.box = "debian/bookworm64"
      p.vm.hostname = "practicaCluster"
      p.vm.network "forwarded_port", guest: 8080, host: 8080
      p.vm.network "private_network", ip: "192.168.37.37"
      p.vm.provision "shell", inline: <<-SHELL
        apt update
      SHELL
    end
  end
```

Una vez iniciada la practica, creamos un directorio llamado practicaNoCluster:

Instalamos nodejs:

```bash 
sudo apt update
sudo apt install nodejs npm -
//verificamos la instalación
node -v
npm -v
```

<img src="./capturas/1.png>

Dentro del directorio ejecutaréis 2 comandos:

```bash 
npm init
npm install express
```
<img src=./capturas/2.png>
<img src=./capturas/3.png>
<img src=./capturas/4.png>

Una vez instalado, cremos nuetra aplicacion js, llamada en mi caso practicaNoCLuster.js, tal y como nos indican en la practica: 

<img src=./capturas/5.png>

Ejecutamos la aplicacion con node:

```bash 
node practicNoCluster.js
```

<img src=./capturas/6.png>

Y verificamos el tiempo de respuesta del navegador según el numero utilizado para la api:

<img src=./capturas/7.png>

<img src=./capturas/8.png>

<img src=./capturas/9.png>

<img src=./capturas/10.png>

<img src=./capturas/11.png>

# 2 ¡Ahora con más clúster!

Creamos de nuevo un js, con cluster:

```bash 
sudo nano practicSiCluster.js
```
Ejecutamos el  js

```bash 
node practicSiCluster.js
```
<img src=./capturas/12.png>

Ahora repetiremos el mismo experimento de antes, primero realizamos una solicitud al servidor
con un valor alto n y en otra pestaña con n bajo:

<img src=./capturas/13.png>

<img src=./capturas/14.png>


# 3 Métricas de rendimiento

Instalamos loadtest:

<img src=./capturas/12.png>

Ejecutamos de nuevo la aplicación con node.js SIN CLUSTER:

```bash 
node practicNoCluster.js
```

Mientras ejecutamos la aplicación, en otro terminal realizamos la siguiente prueba de carga:

<img src=./capturas/20.png>
<img src=./capturas/21.png>
<img src=./capturas/22.png>

Ejecutamos de nuevo la aplicación con node.js CON CLUSTER:

```bash 
node practicSiCluster.js
```
Mientras ejecutamos la aplicación, en otro terminal realizamos la siguiente prueba de carga:

<img src=./capturas/23.png>
<img src=./capturas/24.png>

Podemos comprobar como con el uso de cluster las peticiones son mas rapidas:
SIN CLUSTER
<img src=./capturas/22.png  style="width: 50vw; height: auto;">
CON CLUSTER
<img src=./capturas/24.png  style="width: 50vw; height: auto;">

# 4 Uso de PM2 para administrar un clúster de Node.js

Instalamos PM2 en nuestro Debian:

```bash 
npm install pm2 -g
```

<img src=./capturas/25.png>

Ejecutamos este comando para la aplicacion SIN CLUSTER

```bash 
pm2 start practicaNoCluster.js -i 0

```

<img src=./capturas/26.png>
<img src=./capturas/27.png>

Detengo la aplicación con el siguiente comando:

```bash 
pm2 stop practicaNoCluster.js
```

<img src=./capturas/28.png>

Creamos el archivo Ecosystem para guardar la tarea:

```bash 
pm2 ecosystem
```

<img src=./capturas/29.png>

Modificamos el archivo según nos indica la practica:

<img src=./capturas/30.png>

```bash 
pm2 start ecosystem.config.js
```
<img src=./capturas/31.png>

```bash 
pm2 restart practicaNoCluster.js
```

<img src=./capturas/32.png>

```bash 
pm2 reload practicaNoCluster.js
```
<img src=./capturas/33.png>

```bash
pm2 stop practicaNoCluster.js
```
<img src=./capturas/34.png>

Cuando usemos el archivo Ecosystem:

```bash

pm2 [start|restart|reload|stop|delete] ecosystem.config.js

```

# 5 TAREA

```bash
pm2 ls
```
<img src=./capturas/35.png>
<img src=./capturas/36.png>

Se utiliza para ver todos los procesos que gestiona PM2, comprobar el estado de ejecución (online, stopped, errored), monitorear el consumo de CPU y memoria y conocer el tiempo de actividad de cada proceso.

```bash
pm2 logs
```
<img src=./capturas/37.png>
<img src=./capturas/38.png>

Sirve para ver logs en tiempo real de todas las aplicaciones manejadas por pm2, detectar errores o eventos importantes en las aplicaciones y depuración rápida sin necesidad de acceder a archivos de logs manualmente


```bash
 pm2 monit
```

<img src=./capturas/39.png>

 Monitorear el rendimiento de las aplicaciones en tiempo real, identificar procesos con alto consumo de CPU o memoria y ver logs sin necesidad de usar pm2 logs.

 # 6 TAREA CAPTURAS

 ¿Sabrías decir por qué en algunos casos concretos, como este, la aplicación sin clusterizar tiene
mejores resultados?
Una aplicación sin clusterizar puede ofrecer mejores resultados debido a su menor latencia. Esto se debe a que un único proceso maneja todas las solicitudes, eliminando la necesidad de coordinar múltiples procesos y distribuyendo la carga de manera más directa. Al evitar la sobrecarga asociada con la comunicación interna entre procesos, se optimiza el tiempo de respuesta y se reducen los recursos consumidos en la gestión de concurrencia.

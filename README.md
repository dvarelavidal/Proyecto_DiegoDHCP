# Proyecto_DiegoDHCP

- En esta practica instalaré el servidor DHCP en un contenedor y comprobaremos su funcionalidad con un cliente con Windows 10.

- Vamos a utilizar el enlace que nos dejó el profe para descargar la imagen ya configurada.

```
https://hub.docker.com/r/networkboot/dhcpd/

```

Configuraremos nuestro DHCP, como habitualmente hemos estado haciendo en otras prácticas, creando un docker-compose.yml y añadiendo el siguiente código.


```

version: '3.4'
services:
  dhcpd:
    container_name: DHCP
    network_mode: "host"
    image: networkboot/dhcpd
    volumes:
      - ./data:/data


```

A continuación, crearemos la carpeta llamada "data" y introduciremos en el terminal el comando "docker-compose up" para levantar el servicio.
Al ejecutarlo, creará el archivo dhcp.conf, donde escribiremos el siguiente código.



subnet 10.0.9.0 netmask 255.255.255.0 {    
        
    range 10.0.9.190 10.0.9.200;     
    option domain-name-servers 8.8.8.8, 193.146.96.2, 193.146.96.3;
    option domain-name "Server DHCP";   
    option routers 10.0.9.24;    
    option domain-name-servers 10.0.2.10, 10.0.2.11;   
    option subnet-mask 255.255.255.0;     
    option broadcast-address 10.0.9.255;       
    default-lease-time 86400;
    max-lease-time 172800;
        
        
          }




  - Comprobamos su funcionamiento:
  Al ejecutar el comando "docker-compose up" en el terminal, nos saldrá la siguiente ventana, que nos indicará que todo va bien.
  
  ![This is an image](https://github.com/dvarelavidal/Proyecto_DiegoDHCP/blob/main/imagenes/Captura%20de%20pantalla%20de%202022-12-07%2018-42-47.png)
  
  
  
  
  Para comprobar que funciona lo que voy a hacer es, desde una maquina virtual, voy a pedir una ip dhcp a mi servidor de Docker.

Primero comienzo poniendo mi maquina en modo bridge y elimino la ip del router de clase con el comando ipconfig/release

Ahora veo la mac del equipo con el comando getmac y la introduco en mi archivo dhcp.conf y le doy una ip fija para la maquina de la siguiente manera.
  
  
  
    group {

     default-lease-time 604800;
     max-lease-time 691200;

     host apache {
         hardware ethernet 08:00:27:65:02:F3;
         fixed-address 10.0.9.201;
     }

  }
  
  
  
  ![This is an image](https://github.com/dvarelavidal/Proyecto_DiegoDHCP/blob/main/imagenes/Captura%20de%20pantalla%20de%202022-12-07%2019-43-47.png)
 

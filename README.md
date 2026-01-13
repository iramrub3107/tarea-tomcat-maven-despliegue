# Tarea Tomcat y Maven: Despliegue de Aplicaciones Web | Hecho por Izan Ramos Rubio

# 1. INSTALACIÓN DE TOMCAT9:

Primero que todo, para poder hacer esta práctica, tenemos que instalarnos OpenJDK y Tomcat9 ejecutando ```sudo apt install -y openjdk-11-jdk``` y ```sudo apt install -y tomcat9```.

Luego creamos un grupo exclusivamente para los usuarios de tomcat9 ejecutando ```sudo groupadd tomcat9```, y después creamos un usuario llamado tomcat9 ejecutando ```sudo useradd -s /bin/false -g tomcat9 -d /etc/tomcat9 tomcat9```

Listamos los usuarios y verificamos que se ha creado correctamente nuestro nuevo usuario:
<img width="1200" height="679" alt="image" src="https://github.com/user-attachments/assets/46a33e02-a64a-4dd0-85c7-7900627cb78e" />

Iniciamos el servicio de tomcat ejecutando ```sudo service tomcat9 start``` y miramos su estado con ```sudo service tomcat9 status```
<img width="1203" height="677" alt="image" src="https://github.com/user-attachments/assets/61f8b488-23ac-43c2-8e46-788d2ce33fc7" />

Para poder hacer que se vea el localhost en el puerto 8080 de la máquina virtual en vez de la máquina anfitriona he tenido que hacer varios cambios en la máquina virtual que está en mi VirtualBox:
1. He entrado a la configuración de la máquina virtual pulsando en "Configuración" > "Red" > "Avanzadas" > Reenvío de puertos
2. Dentro de las reglas de reenvío de puertos, añadimos una nueva regla y ponemos en ella la siguiente información:

<img width="961" height="780" alt="image" src="https://github.com/user-attachments/assets/db258f68-515c-497c-99a2-0633a2a45831" />

Y cuando tengamos bien configurada nuestra regla, nos iremos a nuestro navegador y entraremos en el localhost:8080 y nos tendría que salir lo siguiente:

<img width="960" height="379" alt="image" src="https://github.com/user-attachments/assets/dc7a4b0f-d4a0-43a1-b44b-81446f657b0d" />

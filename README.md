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

Dado a que Tomcat no permite acceder por defecto a paneles de control que no sea estando dentro del localhost, vamos a cambiar eso para que se pueda acceder desde donde sea.
Para ello, vamos a irnos a /usr/share/tomcat9-root/default_root/META-INF y editaremos el archivo context.xml, poniendo el contenido que aparece en la captura de pantalla:

<img width="960" height="540" alt="image" src="https://github.com/user-attachments/assets/85afe1cf-03d3-4148-a145-1f37ff68d078" />

Después de aplicar esos cambios y guardar el archivo, reiniciaremos el servicio de tomcat9 ejecutando ```sudo service tomcat9 restart``` y comprobamos si sigue funcionando:

<img width="959" height="525" alt="image" src="https://github.com/user-attachments/assets/638d290b-a789-4fda-97ef-edefa0f3465d" />

Como se puede observar, ha salido todo bien, por lo que el servicio sigue activo.

# 2. CONFIGURACIÓN DE LA ADMINISTRACIÓN

A continuación vamos a configurar el usuario con el que podremos acceder al panel de control de tomcat9. Para ello, configuraremos el archivo /etc/tomcat9/tomcat-users.xml y dentro de este "crearemos" el nuevo usuario:

<img width="961" height="539" alt="image" src="https://github.com/user-attachments/assets/2e8e389a-1a43-49b4-b561-89afadedc107" />

Como se puede observar, también se han creado los roles necesarios para aplicárselos al nuevo usuario y, por ende, poder acceder con este usuario al panel de control.
Una vez tengamos editado y guardado el archivo, vamos a reiniciar el servicio de tomcat9 una vez más. Si todo ha salido bien, el servicio seguirá activo

<img width="1202" height="678" alt="image" src="https://github.com/user-attachments/assets/06d6aef3-f53f-4f7e-ac87-b887c21d992c" />

# 3. INSTALACIÓN DEL ADMINISTRADOR WEB

Para instalar el administrador web **esencial** para poder hacer nuestros despliegues, nos instalaremos el panel de control de tomcat9 ejecutando ```sudo apt install -y tomcat9-admin```

<img width="1204" height="476" alt="image" src="https://github.com/user-attachments/assets/308d9c2c-af52-42b0-8d6b-bb0e6c1ed7fd" />

Cuando tengamos instalado el panel de control de tomcat9, accederemos a nuestro localhost en el puerto 8080, y podemos acceder al panel de control de dos formas:
1. O hacemos clic en donde pone remarcado "manager webapp"
2. O en la URL ponemos localhost:8080/manager/html, añadiendo, claramente, /manager/html.

<img width="1202" height="456" alt="image" src="https://github.com/user-attachments/assets/f0b06f30-2f8e-4db7-a58a-4980c3948694" />

Una vez dentro, como se puede ver, nos pedirá iniciar sesión, así que iniciaremos sesión con el usuario que hemos creado previamente en el archivo /etc/tomcat9/tomcat-users.xml.

<img width="1203" height="677" alt="image" src="https://github.com/user-attachments/assets/e6046236-5a46-4789-8517-e398a8576a6d" />

Y cuando hayamos iniciado sesión, si todo sale bien, nos saldrá el panel de control de tomcat9. Si no, nos volverá a pedir que iniciemos sesión. En mi caso, ya me ha salido todo bien, por lo que estoy dentro del panel de control.

<img width="1203" height="676" alt="image" src="https://github.com/user-attachments/assets/c9c62488-31d8-4a94-b696-e661e756ca13" />

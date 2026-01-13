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
Para ello, vamos a irnos a /usr/share/tomcat9-root/default_root/META-INF y editaremos el archivo context.xml, poniendo lo siguiente:

```xml
<?xml version="1.0" encoding="UTF-8"?>
<Context antiResourceLocking="false" privileged="true" >
  <CookieProcessor className="org.apache.tomcat.util.http.Rfc6265CookieProcessor"
                   sameSiteCookies="strict" />
  <Valve className="org.apache.catalina.valves.RemoteAddrValve"
         allow="\d+\.\d+\.\d+\.\d+" />
  <Manager sessionAttributeValueClassNameFilter="java\.lang\.(?:Boolean|Integer|Long|Number|String)|org\.apache\.catalina\.filters\.CsrfPreventionFilter\$LruCache(?:\$1)?|java\.util\.(?:Linked)?HashMap"/>
</Context>
```

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

A continuación, haremos el mismo proceso de iniciar sesión, pero esta vez en el host-manager

<img width="1203" height="675" alt="image" src="https://github.com/user-attachments/assets/4b7de63e-9e9a-4305-8165-2eb7c77f1334" />

Y nos saldrá el panel de control del host-manager:

<img width="1204" height="658" alt="image" src="https://github.com/user-attachments/assets/bf8b7a74-16e8-44a3-b329-7c1135585812" />

# 4. DESPLIEGUE MANUAL MEDIANTE GUI

Ahora vamos a hacer un despliegue de una aplicación mediante GUI. Para ello, usaremos un archivo .war (concretamente tomcat1.war). Nos lo descargamos y nos vamos a la sección que nos indica desplegar un WAR manualmente:

<img width="1204" height="677" alt="image" src="https://github.com/user-attachments/assets/2a5e692b-8b76-41dd-ab90-c1fd10dee72f" />

Para abrir el archivo .war, haremos clic en donde pone "Seleccionar archivo", y seleccionaremos el archivo .war que nos acabamos de descargar

<img width="1204" height="665" alt="image" src="https://github.com/user-attachments/assets/1c16cfaa-97e4-4752-9365-704d6c03421d" />
<img width="961" height="761" alt="image" src="https://github.com/user-attachments/assets/70c7287d-e7a7-4962-9f9e-3d8eb46e2bc6" />

Y cuando tengamos subido el archivo, hacemos clic en el botón que pone "Desplegar"

<img width="962" height="543" alt="image" src="https://github.com/user-attachments/assets/19c3dde6-a805-426f-b52d-c94e88a7d4c8" />

Esperamos a que se despliegue el archivo, y después, nos aparecerá nuestro archivo ya desplegado como un directorio más de todas nuestras aplicaciones

<img width="964" height="544" alt="image" src="https://github.com/user-attachments/assets/b3e11cd7-455e-4f8d-96a3-08a29f0afa24" />

# 5. DESPLIEGUE CON MAVEN

Para comenzar, vamos a instalar maven con ```sudo apt -y install maven```

<img width="1200" height="676" alt="image" src="https://github.com/user-attachments/assets/2316b9e4-8d89-4f15-bdb7-7e7dc310edf8" />

Para asegurarnos de que maven se ha instalado correctamente, ejecutaremos ```mvn --v``` para que podamos ver la versión instalada

<img width="1201" height="674" alt="image" src="https://github.com/user-attachments/assets/56fe2b15-0c57-4d7c-942a-db5375c30e4c" />

A continuación, vamos a añadir un nuevo usuario en nuestro archivo tomcat-users.xml para que nuestro primer usuario no tenga los mismos permisos (los suyos además de manager-script) que el nuevo.

<img width="1202" height="676" alt="image" src="https://github.com/user-attachments/assets/eed1e45a-095d-4b7d-8871-373f87ffcc07" />

Luego, editaremos el archivo settings.xml que se encuentra en /etc/maven y nos iremos a la etiqueta servers para poner lo siguiente:

```xml
...
...
<servers>
  <server>
    <id>Tomcat</id>
    <username>deploy</username>
    <password>1234</password>
  </server>
 </servers>
```

<img width="1200" height="678" alt="image" src="https://github.com/user-attachments/assets/aac48f31-07c4-42d2-a702-42097b54ee50" />

Después, ejecutaremos el siguiente comando:

```
mvn archetype:generate -DgroupId=tu-enlace -DartifactId=tu-id -Ddeployment -DarchetypeArtifactId=maven-archetype-webapp -DinteractiveMode=fa
```

...Y esperamos a que aparezca por pantalla "BUILD SUCCESS"

<img width="1197" height="678" alt="image" src="https://github.com/user-attachments/assets/087f4552-1a90-40ef-8370-5894097144a7" />

A continuación entraremos a la carpeta que nos ha creado (que en mi caso se llama practica-war) con un ```cd practica-war``` , y editamos el archivo pom.xml para poner el siguiente código:

```xml
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/maven-v4_0_0.xsd">
  <modelVersion>4.0.0</modelVersion>
  <groupId>org.zaidinvergeles</groupId>
  <artifactId>tomcat-war-deployment</artifactId>
  <packaging>war</packaging>
  <version>1.0-SNAPSHOT</version>
  <name>tomcat-war-deployment Maven Webapp</name>
  <url>http://maven.apache.org</url>
  <dependencies>
    <dependency>
      <groupId>junit</groupId>
      <artifactId>junit</artifactId>
      <version>3.8.1</version>
      <scope>test</scope>
    </dependency>
  </dependencies>
<build>
   <finalName>tomcat-war-deployment</finalName>
   <plugins>
     <plugin>
       <groupId>org.apache.tomcat.maven</groupId>
       <artifactId>tomcat7-maven-plugin</artifactId>
       <version>2.2</version>
       <configuration>
         <url>http://localhost:8080/manager/text</url>
         <server>Tomcat</server>
         <path>/despliegue</path>
       </configuration>
     </plugin>
   </plugins>
</build>
</project>
```

<img width="1201" height="678" alt="image" src="https://github.com/user-attachments/assets/b2d6b9ad-fe56-49dd-81bb-9566c9d6a1cc" />

En la captura de pantalla se puede observar mejor la parte la cual ha sido editada.

Una vez lo tengamos, ejecutamos ```mvn tomcat7:deploy```, y esperamos a que se despliegue la aplicación. Una vez hecho, nos tendrá que aparecer por pantalla BUILD SUCCESS

<img width="1201" height="677" alt="image" src="https://github.com/user-attachments/assets/4271fbbe-fa5e-4708-9eb0-bdbc5bd7243c" />

Ahora, verificamos que, al entrar a localhost:8080/despliegue se ve correctamente...

<img width="1200" height="676" alt="image" src="https://github.com/user-attachments/assets/136b1673-cc9c-4781-8ea6-e57c9c8a8226" />

Y como se puede observar, la aplicación se ha desplegado con éxito. 

---

# 6. TAREA

---

Para poder desplegar la aplicación de rock-paper-scissors (la cual se nos ha ofrecido), hay que seguir una serie de pasos:

1. Nos descargamos el repositorio de GitHub en nuestra carpeta home haciendo un git clone del repo. Para esto, escribimos ```git clone https://github.com/cameronmcnz/rock-paper-scissors.git```, y esperamos a que se nos "clone" el repo.

<img width="1199" height="673" alt="image" src="https://github.com/user-attachments/assets/09836955-7904-4044-b63c-f4d8b7fe0b5b" />

2. 

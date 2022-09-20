# flow-clima-mysql
Este repositorio representa un ejercicio hecho en Node-Red donde guarda la información en la base de datos mysql

## Introducción
El flow 6 representa el 6 ejercicio hecho en Node-Red usando una base de datos para guardad la información, en este caso se uso Mysql

# Material Necesario
Para realizar este flow necesitas lo siguiente:

1. Ubuntu 20.04
2. NodeJS
-NPM
-NodeRed
-Nodos Dashboard

# Material de referencia

* [Instalación de Virtual Box y Ubuntu 20.04](https://edu.codigoiot.com/course/view.php?id=812)
* [Instalación de NodeRed](https://edu.codigoiot.com/enrol/index.php?id=817)
* [Introducción a NodeRed](https://edu.codigoiot.com/enrol/index.php?id=278)
* [Introducción a MQTT](https://edu.codigoiot.com/course/view.php?id=851)

# Instrucciones
## Requisitos previos
Para que este flow funcione, debes cumplir con los siguientes requisitos previos

1. Instalación de NodeJS. Se recomienda tener instalado NodeJS en alguna versión LTS. Al momento de creación de este documento, se usó la versión 16.17LTS. Esta instalación debe incluir las Build-Tools para hacer uso de NPM

2. Instalación de NodeRed. La instalación se realiza por NPM. Al momento de la creación de este contenido, se usó la versión 3.0.2
3. Cuenta de openweathermap.org la cual es gratuita y nos permitira conocer el clima por una API
4. Gererar un api key que se usara dentro del proyecto en la siguiente pagina https://home.openweathermap.org/api_keys. tomar a consideración que se activa en un lapso de 2 horas
5. Arrancar grafana con la instrucción sudo /bin/systemctl start grafana-server

# Instrucciones de preparación del entorno
Para ejecutar este flow, es necesario lo siguiente

1. Arrancar NodeRed con el comando node-red
2. Clonar el flow5
2. Añadir los siguientes nodos:
   * 1 nodo timestamp
   * 1 nodo function
   * 1 nodo mysql


**nota:** para ver las conexiones de los nodos dirigirse al apartado de resultados que se encuentra mas abajo figura1

3. Para agregar el nodo mysql, dirigirse a la pestaña de manage palatte e instalar *node-red-node-mysql* 
4. Agregar el siguiente código en el nodo function
>```msg.topic = "INSERT INTO clima (`nombre`,
`temperatura`,`humedad`) VALUES ('Daniel'," + global.get("tempAPI") + "," + global.get("humAPI") + ");";
return msg;```

**En una nueva terminal escribir los siguientes comandos**

* `sudo apt install mysql-server` para instalar mysql
* `sudo mysql` para entrar a la base mysql
* `CREATE USER 'newuser'@'localhost' IDENTIFIED BY 'password';` para crear un nuevo usuario y contraseña
* `GRANT ALL PRIVILEGES ON * . * TO 'newuser'@'localhost';` para otorgar los permisos al nuevo usuario
* `create databases codigoIoT;` para crear una base de datos
* `use codigoIoT;` para seleccionar la base de datos creada

Crear una nueva tabla que contenga los campos:
* id, fecha, nombre, temperatura, humedad
`create table clima (id INT (6) UNSIGNED AUTO_INCREMENT PRIMARY KEY, fecha TIMESTAMP DEFAULT CURRENT_TIMESTAMP, nombre CHAR (248) NOT NULL, temperatura FLOAT (4,2), humedad INT (3));`
6. Modificar el nodo mysql con el usuario, contraseña y el nombre de la base de datos que se creo anteriormente

# Instrucciones de operación
Para observar el resultado de este flow modificamos el timestamp del nodo mysql con un intervalo de 1min y damos clic en deploy, ahora sólo es necesario escribir en la terminal el siguiente comando:
`SELECT *FROM clima;` para ver el contenido de la tabla clima como se muestra en la figura 2

### Segunda parte uso de grafana
Para esta parte se añadira el uso de grafana para la visualización de las gráficas por lo que se harán una serie de modificaciones
 1. Eliminar el nodo timestamp de la base de datos y añadir 4 modulos tipo template, tal como se muestra en la figura 3
 2. Modificar el código del nodo function de la base de datos: 
 * >```msg.topic = "INSERT INTO clima (`nombre`,`temperatura`,`humedad`) VALUES ('" + msg.payload.id + "'," + msg.payload.temp + "," + msg.payload.hum + ");";
return msg;```
3. Dirigirse a grafana en el navegador web con la dirección localhost:3000
4. Iniciar sesión el usuario y contraseña sera admin por ser primera vez, posteriormente se modificara la contraseña
5. Agregar una nueva base de datos en la opción data source con los siguientes datos:
   * Seleccionar la opcion MySQL
   * Host: localhost:3306
   * Database: Nombre de la base ya creada anteriormente
   * User & Password: nombre y contraseña que se crearon anteriormente
   * TimeZone: -05:00 para CDMX
Dar clic en save&test para guardar
6. Agregar 1 panel con el parámetro temperatura y mostrara la temperaturá publica de todos, agregar un segundo panel con el parámetro humedad 
7. Para la información personal se debera agregar un nuevo panel y se agregara una condición en el parámetro WHERE donde se indicara que solo muestre la humedad o temperatura de un solo usuario escribiendo tal cual aparece en los registros del ejercicio anterior
8. Para insertar los nuevos paneles de grafana se modificara`;allow_embedding = false` a `allow_embedding = true` en el archivo grafana.ini que esta en la dirección /etc/grafana, y restaurar el servidor de grafana con el comando `sudo /bin/systemctl restart grafana-server`
9. Copiar la dirección de embed html del panel que queremos incrustar dando clic en el nombre, share y pegar en el template de node-red. 
10. Dar clic en deploy y abrir la dirección localhost:1880/ui para ver los gráficos personales figura4 y gráficos públicos figura5

# Resultados
A continuación puede verse una vista previa del resultado de este flow.
Resultados

figura 1 Diagrama de nodos 
![Cargando](https://github.com/DanielRochez/flow-clima-mysql/blob/main/imagen9.png?raw=true)

figura 2 resultado
![Cargando](https://github.com/DanielRochez/flow-clima-mysql/blob/main/imagen11.png?raw=true)

figura 3 diagrama de nodos template
![Cargando](https://github.com/DanielRochez/flow-clima-mysql/blob/main/imagen12.png?raw=true)

figura 4 Información personal con grafana
![Cargando](https://github.com/DanielRochez/flow-clima-mysql/blob/main/imagen13.png?raw=true)

figura 5 Información pública con grafana
![Cargando](https://github.com/DanielRochez/flow-clima-mysql/blob/main/imagen14.png?raw=true)
### Instrucciones para la instalación de grafana 
* `sudo apt-get install -y adduser libfontconfig1`
* `wget https://dl.grafana.com/enterprise/release/grafana-enterprise_9.1.3_amd64.deb`
* `sudo dpkg -i grafana-enterprise_9.1.3_amd64.deb`

# Créditos
Desarrollado por Daniel Rochez

* [GitHub](https://github.com/DanielRochez)

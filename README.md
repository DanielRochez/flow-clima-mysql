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

# Resultados
A continuación puede verse una vista previa del resultado de este flow.
Resultados

figura 1 Diagrama de nodos 
![Cargando](https://github.com/DanielRochez/flow-clima-mysql/blob/main/imagen9.png?raw=true)

figura 2 resultado
![Cargando](https://github.com/DanielRochez/flow-clima-mysql/blob/main/imagen11.png?raw=true)

## Evidencias

# Créditos
Desarrollado por Daniel Rochez

* [GitHub](https://github.com/DanielRochez)

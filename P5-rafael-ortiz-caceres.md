#Practica 5. Replicación de base de datos mysql.
##Creacion de la bd mysql
Primero partiendo que tenemos 2 maquinas con mysql instalado, vamos a duplicar los datos de una maquina 1 principal, en una maquina 2 que sera el backup.
Empezamos por la maquina 1, para iniciar mysql usar el comando:
mysql -uroot -p, e introducir el password de root.

Ahora dentro de mysql, crear la base de datos en la maquina 1, con estas sentencias sql:
	* create database contactos;
	* use contactos;
	* create table datos (Nombre Varchar(100),Tlf int);
	* insert into datos (Nombre,Tlf) values ("pepe",123456789);
Con estas sentencias sql, se ha creado una bd, contactos, en la maquina 1, que tiene una tabla, datos, y una fila introducida con los valores de pepe en nombre y 12345678 en tlf.
![Imgur](http://i.imgur.com/8voIHlU.jpg)

##Duplicar la BD con mysqldump
Primero para evitar que mientras copiamos la BD sea modificada, bloqueamos el acceso a las tablas, desde mysql usamos:
	* FLUSH TABLES WITH READ LOCK;
	* quit;
Ahora podemos replicar la BD.
	* mysqldump contactos -u root p > /root/contactos.sql
Y desbloqueamos la BD;
	* mysql> UNLOCK TABLES;
	* mysql> quit;
![Imgur](http://i.imgur.com/gNGKEpe.jpg)

Desde la maquina 2, copiamos el archivo de la BD clonada, con:
sudo scp root@maquina1:/root/contactos.sql /root/

Con esto ya tenemos los datos de la BD de la maquina 1, en la maquina 2, ahora para restauralos hay que crear primero la BD en la maquina 2.
	* mysql>create database contactos;
	* mysql>quit;
Y ahora introducir los datos copiados a la BD creada nueva.
	* mysql -u root -p contactos < /root/contactos.sql
![Imgur](http://i.imgur.com/QecWbgV.jpg)
![Imgur](http://i.imgur.com/JqSIbGL.jpg)

##Replicacion de BD mediante una configuracion maestro-esclavo

Para configurar 2 maquinas de forma, que la BD de la maquina2 sea replica sincronizada con la maquina 1, lo hacemos de la siguiente forma:
* Primero en la maquina 1, accedemos al archivo de configuracion de mysql que esta en /etc/mysql/my.cnf, y lo editamos cambiando estos parametros.
	* Comentar la linea #  bind-address 127.0.0.1
	* Descomentar la linea server-id=1, si no esta añadirla
	* Descomentar la linea log_error=/var/log/mysql/error.log, si no esta añadirla
	* log_bin=/var/log/mysql/bin.log
Guardamos los cambios hechos en el archivo y reiniciamos mysql.
	* sudo /etc/init.d/mysql restart
![Imgur](http://i.imgur.com/nsjasED.jpg)
* Segundo en la maquina 2, editamos tambien el archivo de configuracion de mysql cambiando esta vez:
	* server-id=2
Guardamos los cambios hechos en el archivo y reiniciamos mysql.
	* sudo /etc/init.d/mysql restart
* Tercero volvemos a la maquina 1, y en mysql usamos estos comandos, para crear un usuario en mysql y asignarle permisos:
	* CREATE USER esclavo IDENTIFIED BY 'esclavo';
	* GRANT REPLICATION SLAVE ON *.* TO 'esclavo'@'%' IDENTIFIED BY 'esclavo';
	* FLUSH PRIVILEGES;
	* FLUSH TABLES;
	* FLUSH TABLES WITH READ LOCK;
* Para acabar con el maestro obtenemos los datos de la BD que vamos a replicar, para usarlos en la configuracion del esclavo.
	* SHOW MASTER STATUS;
![Imgur](http://i.imgur.com/xb6fixs.jpg)
![Imgur](http://i.imgur.com/fkHBTUd.jpg)
![Imgur](http://i.imgur.com/JCOT271.jpg)

* Volvemos finalmente a la maquina 2, esclava, y en mysql introducimos estos comandos para cambiar el archivo de configuracion, en versiones anteriores a la 5.5 se cambian directamente las varibles en el archivo.
	* CHANGE MASTER TO MASTER_HOST = '192.168.197.128',MASTER_USER= 'esclavo',  MASTER_PASSWORD='esclavo', MASTER_LOG_FILE='mysql-bin.000001', MASTER_LOG_POS=501, MASTER_PORT=3306;
	* START SLAVE;
Con estos si todo se ha hecho correctamente, las BD de la maquina 1 y 2 estan sincronizadas, copiandose en la maquina esclavo todo lo de la master.

![Imgur](http://i.imgur.com/YhuM8mu.jpg)
![Imgur](http://i.imgur.com/xpirlgZ.jpg)



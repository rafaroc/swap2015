Tras colocar la mas maquinas bien complentando la practica 1.
La ip de la maquina1 es 192.168.197.128
#Realizar los siguientes pasos

* Instalar rsync con apt-get en las 2 maquinas.
* Generar la pareja de claves privadas/publica en las maquinas. Aunque solo necesitamos pasar la clave publica de la
maquina2 a la maquina1, generar tambien las claves para la maquina1 y que se cree la carpeta .ssh, dentro de esa carpeta
crear un archivo que se llame authorized_keys y ponerle permisos 600 con chmod.
*Para copiar la clave publica de la maquina2 a la maquina1 usar el comando que se indica en el guion ssh-copy-id -i .ssh/id_dsa.pub root@192.168.197.128
Con esto ya podremos conectarnos sin password.
*Ahora programar la tarea de copiado, escribir en el fichero /etc/crontab con el editor vi la siguiente linea
00 * * * * root rsync -avz --delete -e "ssh -l root" root@192.168.197.128:/var/www/ /var/www/

*Con esto la maquina 2 replicara todo el contenido de /var/www/ de la maquina1 en /var/www/ de la maquina2, ademas al añadir --delete si se borra algo en la maquina1 tambien se borrara en la maquina2
![Imgur](http://i.imgur.com/XzIBWSu.jpg)
![Imgur](http://i.imgur.com/MEGZL9A.jpg)


# Práctica 6. Discos en RAID
* Añadimos a una maquina 2 discos duros, en este caso como son maquinas virtuales lo hacemos desde el programa.
![Imgur](http://i.imgur.com/sh0V544.jpg)
* Hacemos fdisk -l para ver los discos y la informacion a usar
![Imgur](http://i.imgur.com/ZLndxqA.jpg)
* Creamos con el programa mdadm el raid1, su el programa no esta instalado usar apt-get intall mdadm.
sudo mdadm -C /dev/md0 --level=raid1 --raid-device=2 /dev/sdb /dev/sdc
![Imgur](http://i.imgur.com/FMN5XL6.jpg)

Creamos tambien una carpeta para montar el raid despues.
Si no ha habido ningun error en la carpeta /datos tenemos el raid1, con 2 discos montado.
![Imgur](http://i.imgur.com/aBtVSZj.jpg)
![Imgur](http://i.imgur.com/WdNTdF8.jpg)


Por ultimo si queremos que cada vez que inicie el equipo el raid /dev/md0 este montado en /datos, añadir a la opcion mount -a, o editar el fichero /etc/fstab y añadir una linea con el nuevo dispositivo
![Imgur](http://i.imgur.com/LbyZSg1.jpg)
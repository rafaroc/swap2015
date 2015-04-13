En esta practica tenemos 3 maquinas
	maquina m1 192.168.197.128 
	maquina m2 192.168.197.129
	maquina m3 192.168.197.130 (Balanceador)

* Primero probamos con nginx el balanceador de carga.
  En la maquina m3 instalamos nginx con sudo apt-get install nginx y  editamos su archivo de configuracion que se encuentra en  /etc/nginx/conf.d/default.conf

![Imgur](http://i.imgur.com/DOlQImD.jpg)

Tiene que quedar de la siguiente manera:
![Imgur](http://i.imgur.com/A21qR2p.jpg)

Con esta configuracion desde la maquina anfitrion o cualquier otra que este fuera de la granja, y despues del balanceador comprobamos que funciona el servicio web accediendo a la maquina m3 balanceadora bien desde el navegador o con curl, y la ip de la maquina m3
![Imgur](http://i.imgur.com/uaiY1ga.jpg)

Con esto comprobamos y nos aseguramos que nginx esta funcionando ya que accedemos a la ip de la maquina 3 y nos esta mostrando la pagina web alojada en las maquinas 1 y 2.
Ahora vamos a realizar la misma tare pero con el haproxy, es similar a nginx pero cambia la forma de editar la configuracion.

Instalamos haproxy en la m3, con sudo apt-get install haproxy y editamos su archivo de configuracion que se encuentra /etc/haproxy/haproxy.cfg
Tiene que quedar esta configuracion:
![Imgur](http://i.imgur.com/pXjS9Eu.jpg)
Reiniciamos haproxy con sudo /usr/sbin/haproxy -f /etc/haproxy/haproxy.cfg
y comprobamos accediendo a la maquina m3 que de nuevo funciona el servicio web
![Imgur](http://i.imgur.com/uvDWS1u.jpg)

 
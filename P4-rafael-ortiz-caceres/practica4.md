#Practica 4 
#Test Apache Benchmark
Para realizar este test, usar una maquina diferente al balanceador o las 2 maquinas servidores.
Desde la maquina4 si tiene apache instalado podra usar apache benchmark, de la siguiente forma:
ab -n 100000 -c 100 http://192.168.197.128/index.html
Este comando envia 100000 peticiones en bloques de 100 a la maquina, en este caso la maquina 1 un servidor apache.
Sustituyendo la ip 128 por 130, he realizado 2 test mas con la maquina 3, usando nginx y haproxy.
##Apache Benchmark Maquina 1
![Imgur](http://i.imgur.com/HVOs3os.jpg)
##Apache Benchmark Maquina 3 nginx
![Imgur](http://i.imgur.com/RtN3uZy.jpg)
##Imagenes del funcionamiento en los servidores
Comando top en Maquina 3 nginx
![Imgur](http://i.imgur.com/2sDm5Ff.jpg)
Top en Maquina 1 con Apache
![Imgur](http://i.imgur.com/YUTCG3d.jpg)
##Carga de los servidores
![Imgur](http://i.imgur.com/F1Z6xuM.jpg)
![Imgur](http://i.imgur.com/IKKM7XE.jpg)
![Imgur](http://i.imgur.com/HmwG76h.jpg)
![Imgur](http://i.imgur.com/AcimRui.jpg)
![Imgur](http://i.imgur.com/kIKIwHf.jpg)



#Test Httperf
Con el comando httperf tambien podemos realizar pruebas de rendimiento de la siguiente forma:
httperf --server http://192.168.197.128 --port 80 --uri /index.html --rate 1500 --num-conn 100000 --num-call 1 --timeout
Este comando lo he usado para la maquina 1 y para la maquina 3 funcionando con nginx y con haproxy. Este por ejemplo seria un ejemplo de un resultado obtenido en la maquina 1

----------------------------------------------------------------------------------------------------------------------------------------------------------------

httperf --timeout=5 --client=0/1 --server=192.168.197.128 --port=80 --uri=/index.html --rate=1500 --send-buffer=4096 --recv-buffer=16384 --num-conns=100000 --num-calls=1
Maximum connect burst length: 265

Total: connections 100000 requests 100000 replies 100000 test-duration 68.665 s

Connection rate: 1456.3 conn/s (0.7 ms/conn, <=893 concurrent connections)
Connection time [ms]: min 0.2 avg 49.8 max 3010.7 median 1.5 stddev 178.3
Connection time [ms]: connect 27.7
Connection length [replies/conn]: 1.000

Request rate: 1456.3 req/s (0.7 ms/req)
Request size [B]: 78.0

Reply rate [replies/s]: min 1326.3 avg 1500.1 max 1674.1 stddev 79.0 (13 samples)
Reply time [ms]: response 22.1 transfer 0.0
Reply size [B]: header 283.0 content 177.0 footer 0.0 (total 460.0)
Reply status: 1xx=0 2xx=100000 3xx=0 4xx=0 5xx=0

CPU time [s]: user 2.81 system 63.62 (user 4.1% system 92.6% total 96.7%)
Net I/O: 765.1 KB/s (6.3*10^6 bps)

Errors: total 0 client-timo 0 socket-timo 0 connrefused 0 connreset 0
Errors: fd-unavail 0 addrunavail 0 ftab-full 0 other 0

-------------------------------------------------------------------------------------------------------------------------------------------------------------------

#Test Openload
Con Openload tambien se pueden realizar test de rendimiento a los servidores, usando el comando de la siguiente forma:
openload http://192.168.197.128 1500

Donde 1500 seria el numero de clientes simultaneos que simula. En este ejemplo es la ip de la maquina 1 pero tambien lo he probado con la maquina 3 con nginx y haproxy.
##Openload Maquina1
![Imgur](http://i.imgur.com/CgWGyZq.jpg)
##Openload Maquina 3 nginx
![Imgur](http://i.imgur.com/EcUafET.jpg)
##Openload Maquina 3 haproxy
![Imgur](http://i.imgur.com/8pTVPZH.jpg)

#Comparacion de resultados de tiempos y rendimiento
![Imgur](http://i.imgur.com/5rIwsUo.jpg)
![Imgur](http://i.imgur.com/R9FAk1f.jpg)

>En la carpeta practica 4 del repositorio se adjunta y estan subidas a github los archivos del resultado de los datos y las tablas en formato excel.
<https://github.com/rafaroc/swap2015/tree/master/P4-rafael-ortiz-caceres/resultadosswap>


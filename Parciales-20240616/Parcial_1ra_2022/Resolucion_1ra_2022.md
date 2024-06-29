# Redes y comunicaciones - 1ra fecha (21/06/2022) 
Siempre es necesario justificar, las respuestas no debidamente justificadas serán consideradas incorrectas. 
En los ejercicios  de subnetting  deberá  ser claro tanto  el por qué se selecciona  una red y como así también se deben dejar expresados los cálculos o explicado el proceso para obtener el resultado. 
*Al comenzar cada ejercicio se suponen todas las tablas vacías (CAM, Caché, ARP). Para  mencionar  la dirección MAC  de un dispositivo utilice la notación: MAC_dev_iface. Ej.: la MAC de pc-B  será MAC- PC-B-  ethO.*  

1. La persona encargada de la organización redes.edu.ar requiere de su servicio de consultoría. Le presenta una explicación con el siguiente diagrama:  
    REQUISITOS: 
    • Ambos host WWW comparten un filesystem y solo alojan tres sitios estáticos www.redes.edu.ar, sedeprincipal.redes.edu.ar y sedesecundaria.redes.edu.ar.  Debido  a  la  alta  demanda  es  necesario  balancear carga entre los servidores. 
    • El host MAIL es un servidor SMTP utilizado por aplicaciones para enviar correos a los usuarios. No cumple ninguna otra función. 
    • NS1 y NS2 brindaran respuestas autoritativas para el dominio. 
    • El personal de IT usa el nombre  FTP para acceder a dicho host, pero los usuarios prefieren un nombre más semántico como "sharedfolder".
    ![diagrama ejercicio 1](/Parciales-20240616/Parcial_1ra_2022/imgs/image.png)
    a) Liste los registros DNS que debería tener configurado ns1 para cumplir con los requisitos  dados. 

    Cosas a tener en cuenta: debemos configurar los registros A para poder obtener las direcciones IP de los sitios y de los servidores NS; se debe configurar un registro MX por donde se enviarán los correos electrónicos; debemos tener un registro CNAME para que los usuarios conozcan al personal de IT por el nombre semántico sharedfolder.
    ```
        redes.edu.ar NS ns1.redes.edu.ar
        redes.edu.ar NS ns2.redes.edu.ar

        ns1.redes.edu.ar A 198.51.100.195
        ns2.redes.edu.ar A 203.0.113.67

        www.redes.edu.ar A 198.51.100.196
        www.redes.edu.ar A 203.0.113.68
        sedeprincipal.redes.edu.ar CNAME www.redes.edu.ar
        sedesecundaria.redes.edu.ar CNAME www.redes.edu.ar

        redes.edu.ar MX ftp.redes.edu.ar
        ftp.redes.edu.ar CNAME sharedfolder.redes.edu.ar

        mail.redes.edu.ar A 198.51.100.194
    ```


    b) Se  desea  configurar  un  nuevo  host  en  la  red  de  usuarios  de  Sede  Principal.  Indique  todos  los  valores  de  red  que  el  técnico  de  la  red  debería  configurar  para  que  el  host  pueda  conectarse a Internet y a los recursos de la organización.

    ```
        Se quiere configurar un nuevo host, el cual recibirá el nombre de PC-G, cuya dirección IP será 192.0.2.133/25.

        Se tiene que configurar dentro de él un default gateway principal cuya dirección será 192.0.2.129 para que pueda conectarse a internet y acceder a los recursos de la organización. 
    ```

    c) Si  un  usuario  en  PC-C  ingresa  mediante  su  navegador  a  http://www.redes.edu.ar  ¿es  posible  determinar a qué host llegará esa solicitud?

    ```
        Debido a la configuración que posee la red, la solicitud debería llegar al host ns1, pero depende de si dicho host no está caido o colapsado.
    ```
    
    d) Cuándo  cualquiera  de  los  hosts  "www"  recibe  una  solicitud,  ¿Qué  característica  del  protocolo  en  cuestión permite determinar qué sitio dentro de los que aloja debe presentar al cliente?

    ```
        La caracterísitica del protocolo HTTP que permite saber a un host a qué sitio desea acceder el cliente, es la cabecera host, ya que ahí es donde se indica el recurso deseado.
    ```
---
2.  Dada la siguiente salida del comando ss -nltu en el host A:
    ![tabla ejercicio 2](/Parciales-20240616/Parcial_1ra_2022/imgs/image-1.png)
    a. ¿Puede determinar y listar las direcciones IP que tiene asignadas host A? 

    ```
        Podemos ver que el host A tiene dos direcciones IP asignadas:
        127.0.0.1 -> dirección de loopback, localhost.
        192.168.22.15 -> asignada por el servidor DHCP.
    ```

    b. ¿A qué fase de la conexión se corresponde el estado de la línea 12? Independientemente de quien  inicia esta fase, brinde dos intercambios de mensajes posibles asociados.

    ```
        El estado de conexión TIME-WAIT indica que el servidor cerró su parte de la conexión y el cliente está esperando un determinado tiempo, para asegurarse de que lleguen todos los paquetes que faltan.
    ```

    c. Indique como se vería con TCPDUMP la respuesta a los siguientes paquetes observados en el host A (incluya toda la informacion posible: direcciones IP, puertos, flags y numeras de secuencia y confirmación):
        a.  192.168.22.20:35794 > <ip_host_A> :80 : Flags [S], seq 110012 , ack 0
        ```sh
            IP_origen: ip_host_A:80
            IP_destino: 192.168.22.20:35794
            Flags: SYN/ACK
            SEQ: 0
            ACK: 110013
        ```
        b.      192.168.22.20:35794 >  <ip_host_A> :9100 : UDP , length 1024
        ```sh
            Al ser una conexión UDP no hay respuesta, a menos que haya un mensaje ICMP de error.
        ```
        c.  192.168.22.20:33444 > <ip_host_A> :8080 : Flags [S], seq 552201, ack 0
        ```sh
            IP_origen: ip_host_A:8080
            IP_destino: 192.168.22.20:33444
            Flags: SYN/ACK
            SEQ: 0
            ACK: 552202
        ```
---
3.  Un usuario indica que utilizando un navegador accede a http://www.redes.edu/hola.txt y tiene problemas para visualizar el recurso solicitado. Usted se conecta al servidor y obtiene la siguiente captura de tráfico:
    ```r
    172.16.8.16.35794 > 192.168.22.31.80:  Flags [S],  seq 558012, win 65495, length 0 
    192.168.22.31.80  > 172.16.8.16.35794:  Flags [SA], seq 160879, ack 558012, win 65483, length 0 
    172.16.8.16.35794 > 192.168.22.31.80 :  Flags [RA], ack 1, win 404, length 0 
    ```
    a. ¿En qué protocolo ubicaría el problema? ¿el problema está en el cliente o en el servidor?

    ```
        El protocolo donde ser ubica el problema es TCP y en el servidor, esto se debe a que pone mal el ACK, ya que el cliente debería recibir el ACK con valor 558013 (1 más que cuando se estableció la conexión). Otro error que puede verse es que el tamaño de ventana se achica aunque el cliente esté recibiendo algo con longitud 0.
    ```

    b. ¿Qué datos del protocolo HTTP llegaron a ser intercambiados? 
    ```
        Se envío el http request, luego de eso, el servidor devuelve un código de error 500.
    ```
 ---
4.  Utilizando CIDR, indique cuáles de los siguientes bloques pueden ser agrupados y determine el bloque CIDR resultante de dicha sumarización: 113.33.215.0/24, 113.33.216.0/24, 113.33.217.0/24 y  113.33.218.0/23. 

Todos los bloques pueden ser agrupados en un mismo bloque CIDR de la siguiente forma:

113.33.1101 0111.0000000 -> 113.33.215.0/24
113.33.1101 1000.0000000 -> 113.33.216.0/24
113.33.1101 1001.0000000 -> 113.33.217.0/24
113.33.1101 1010.0000000 -> 113.33.218.0/23

Podemos ver que hasta el bit número 20 son iguales, con lo cual el bloque CIDR resultante será: 113.33.208.0/20, tenemos 20 bits que pueden utilzarse para hacer redes y 12 bits reservados para hosts.

---
5.  Utilizando el prefijo de red 13.14.56.0/23, asigne direcciones IP a las siguientes subredes, las cuales  tienen los siguientes requerimientos: Red A (117 hosts), Red B (97 hosts), Red C (192 hosts). 

Para hacer la asignación de IP a las redes necesarias, se utilizará el método VLSM. 

Tomamos primero la red que más hosts necesitará, en este caso es la Red C.

Red C: necesita 192 hosts, con lo cual utilizará 8 bits para host (2^8=255 hosts disponibles). Teniendo la red 13.14.56.0/23, debemos reacomodar la máscara de la misma para que queden solamente 8 bits de host:

00001101.00001110.00111000.0000000 - 13.14.56.0
11111111.11111111.11111110.0000000 - mask /23
11111111.11111111.11111111.0000000 - nueva mask subred /24

Podemos ver que obtenemos 1 bit más para hacer redes, con lo cual nos quedarían 2 redes, las cuales son:

00001101.00001110.00111000.00000000 - 13.14.56.0/24 (asignada a Red C)
00001101.00001110.00111001.00000000 - 13.14.57.0/24 (disponible para seguir subnetteando)

Ahora tomamos la 2da red que más hosts necesita, que en este caso es la Red A.

Red A: necesita 117 hosts, con lo cual utilizará 7 bits para hosts (2^7=128 hosts disponibles). Utilizamos la red 13.14.57.0/24 que nos quedó disponible de la asignación de Red C para subnettear.

00001101.00001110.00111001.00000000 - 13.14.57.0
11111111.11111111.11111111.00000000 - mask subred /24
11111111.11111111.11111111.10000000 - nueva mask subred /25

Nuevamente obtenemos 1 bit más para redes y nos quedan los 7 bits que necesitamos para hosts.

00001101.00001110.00111001.00000000 - 13.14.57.0/25 (Asignada a Red A)
00001101.00001110.00111001.10000000 - 13.14.57.128/25 (disponible para seguir subnetteando).

Por último nos queda hacer la asginación a la Red B, la cual necesita 97 hosts, con lo cual utilizará 7 bits para hosts (2^7=128 hosts disponibles). En este caso también tomamos la red que nos quedó disponible para hacer subnetting de la asignación de Red A y como podemos ver, ya tiene los 7 bits que se necesitan para hosts, con lo cual, se asigna como dirección a la Red b sin necesidad de hacer modificaciones:

00001101.00001110.00111001.10000000 - 13.14.57.128/25 - (Asignada a Red B)

---
6.  Indique todas las direcciones IPv6 con las que se autoconfigura una PC utilizando EUl-64 considerando que: Tiene la dirección MAC 3c:e5:8d:96:9a:b5. Está conectada a un segmento de red en el que se recibe un Router Advertisement del prefijo 2900:aabb::/64.

Tomamos la dirección IPv6 y la dividimos en dos partes de 32 bits cada una:
Primera mitad: 3c:e5.:8d
Segunda mitad: 96:9a:b5

En la primera mitad, tomamos el 7 bit más significativo y lo invertimos:
3C - 0011 1100 - después de invertir quedaría 0011 1110 => 3E

Juntamos las dos mitades y agregamos FF:FE entre ellas:

3E:E5:8D:FF:FE:96:9A:B5

Por último debemos agregar el prefijo 2900:AABB:: y los agrupo de a 16 bits:

2900:AABB::3EE5:8DFF:FE96:9AB5

---
7.  Dada la siguiente tabla de rutas: Indique por cuál interfaz se enviarán  los  siguientes  paquetes  y  en  cada  uno,  la  dirección  MAC de  destino  que  tendría  que  usar  (suponga que la conoce).
![tabla de rutas ejercicio 7](/Parciales-20240616/Parcial_1ra_2022/imgs/image-2.png)
![tabla ejercicio 7](/Parciales-20240616/Parcial_1ra_2022/imgs/image-3.png)

---
8. Considerando la siguiente topología y luego de que pc-A hace un ping exitoso a pc-C:
    ![topologia ejercicio 8](/Parciales-20240616/Parcial_1ra_2022/imgs/image-4.png)
    a. Complete la información del ARP request que realiza pe-A. Incluya la información de los protocolos  ethernet y ARP.
    b. Indique cómo quedan las tablas CAM de los dispositivos que están en el mismo segmento de broadcast que pc-A.
    c. Si en pc-B hubiesen estado capturando tráfico en la interfaz eth0, ¿que hubiesen escuchado? Para cada paquete capturado, indique el protocolo y si se trata de un requerimiento o una respuesta. 
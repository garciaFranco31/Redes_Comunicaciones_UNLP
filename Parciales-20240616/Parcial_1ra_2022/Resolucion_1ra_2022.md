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
    b) Se  desea  configurar  un  nuevo  host  en  la  red  de  usuarios  de  Sede  Principal.  Indique  todos  los  valores  de  red  que  el  técnico  de  la  red  debería  configurar  para  que  el  host  pueda  conectarse a Internet y a los recursos de la organización.
    c) Si  un  usuario  en  PC-C  ingresa  mediante  su  navegador  a  http://www.redes.edu.ar  ¿es  posible  determinar a qué host llegará esa solicitud?
    d) Cuándo  cualquiera  de  los  hosts  "www"  recibe  una  solicitud,  ¿Qué  característica  del  protocolo  en  cuestión permite determinar qué sitio dentro de los que aloja debe presentar al cliente?
---
2.  Dada la siguiente salida del comando ss -nltu en el host A:
    ![tabla ejercicio 2](/Parciales-20240616/Parcial_1ra_2022/imgs/image-1.png)
    a. ¿Puede determinar y listar las direcciones IP que tiene asignadas host A? 
    b. ¿A qué fase de la conexión se corresponde el estado de la línea 12? Independientemente de quien  inicia esta fase, brinde dos intercambios de mensajes posibles asociados.
    c. Indique como se vería con TCPDUMP la respuesta a los siguientes paquetes observados en el host A (incluya toda la informacion posible: direcciones IP, puertos, flags y numeras de secuencia y confirmación):
        a.  192.168.22.20:35794 > <ip_host_A> :80 : Flags [S], seq 110012 , ack 0
        b.      192.168.22.20:35794 >  <ip_host_A> :9 100 : UDP , length 1024
        c.  192.168.22.20:33444 > <ip_host_A> :8080 : Flags [S], seq 552201, ack 0
---
3.  Un usuario indica que utilizando un navegador accede a http://www.redes.edu/hola.txt y tiene problemas para visualizar el recurso solicitado. Usted se conecta al servidor y obtiene la siguiente captura de tráfico:
    ```r
    172.16.8.16.35794 > 192 .168 .22 .31. 80:  Flags [S],  seq 55 80 12, w in 6 5495, length 0 
    192.16 8.22 .31.80  > 17 2 .16. 8 .1 6 .3 57 9 4:  Flags [SA], seq 16 08 79 , ack 558012, win 65483 , length 0 
    172.16. 8 .16. 357 94 > 192 .1 68 .22 . 3 1. 80 :  Flags [RA], ack 1, win 404 , length 0 
    ```
    a. ¿En qué protocolo ubicaría el problema? ¿el problema está en el cliente o en el servidor?
    b. ¿Qué datos del protocolo HTTP llegaron a ser intercambiados? 

 ---
4.  Utilizando CIDR, indique cuáles de los siguientes bloques pueden ser agrupados y determine el bloque CIDR resultante de dicha sumarización: 113.33.215.0/24, 113.33.216.0/24, 113.33.217.0/24 y  113.33.218.0/23. 

---
5.  Utilizando el prefijo de red 13.14.56.0/23, asigne direcciones IP a las siguientes subredes, las cuales  tienen los siguientes requerimientos: Red A (117 hosts), Red B (97 hosts), Red C (192 hosts). 

---
6.  Indique todas las direcciones IPv6 con las que se autoconfigura una PC utilizando EUl-64 considerando que: Tiene la dirección MAC 3c:e5:8d:96:9a:b5. Está conectada a un segmento de red en el que se recibe un Router Advertisement del prefijo 2900:aabb::/64.

---
7.  Dada la siguiente tabla de rutas: Indique por cuál interfaz se enviarán  los  siguientes  paquetes  y  en  cada  uno,  la  dirección  MAC de  destino  que  tendría  que  usar  (suponga que la conoce).
![tabla de rutas ejercicio 7](/Parciales-20240616/Parcial_1ra_2022/imgs/image-2.png)
![tabla ejercicio 7](/Parciales-20240616/Parcial_1ra_2022/imgs/image-3.png)

---
8. Considerando la siguiente topología y luego de que pe-A hace un ping exitoso a pc-C:
    ![topologia ejercicio 8](/Parciales-20240616/Parcial_1ra_2022/imgs/image-4.png)
    a. Complete la información del ARP request que realiza pe-A. Incluya la información de los protocolos  ethernet y ARP.
    b. Indique cómo quedan las tablas CAM de los dispositivos que están en el mismo segmento de broadcast que pc-A.
    c. Si en pc-B hubiesen estado capturando tráfico en la interfaz eth0, ¿que hubiesen escuchado? Para cada paquete capturado, indique el protocolo y si se trata de un requerimiento o una respuesta. 
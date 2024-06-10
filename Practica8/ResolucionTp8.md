# PRÁCTICA 8 - CAPA DE RED: FRAGMENTACIÓN - RUTEO

## Fragmentación
1. Se tiene la siguiente red con los MTUs indicados en la misma. Si desde pc1 se envía un paquete IP a pc2 con un tamaño total de 1500 bytes (cabecera IP más payload) con el campo Identification = 20543, responder:
![alt text](/Practica8/imgs/ejercicio1.png)
* Indicar IPs origen y destino y campos correspondientes a la fragmentación cuando el paquete sale de pc1
* ¿Qué sucede cuando el paquete debe ser reenviado por el router R1?
* Indicar cómo quedarían los paquetes fragmentados para ser enviados por el enlace entre R1 y R2.
* ¿Dónde se unen nuevamente los fragmentos? ¿Qué sucede si un fragmento no llega?
* Si un fragmento tiene que ser reenviado por un enlace con un MTU menor al tamaño del fragmento, ¿qué hará el router con ese fragmento? Ruteo

a) 
```
    IP origen: 10.0.0.20
    IP destino: 10.0.2.20
    Identificación: 20543
    Longitud total: 1500
    Flag DF: 0 (false)
    Flag MF: 0 (false)
    Offset de fragmento: 0
```

b y c)
Para poder hacer este ejercicio, tenemos que tener en cuenta ciertas cosas:
* El header de identificación siempre es 20543
* Se pone el flag MF en true, salvo en el último fragmento.
* Como el MTU es de 600 bytes (20 son headers), tenemos que dividir el paquete en diferentes fragmentos, cada uno tiene que tener un tamaño menor o igual a 600 bytes. Esto lo logramos de la siguiente forma:
  * tomamos 600 bytes, a los cuales les restamos 20 bytes (headers) => 600 - 20 = 580, el problema que tenemos con 580 es que no es múltiplo de 8, con lo cual, tenemos que buscar el siguiente número más bajo que sí lo sea, en este caso 576.
  * Debido a que el campo Total Length incluye la longitud de los headers será de 576 + 20 = 596
  * Por lo tanto faltarán por enviarse 1480 - 576 = 904 bytes


```
Fragmento 1
    Header: 20
    Tamaño total: 596
    Identificación: 20543
    DF Flag: 0
    MF Flag: 1
    Fragment Offset: 0

Fragmento 2
    Header: 20
    Tamaño total: 596
    Identificación: 20543
    DF Flag: 0
    MF Flag: 1
    Fragment Offset: 72

Fragmento 3
    Header: 20
    Tamaño total: 348
    Identificación: 20543
    DF Flag: 0
    MF Flag: 0
    Fragment Offset: 144
```

* El fragment offset se calcula de la siguiente forma:
   sumatoria(tamaño_fragmento_anterior)/8
   Ej. En el caso del fragmento 2, sería 576 (596-20, ya que el tamaño total toma en cuenta a los headers (20 bytes)) => 576/8 = 72 => frgment Offset = 72

   En el caso del fragmento 3, sería (576+576)/8 = 144 (es 576 por lo mismo que en el ej anterior 596-20). 

d) Los fragmentos vuelven a unirse en el host destino (PC2). En caso de que un fragmento no llegue, dependientdo del protocolo de la capa de transporte, es posible que se deba volver a transmitir todos los fragmentos del paquete.

e) En ese caso, el router volverá a fragmentar ese fragmento, para poder enviarlo.

---
2. ¿Qué es el ruteo? ¿Por qué es necesario?

El ruteo es el proceso para determinar la ruta que deben seguir los datos o paquetes para llegar de un origen a un destino por medio de la red. Es decir, es la función de seleccionar la interfaz mediante la cual voy a enviar un dato, el cual será recibido por otro router (que también seleccionará la interfaz por donde lo va a recibir).

Es necesario ya que permite formar caminos entre el host origen y el host destino pasando por los nodos intermedios.

---
3. En las redes IP el ruteo puede configurarse en forma estática o en forma dinámica. Indique ventajas y desventajas de cada método.




| Estática  | Dinámica |
| :-------- | :------- |
| Simplicidad: es fácil de configurar y enteder.| Adaptabilidad: se ajusta automáticamente a cambios en la red (enlaces caídos, nuevas rutas,...). Mejora la resiliencia de la red.|
| Control Total: el administrador ded red tiene un control total sobre las rutas y puede diseñar la red según sus necesidades específicas.| Escalabilidad: es más adecuado para redes grandes y complejas, ya que la configuración se propaga automáticamente a través de la red.|
| Menos carga en la red: generalmente genera menos tráfico de enrutamiento en la red, debido a que las rutas se configuran manualmente y no cambian automáticamente.| Eficiendia de recursos: puede encontrar rutas óptimas en función de métricas como la velocidad o la carga de los enlaces, lo que mejora la eficiencia del tráfico.|


| Estática  | Dinámica |
| :-------- | :------- |
| No se adapta a cambios: no se ajusta automáticamente a cambios en la topología de la red, esto quiere decir que si una ruta falla o cambia, se tiene que actualizar manualmente.| Mayor complejidad: requiere un conocimiento más profundo, debido a que la configuración y el mantenimiento puede ser más complejo que cuando es estático.|
| No es escalable: en redes grandes y complejas, la gestión manual de rutas puede ser abrumadora y propensa a errores.| Mayor tráfico de enrutamiento: genera más tráfico de enrutamiento en la red, esto debido a que los routers intercambian información sobre las ruts, lo que puede consumir ancho de banda.|
| Menos eficiente en términos de tiempo: puede ser más demorado que utilizar enrutamiento dinámico en redes grandes.| Posible inestabilidad: si la configuración no se realiza de manera adecuada, el enrutamiento dinámico puede causar problemas de estabilidad en la red.|

---
4. Una máquina conectada a una red pero no a Internet, ¿tiene tabla de ruteo?

Si, esto se debe a que al estar conectada a una red local, tiene la tabla de enrutamiento que le permite gestionar la comunicación dentro de dicha red local. Esta tabla es necesaria para determinar cómo enviar datos entre los dispositivos que se encuentran conectados dentro de dicha red local.

---
5. Observando el siguiente gráfico y la tabla de ruteo del router D, responder:
    ![alt text](/Practica8/imgs/ejercicio5.png)
    a. ¿Está correcta esa tabla de ruteo? En caso de no estarlo, indicar el o los errores   encontrados. Escribir la tabla correctamente (no es necesario agregar las redes que conectan contra los ISPs)
    b. Con la tabla de ruteo del punto anterior, Red D, ¿tiene salida a Internet? ¿Por qué? ¿Cómo lo solucionaría? Suponga que los demás routers están correctamente configurados, con salida a Internet y que Rtr-D debe salir a Internet por Rtr-C.
    c. Teniendo en cuenta lo aplicado en el punto anterior, si Rtr-C tuviese la siguiente entrada en su tabla de ruteo, ¿qué sucedería si desde una PC en Red D se quiere acceder un servidor con IP 163.10.5.15? Red Destino Mask Next-Hop Iface 163.10.5.0 /24 10.0.0.9 eth1
    d. ¿Es posible aplicar sumarización en la tabla del router Rtr-D? ¿Por qué? ¿Qué debería suceder para poder aplicarla?
    e. La sumarización aplicada en el punto anterior, ¿se podría aplicar en Rtr-B? ¿Por qué?
    f. Escriba la tabla de ruteo de Rtr-B teniendo en cuenta lo siguiente:
        * Debe llegarse a todas las redes del gráfico
        * Debe salir a Internet por Rtr-A
        * Debe pasar por Rtr-D para llegar a Red D
        * Sumarizar si es posible
    g. Si Rtr-C pierde conectividad contra ISP-2, ¿es posible restablecer el acceso a Internet sin esperar a que vuelva la conectividad entre esos dispositivos?

a) la tabla de ruteo no es correcta, podemos ver ciertos errores, como:
* La dirección de Next-Hop 10.0.0.5/30 no es correcta, debido a que la misma no debe tener la máscara.
* Falta la dirección 10.0.0.8 la cuál conecta el Rtr D con el Rtr C.
* La dirección de red 205.10.128.0 no existe en el gráfico.
* La dirección de red 205.20.0.128 no es válida, debido a que es una dirección de host, no de red.

|Red Destino | Mask | Next-Hop | iface |
|:-----------|:-----|:---------|:------|
|153.10.20.128 | /27 | - | eth1 |
|10.0.0.4 | /30 | - | eth0 |
|10.0.0.0 | /30 | - | eth5 |
|10.0.0.8 | /30 | - | eth3 |
|10.0.0.12 | /30 | 10.0.0.5 | eth0 |
|10.0.0.16 |/30 | 10.0.0.10 | eth3 |
|205.20.0.192| /26 | 10.0.0.5 | eth0 |
|205.20.0.128 | /26 | 10.0.0.5 | eth0 |
|163.10.5.64 | /27 | 10.0.0.10 | eth3 |
---
b) No tiene salida a internet, esto debidoa que a Rtr D le falta el gateway default (ninguna de sus entradas lleva a un ISP). Esto podría solucionarse usando a Rtr A o Rtr C como default gateway de la siguiente forma:

|Red Destino | Máscara | Sig. Gateway | Interfaz |
|:-----------|:--------|:-------------|:---------|
|0.0.0.0  | /0 | 10.0.0.10 | eth3 |

c) Se daría la siguiente secuencia:
* Rtr-D
    (1) recibe el paquete y decrementa el TTL
    (2) aplica la máscara de red y reconoce que el paquete se dirige a la red 0.0.0.0/0
    (3) para que siga su camino, lo envía al destino 10.0.0.10/30 a través de la interfaz eth3.

* Rtr-C 
    (1) recibe el paquete y decrementa el TTL
    (2) aplica la máscara de red y reconoce que el paquete se dirige a la red 163.10.5.0/24
    (3) para que siga su camino, lo envía al destino 10.0.0.9/30 a través de la interfaz eth1.

Los dos routers van a seguir haciendo lo descripto anteriormente, hasta que el TTL llegue a 0, cuando esto pase uno de los dos routers va a descartar el paquete.

d) Es posible realizar la sumarización en el caso de las redes 205.20.0.192/26 y 205.20.0.128/26, debido a que tienen el mismo salto, la misma máscara y la misma interfáz.




---
6. Evalúe para cada caso si el mensaje llegará a destino, saltos que tomará y tipo de respuesta recibida en el emisor.
![alt text](/Practica8/imgs/ejercicio6.png)
* Un mensaje ICMP enviado por PC-B a PC-C.
* Un mensaje ICMP enviado por PC-C a PC-B.
* Un mensaje ICMP enviado por PC-C a 8.8.8.8.
* Un mensaje ICMP enviado por PC-B a 8.8.8.8.

## DHCP y NAT
7. Con la máquina virtual con acceso a Internet realice las siguientes observaciones respecto de la autoconfiguración IP vía DHCP:
    a. Inicie una captura de tráfico Wireshark utilizando el filtro bootp para visualizar únicamente tráfico de DHCP.
    b. En una terminal de root, ejecute el comando $ sudo /sbin/dhclient eth0 y analice el intercambio de paquetes capturado.
    c. Analice la información registrada en el archivo /var/lib/dhcp/dhclient.leases, ¿cuál parece su función?  
    d. Ejecute el siguiente comando para eliminar información temporal asignada por el servidor DHCP. $ rm /var/lib/dhcp/dhclient.leases
    e. En una terminal de root, vuelva a ejecutar el comando $ sudo /sbin/dhclient eth0 y analice el intercambio de paquetes capturado nuevamente ¿a que se debió la diferencia con lo observado en el punto “b”?
    f. Tanto en “b” como en “e”, ¿qué información es brindada al host que realiza la petición DHCP, además de la dirección IP que tiene que utilizar?

---
8. ¿Qué es NAT y para qué sirve? De un ejemplo de su uso y analice cómo funcionaría en ese entorno. Ayuda: analizar el servicio de Internet hogareño en el cual vario dispositivos usan Internet simultáneamente.

---
9. ¿Qué especifica la RFC 1918 y cómo se relaciona con NAT?

---
10. En la red de su casa o trabajo verifique la dirección IP de su computadora y luego acceda a www.cualesmiip.com. ¿Qué observa? ¿Puede explicar qué sucede?

---
11. Resuelva las consignas que se dan a continuación.
    a. En base a la siguiente topología y a las tablas que se muestran, complete los datos que faltan.
    ![alt text](/Practica8/imgs/ejercicio11_a.png)
    ```
    PC-A (ss)
    Local Address:Port Peer Address:Port
    192.168.1.2:49273 _________________
    _________________ 190.50.10.63:25
    192.168.1.2:_____ 190.50.10.81:8080
    PC-B (ss)
    Local Address:Port Peer Address:Port
    192.168.1.3:52734 _________________
    192.168.1.3:39275 _________________
    RTR-1 (Tabla de NAT)
    Lado LAN Lado WAN
    192.168.1.2:49273 205.20.0.29:25192
    192.168.1.2:51238 _________________
    192.168.1.3:52734 205.20.0.29:51091
    192.168.1.2:37484 205.20.0.29:41823
    192.168.1.3:39275 205.20.0.29:9123
    SRV-A (ss)
    Local Address:Port Peer Address:Port
    190.50.10.63:80 205.20.0.29:25192
    190.50.10.63:25 205.20.0.29:41823
    SRV-B (ss)
    Local Address:Port Peer Address:Port
    190.50.10.81:8080 205.20.0.29:16345
    190.50.10.81:8081 205.20.0.29:51091
    190.50.10.81:8080 205.20.0.29:9123
    ```
    b. En base a lo anterior, responda:
        i. ¿Cuántas conexiones establecidas hay y entre qué dispositivos?
        ii. ¿Quién inició cada una de las conexiones? ¿Podrían haberse iniciado en sentido inverso? ¿Por qué? Investigue qué es port forwarding y si serviría como solución en este caso.

---
## EJERCICIO DE PARCIAL
![alt text](/Practica8/imgs/ejercicio_parcial.png)

1. Asigne las redes que faltan utilizando los siguientes bloques y las consideraciones debajo:
        <strong style="color: red">
            226.10.20.128/27 200.30.55.64/26 127.0.0.0/24 192.168.10.0/29
            224.10.0.128/27 224.10.0.64/26 192.168.10.0/24 10.10.10.0/27
        </strong>
        * Red C y la Red D deben ser públicas.
        * Los enlaces entre routers deben utilizar redes privadas.
        * Se debe desperdiciar la menor cantidad de IP posibles.
        * Si va a utilizar un bloque para dividir en subredes, asignar primero la red con más cantidad de hosts y luego las que tienen menos.
        * Las redes elegidas deben ser válidas.

2. Asigne IP a todas las interfaces de las redes listadas a continuación. **Nota: Los routers deben tener asignadas las primeras IP de la red. Para enlaces entre routers, asignar en el siguiente orden: RouterA, RouterB, RouterC, RouterD y RouterE.**
    * Red A, Red B, Red C y Red D.
    * Red entre RouterA-RouterB-RouterE.
    * Red entre RouterC-RouterD.

3. Realice las tablas de rutas de RouterE y BORDER considerando:
    * Siempre se deberá tomar la ruta más corta.
    * Sumarizar siempre que sea posible.
    * El tráfico de Internet a la Red D y viceversa debe atravesar el RouterC.
    * Todos los hosts deben poder conectarse entre sí y a Internet

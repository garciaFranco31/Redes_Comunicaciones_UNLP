# PRÁCTICA 5 - CAPA DE TRANSPORTE (PARTE 1)

1. ¿Cuál es la función de la capa de transporte?

La capa de transporte es la encargada de proporcionar directamente servicios de comunicación a los procesos de aplicación que se ejecutan en hosts diferentes.
Cada protocolo de la capa de transporte proporciona una *Comunicación lógica (hace ver como que los hosts que se comunican están conectados directamente)* entre procesos de aplicación que se ejecutan en hosts diferentes.

---
2. Describa la estructura del segmento TCP y UDP.
    ![Estructura del segmento TCP](imgs/segmentoTCP.png)
    
    <!-- TODO completar -->


    ![Estructura del segmento UDP](imgs/segmentoUDP.png)

* Los datos de la aplicación ocupan el campo de datos del segmento UDP.
* La cabecera UDP solo tiene cuatro campos, y cada uno de ellos tiene una longitud de dos bytes.
* Los números de puerto permiten al host de destino pasar datos de la aplicación al proceso apropiado que está ejecutándose en el sistema terminal de destino.
* El campo de longitud especifíca el número de bytes del segmento UDP (la cabecera más los datos).
* El host receptor utiliza la suma de comprobación para detectar si se han introducido errores en el segmento.
  
---
3. ¿Cuál es el objetivo del uso de puertos en el modelo TCP/IP?

Los puertos se utilizan para distinguir las aplicaciones y los protocolos que están enviando/recibiendo datos.
Los puertos actúan como puntos finales en una comunicación y permiten que múltiples aplicaciones en una misma computadora/dispositivo se comuniquen simultáneamente a través de la red.

---
4. Compare TCP y UDP en cuanto a:
    a. Confiabilidad: TCP es un modelo confiable debido a que garantiza que los datos se entregue en el orden correcto y sin errores. En cambio, UDP es menos confiable debido a que no garantiza ni la entrega de los datos, ni que los mismos se entreguen en orden.
    
    ---
    b. Multiplexación: En el caso de TCP, la multiplexación y demultiplexación es orientada a la conexión, cuando llega de la procedente de la red a un host un segmento TCP, el host emplea los valores de IP orígen, número de puerto de orígen, IP destino y número de puerto de destino para demultiplexar el segmento al socket apropiado.
    En el caso de UDP la multiplexación y demultiplexación no es orientada a la conexión. El socket UDP queda identificado por la dirección IP destino y un número de puerto de destino, por lo tanto si dos segmentos UDPP con diferentes direcciones IP y puertos de orígen distintos, tienen el mismo puerto destino y la misma dirección IP destino, entonces el segmento se envía al mismo proceso de destino por medio  del mismo socket.

    ---
    c. Orientado a la conexión: TCP establece una conexión antes de iniciar con la transmisión de los datos, para así asegurarse que ambas partes estén sincronizadas hablando de secuencia de datos y control de flujo. En el caso de UDP no se necesita una conexión para poder iniciar/finalizar una transferencia de datos.
    
    ---
    d. Controles de congestión: TCP proporciona mecanismos de control de congestión, para así evitar que cualquier conexión TCP sature con una cantidad de tráfico considerable los enlaces y routers existentes entre los hosts. UDP no tiene su tráfico regulado, or lo tanto una aplicación que emplee el protocolo de transporte UDP puede enviar los datos que quiera a la velocidad que quiera, durante todo el tiempo que quiera.
    
    ---
    e. Utilización de puertos: TCP establece una conexión punto a punto entre (únicamente) dos dispositivos. Utiliza números de puerto para identificar aplicaciones específicas. UDP permite que muchos clientes/procesos envíen datos por el mismo socket y al igual que TCP utiliza números de puerto para identificar aplicaciones específicas.

---
1. La PDU de la capa de transporte es el segmento. Sin embargo, en algunos contextos suele utilizarse el término datagrama. Indique cuando.

---
6. Describa el saludo de tres vías de TCP. ¿UDP tiene esta característica?

---
7. Investigue qué es el ISN (Initial Sequence Number). Relaciónelo con el saludo de tres vías.

---
8. Investigue qué es el MSS. ¿Cuándo y cómo se negocia?

---
9. Utilice el comando ss (reemplazo de netstat) para obtener la siguiente información de su PC:
    a. Para listar las comunicaciones TCP establecidas.
    b. Para listar las comunicaciones UDP establecidas.
    c. Obtener sólo los servicios TCP que están esperando comunicaciones
    d. Obtener sólo los servicios UDP que están esperando comunicaciones.
    e. Repetir los anteriores para visualizar el proceso del sistema asociado a la conexión.
    f. Obtenga la misma información planteada en los items anteriores usando el comando **netstat**.

---
10. ¿Qué sucede si llega un segmento TCP con el flag SYN activo a un host que no tiene ningún proceso esperando en el puerto destino de dicho segmento (es decir, el puerto destino no está en estado LISTEN)?
    a. Utilice hping3 para enviar paquetes TCP al puerto destino 22 de la máquina virtual con el flag SYN activado.
    b. Utilice hping3 para enviar paquetes TCP al puerto destino 40 de la máquina virtual con el flag SYN activado.
    c. ¿Qué diferencias nota en las respuestas obtenidas en los dos casos anteriores? ¿Puede explicar a qué se debe? (Ayuda: utilice el comando ss visto anteriormente).

---
11. ¿Qué sucede si llega un datagrama UDP a un host que no tiene ningún proceso esperando en el puerto destino de dicho datagrama (es decir, que dicho puerto no está en estado LISTEN)?
    a. Utilice hping3 para enviar datagramas UDP al puerto destino 5353 de la máquina virtual.
    b. Utilice hping3 para enviar datagramas UDP al puerto destino 40 de la máquina virtual.
    c. ¿Qué diferencias nota en las respuestas obtenidas en los dos casos anteriores? ¿Puede explicar a qué se debe? *(Ayuda: utilice el comando ss visto anteriormente)*.

---
12. Investigue los distintos tipos de estado que puede tener una conexión TCP. Ver **https://users.cs.northwestern.edu/~agupta/cs340/project2/TCPIP_State_Transition_Diagram.pdf**

---
13. Dada la siguiente salida del comando ss, responda:
    ![captura ejercicio 13](imgs/ejercicio13.png)
    a. ¿Cuántas conexiones hay establecidas?
    b. ¿Cuántos puertos hay abiertos a la espera de posibles nuevas conexiones?
    c. El cliente y el servidor de las comunicaciones HTTPS (puerto 443), ¿residen en la misma máquina?
    d. El cliente y el servidor de la comunicación SSH (puerto 22), ¿residen en la misma máquina?
    e. Liste los nombres de todos los procesos asociados con cada comunicación. Indique para cada uno si se trata de un proceso cliente o uno servidor.
    f. ¿Cuáles conexiones tuvieron el cierre iniciado por el host local y cuáles por el remoto?
    g. ¿Cuántas conexiones están aún pendientes por establecerse?

---
14. Dadas las salidas de los siguientes comandos ejecutados en el cliente y el servidor, responder:
    ![captura ejercicio 14](imgs/ejercicio14.png)
    a. ¿Qué segmentos llegaron y cuáles se están perdiendo en la red?
    b. ¿A qué protocolo de capa de aplicación y de transporte se está intentando conectar el cliente?
    c. ¿Qué flags tendría seteado el segmento perdido?

---
15. Use CORE para armar una topología como la siguiente, sobre la cual deberá realizar:
    a. En ambos equipos inspeccionar el estado de las conexiones y mantener abiertas ambas ventanas con el comando corriendo para poder visualizar los cambios a medida que se realiza el ejercicio. **Ayuda: watch -n1 ’ss -nat’**.
    b. En Servidor, utilice la herramienta ncat para levantar un servicio que escuche en el puerto 8001/TCP. Utilice la opción -k para que el servicio sea persistente. Verifique el estado de las conexiones.
    ![captura ejercicio 15](imgs/ejercicio15.png)
    c. Desde CLIENTE1 conectarse a dicho servicio utilizando también la herramienta ncat. Inspeccione el estado de las conexiones.
    d. Iniciar otra conexión desde CLIENTE1 de la misma manera que la anterior y verificar el estado de las conexiones. ¿De qué manera puede identificar cada conexión?
    e. En base a lo observado en el item anterior, ¿es posible iniciar más de una conexión desde el cliente al servidor en el mismo puerto destino? ¿Por qué? ¿Cómo se garantiza que los datos de una conexión no se mezclarán con los de la otra?
    f. Analice en el tráfico de red, los flags de los segmentos TCP que ocurren cuando:
        i. Cierra la última conexión establecida desde CLIENTE1. Evalúe los estados de las conexiones en ambos equipos.
        ii. Corta el servicio de ncat en el servidor (Ctrl+C). Evalúe los estados de las conexiones en ambos equipos.
        iii. Cierra la conexión en el cliente. Evalúe nuevamente los estados de las conexiones.

---

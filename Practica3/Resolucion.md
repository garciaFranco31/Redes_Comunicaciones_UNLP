# Práctica 3 - Capa de Aplicación DNS (Domain Name Server)

## Introducción

1. Investigue y describa cómo funciona el DNS. ¿Cuál es su objetivo?

DNS es un sistema distribuido de forma jerárquica,conformado por muchos servidores al rededor del mundo.
El objetivo principal del DNS es traducir nombres de dominio a direcciones IP, permitiendo lograr una abstracción de las direcciones de red utilizadas internamente por los protocolos.

Cuando un cliente DNS recibe un nombre de host, lo pasa a su servidor DNS, el cual es el encargado de devolverle al cliente DNS la dirección IP correspondiente al nombre del host solicitado. Una vez que el navegador, recibe la dirección IP del servidor DNS, puede iniciar una conexión TCP.

---
2. ¿Qué es un root server? ¿Qué es un generic top-level domain (gtld)?

**gTLD (Generic Top-Level Domain):** son aquellos que contienen dominios con propósitos particulares.

**Root Server:** son aquellos encargados de atender la raíz del servicio y se encargan de delegar cada una de las zonas generadas para los TLD (gTLD y ccTLD). Lqa delegación consiste en saber las direcciones IP correspondientes a los servidores que se encargan de resolver las zonas de manera autoritativa.

*Servidor autoritativo: tiene toda la información para una zona, por lo cual puede producir cambios sobre la misma y tiene siempre la última versión.*

---
3. ¿Qué es una respuesta del tipo autoritativa?

Una *respuesta autoritativa* se produce cuando un *servidor autoritativo* recibe una consulta de un nombre sobre el cual tiene autoridad y la responde desde su base de datos de nombres.

---
4. ¿Qué diferencia una consulta DNS recursiva de una iterativa?

**Consulta recursiva:** es aquella donde el servidor DNS debe ponerse en contacto con otros servidores DNS (los que sean necesarios para resolver la solicitud)

**Consulta iterativa:**es aquella en la que se espera que el servidor DNS responda con la información local que tiene almacenada de lo que conoce de la zona local o de la caché.

---
5. ¿Qué es el resolver?

El resolver es un agente encargado de resolver los nombres a solicitud del cliente, el cual generalmente no se implementa como un servicio activo, si no, más bien como un conjunto de rutinas encapsuladas en una biblioteca de funciones.

---
6. Describa para qué se utilizan los siguientes tipos de registros de DNS:
    a. **A (Address):** son registros que mapean de nombre de dominio a una dirección IP (IPv4). Pueden existir varios registros A con el mismo nombre:
    b. **MX (Mail Exchanger):** indican para un nombre de dominio, cuáles son los servidores de mail encargados de recibir los mensajes para ese dominio.
    c. **PTR (Pointer):** son registros que mapean direcciones IP a nombres de dominio (al contario que los registros A).
    d. **AAAA:** igual que el registro A, es decir, se encargan de mapear el nombre de dominio a una dirección IP, pero en este caso, en su versión v6 (IPv6)
    e. **SRV (Service):**un registro de servicio, especifica un servidor y un puerto para servicios específicos (ej. mensajería instantanea).
    f. **NS (Name Server):** indican los servidores de nombre autoritativos para una zona o sub-domunio.
    g. **CNAME (Canonical Name):** también conocidos como aliases, se encargan de mapear un nombre de dominio a otros nombres al nombre original (nombre canónico).
    h. **SOA (Start of Authority):**se crean por cada zona o sub-zona que brinda el servicio de DNS, en ellos se especifican los parámetros globales para todos los registros del dominio o zona.
    i. **TXT (Textual):** mapena un nombre de dominio a información extra asociada con el equipo que tiene dicho nombre.

---
7. En Internet, un dominio suele tener más de un servidor DNS, ¿por qué cree que esto es así?

Esto es para poder responder las solicitudes de la forma más rápida posible y no colapsar en caso de que le lleguen demasiadas solicitudes al mismo tiempo, ya que esto les permite redirigir el tráfico.

---
8. Cuando un dominio cuenta con más de un servidor, uno de ellos es el primario (o maestro) y todos los demás son secundarios (o esclavos). ¿Cuál es la razón de que sea así?

Esto es así debido a que hace que sea más fácil actualizar el servidor en caso de que se requiera porque solamente debemos actualizar el servidor primerio y luego los servidores secundarios obtendrán una copia de la base de datos del servidor primario.

---
9. Explique brevemente en qué consiste el mecanismo de transferencia de zona y cuál es su finalidad.

El mecanismo de transferencia de zona se da cuando un servidor primario fue actualizado y da la señal para que los servidores secundarios re-copian la base de datos desde el servidor maestro. Esta comunicación entre los servidores se realiza sobre TPC vía el mismo protolo DNS.

---
10. Imagine que usted es el administrador del dominio de DNS de la UNLP (unlp.edu.ar). A su vez, cada facultad de la UNLP cuenta con un administrador que gestiona su propio dominio (por ejemplo, en el caso de la Facultad de Informática se trata de info.unlp.edu.ar).
Suponga que se crea una nueva facultad, Facultad de Redes, cuyo dominio será redes.unlp.edu.ar, y el administrador le indica que quiere poder manejar su propio dominio. ¿Qué debe hacer usted para que el administrador de la Facultad de Redes pueda gestionar el dominio de forma independiente? (Pista: investigue en qué consiste la delegación de dominios). Indicar qué registros de DNS se deberían agregar.

## DNS

11. Responda y justifique los siguientes ejercicios.
    a. En la VM, utilice el comando dig para obtener la dirección IP del host www.redes.unlp.edu.ar y responda:
        i. ¿La solicitud fue recursiva? ¿Y la respuesta? ¿Cómo lo sabe?
        ii. ¿Puede indicar si se trata de una respuesta autoritativa? ¿Qué significa que lo sea?
        iii. ¿Cuál es la dirección IP del resolver utilizado? ¿Cómo lo sabe?
    
    ---
    b. ¿Cuáles son los servidores de correo del dominio redes.unlp.edu.ar? ¿Por qué hay más de uno y qué significan los números que aparecen entre MX y el nombre? Si se quiere enviar un correo destinado a redes.unlp.edu.ar, ¿a qué servidor se le entregará? ¿En qué situación se le entregará al otro?
    
    ---
    c. ¿Cuáles son los servidores de DNS del dominio redes.unlp.edu.ar?
    
    ---
    d. Repita la consulta anterior cuatro veces más. ¿Qué observa? ¿Puede explicar a qué se debe?
    
    ---
    e. Observe la información que obtuvo al consultar por los servidores de DNS del dominio. En base a la salida, ¿es posible indicar cuál de ellos es el primario?
    
    ---
    f. Consulte por el registro SOA del dominio y responda.
        i. ¿Puede ahora determinar cuál es el servidor de DNS primario?
        ii. ¿Cuál es el número de serie, qué convención sigue y en qué casos es importante actualizarlo?
        iii. ¿Qué valor tiene el segundo campo del registro? Investigue para qué se usa y cómo se interpreta el valor.
        iv. ¿Qué valor tiene el TTL de caché negativa y qué significa?
    
    ---
    g. Indique qué valor tiene el registro TXT para el nombre saludo.redes.unlp.edu.ar. Investigue para qué es usado este registro.
    
    ---
    h. Utilizando dig, solicite la transferencia de zona de redes.unlp.edu.ar, analice la salida y responda.
        i. ¿Qué significan los números que aparecen antes de la palabra IN? ¿Cuál es su finalidad?
        ii. ¿Cuántos registros NS observa? Compare la respuesta con los servidores de DNS del dominio redes.unlp.edu.ar que dio anteriormente. ¿Puede explicar a qué se debe la diferencia y qué significa?
    
    ---
    i. Consulte por el registro A de www.redes.unlp.edu.ar y luego por el registro A de www.practica.redes.unlp.edu.ar. Observe los TTL de ambos. Repita la operación y compare el valor de los TTL de cada uno respecto de la respuesta anterior. ¿Puede explicar qué está ocurriendo? (Pista: observar los flags será de ayuda).
    
    ---
    j. Consulte por el registro A de www.practica2.redes.unlp.edu.ar. ¿Obtuvo alguna respuesta? Investigue sobre los códigos de respuesta de DNS ¿Para qué son utilizados los mensajes NXDOMAIN y NOERROR?


---
12. Investigue los comandos nslookup y host. ¿Para qué sirven? Intente con ambos comandos obtener:
    - Dirección IP de www.redes.unlp.edu.ar.
    - Servidores de correo del dominio redes.unlp.edu.ar.
    - Servidores de DNS del dominio redes.unlp.edu.ar.

---
13. ¿Qué función cumple en Linux/Unix el archivo /etc/hosts o en Windows el archivo \WINDOWS\system32\drivers\etc\hosts?

---
14. Abra el programa Wireshark para comenzar a capturar el tráfico de red en la interfaz con IP 172.28.0.1. Una vez abierto realice una consulta DNS con el comando dig para averiguar el registro MX de redes.unlp.edu.ar y luego, otra para averiguar los registros NS correspondientes al dominio redes.unlp.edu.ar. Analice la información proporcionada por dig y compárelo con la captura.

---
15. Dada la siguiente situación: “Una PC en una red determinada, con acceso a Internet, utiliza los servicios de DNS de un servidor de la red”. Analice:
    a. ¿Qué tipo de consultas (iterativas o recursivas) realiza la PC a su servidor de DNS?
    
    ---
    b. ¿Qué tipo de consultas (iterativas o recursivas) realiza el servidor de DNS para resolver requerimientos de usuario como el anterior? ¿A quién le realiza estas consultas?

---
16. Relacione DNS con HTTP. ¿Se puede navegar si no hay servicio de DNS?

---
17. Observar el siguiente gráfico y contestar:
<IMAGEN>
    a. Si la PC-A, que usa como servidor de DNS a "DNS Server", desea obtener la IP de www.unlp.edu.ar, cuáles serían, y en qué orden, los pasos que se ejecutarán para obtener la respuesta.
    
    ---
    b. ¿Dónde es recursiva la consulta? ¿Y dónde iterativa?

---
18. ¿A quién debería consultar para que la respuesta sobre www.google.com sea autoritativa?

---
19. ¿Qué sucede si al servidor elegido en el paso anterior se lo consulta por www.info.unlp.edu.ar? ¿Y si la consulta es al servidor 8.8.8.8?

## EJERCICIO DE PARCIAL

20. En base a la siguiente salida de dig, conteste las consignas. Justifique en todos los
casos.
```c

;; flags: qr rd ra; QUERY: 1, ANSWER: 2, AUTHORITY: 4, ADDITIONAL: 4
;; QUESTION SECTION:
;ejemplo.com. IN __
;; ANSWER SECTION:
ejemplo.com. 1634 IN __ 10 srv01.ejemplo.com. (1)
ejemplo.com. 1634 IN __ 5 srv00.ejemplo.com. (2)
;; AUTHORITY SECTION:
ejemplo.com. 92354 IN __ ss00.ejemplo.com.
ejemplo.com. 92354 IN __ ss02.ejemplo.com.
ejemplo.com. 92354 IN __ ss01.ejemplo.com.
ejemplo.com. 92354 IN __ ss03.ejemplo.com.
;; ADDITIONAL SECTION:
srv01.ejemplo.com. 272 IN __ 64.233.186.26
srv01.ejemplo.com. 240 IN __ 2800:3f0:4003:c00::1a
srv00.ejemplo.com. 272 IN __ 74.125.133.26
srv00.ejemplo.com. 240 IN __ 2a00:1450:400c:c07::1b

```
a. Complete las líneas donde aparece __ con el registro correcto.
b. ¿Es una respuesta autoritativa? En caso de no serlo, ¿a qué servidor le preguntaría para obtener una respuesta autoritativa?
c. ¿La consulta fue recursiva? ¿Y la respuesta?
d. ¿Qué representan los valores 10 y 5 en las líneas (1) y (2).

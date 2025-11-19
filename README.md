# Integración balanza TM-30AB / TM-A30 por TCP/IP (Java)

Este documento explica cómo programar PLUs (precio y nombre de producto) en una balanza **TM-30AB / TM-A30** usando **Java** y conexión **TCP/IP** (puerto 4001), a partir de ingeniería inversa hecha sobre las tramas que envía el software oficial.

> Importante: esto está basado en capturas reales de red de un modelo TM-A30.  
> Otros modelos o firmwares pueden variar.

---

## 1. Conexión de red

- La balanza se configura con una IP fija, por ejemplo: `192.168.0.150` la  configuracion es func+9002+enter e ir agregando la ip
- Puerto TCP usado para programación de PLUs: **4001**
- La PC/Java se conecta como **cliente TCP** a la balanza.
- Cada trama se envía como **texto ASCII** y conviene terminarla con `CRLF` (`\r\n`).

Ejemplo simple en Java:

```java
Socket socket = new Socket();
socket.connect(new InetSocketAddress("192.168.0.150", 4001), 5000);
OutputStream out = socket.getOutputStream();
String trama = "..."; // trama completa
out.write((trama + "\r\n").getBytes());
out.flush();
socket.close();

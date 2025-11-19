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
```

## 2. Ejemplo de trama completa de PLU

Ejemplo real de trama capturada para el PLU 17, precio 5.000 (ejemplo) y nombre Banana:
```!0V0017A4525925000500000000000000000000000000000000000000000000000000000000000000B066097110097110097000C000D000E```
La estructura general es:
```!0V[PLU4][Axxxxxxx][PRECIO5][RELLENO]B[ASCII_NOMBRE]000C000D000E```

## 3. Desglose campo por campo
```!0V0017A4525925000500000..............B066097110097110097000C000D000E```
## 3.1. Comando y PLU

```!0V```
Comando de programación de PLU (es lo que envía el software oficial).

```0017```
Número de PLU (4 dígitos, con ceros a la izquierda).
Valores típicos: ```0001–9999```.



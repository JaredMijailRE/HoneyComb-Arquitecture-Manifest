# RPC (Remote Procedure Call)

**Remote Procedure Call (RPC)** es un protocolo de comunicación que permite a un programa ejecutar funciones o procedimientos en otro sistema remoto como si fueran locales. Es comúnmente usado en arquitecturas de microservicios por su eficiencia y bajo overhead.

* Es un protocolo de **alto nivel**, ubicado generalmente entre las capas 5 y 7 del modelo OSI (sesión, presentación y aplicación).
* Se basa en el modelo **cliente-servidor**:

  * El cliente realiza la solicitud del procedimiento.
  * El servidor ofrece y ejecuta dicho procedimiento.

---

# gRPC

**gRPC** es una implementación moderna y de alto rendimiento de RPC desarrollada por Google. Es **multiplataforma**, **multilenguaje** y más eficiente que APIs tradicionales como REST.

## Características clave

* Utiliza **HTTP/2**, lo que permite:

  * Multiplexación de mensajes.
  * Compresión de cabeceras.
  * Conexiones persistentes y más rápidas.
* Usa por defecto **Protocol Buffers (Protobuf)** para serializar datos:

  * Más eficiente y compacto que JSON o XML.
  * Definiciones se escriben en archivos `.proto`.

### Ejemplo de Protobuf:

```proto
syntax = "proto3";

message Person {
  string name = 1;
  int32 id = 2;
  bool has_ponycopter = 3;
}
```

> Estas definiciones generan automáticamente el código cliente y servidor en múltiples lenguajes (Go, Java, Python, etc.).

---

## Tipos de métodos gRPC

gRPC define 4 tipos de comunicación entre cliente y servidor:

* **Unary RPC**:
  Cliente envía una sola solicitud y recibe una única respuesta.

* **Server Streaming RPC**:
  Cliente envía una solicitud y el servidor responde con un flujo de mensajes.

* **Client Streaming RPC**:
  Cliente envía una secuencia de mensajes y recibe una única respuesta del servidor.

* **Bidirectional Streaming RPC**:
  Cliente y servidor intercambian flujos de mensajes de forma simultánea e independiente.

---

## Consideraciones adicionales

* **Soporta comunicación sincrónica y asincrónica**, según el lenguaje y framework utilizado.
* Se pueden configurar **timeouts** para evitar bloqueos prolongados.
* La comunicación puede ser **cancelada** por cualquiera de las partes en cualquier momento.
* El **canal gRPC** representa una conexión lógica y segura entre cliente y servidor.
* Es compatible con **TLS/SSL** para conexiones seguras.
* Soporta **interceptores** para autenticación, logging, retries, etc.

---

### Recursos oficiales

* Introducción: [https://grpc.io/docs/what-is-grpc/introduction/](https://grpc.io/docs/what-is-grpc/introduction/)
* Conceptos clave: [https://grpc.io/docs/what-is-grpc/core-concepts/](https://grpc.io/docs/what-is-grpc/core-concepts/)

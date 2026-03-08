# Laboratorio 01 — Explorar TLS

En este laboratorio comenzaremos a observar cómo funciona una conexión TLS real.
El objetivo no es todavía crear certificados, sino **analizar qué ocurre cuando un cliente se conecta a un servicio seguro**.

TLS es el protocolo que protege la mayoría de las comunicaciones seguras en Internet, incluyendo:

* HTTPS
* SMTPS / STARTTLS
* IMAPS
* APIs seguras
* servicios internos entre aplicaciones.

Antes de aprender a generar certificados o crear una PKI, es importante comprender **qué ocurre durante una conexión TLS y cómo se presentan los certificados al cliente**.

---

## Qué aprenderás en este laboratorio

Durante este laboratorio se explorarán los elementos básicos de una conexión TLS.

Aprenderás a:

* establecer una conexión TLS con un servidor
* observar el proceso de handshake TLS
* extraer el certificado presentado por un servidor
* analizar la estructura de un certificado
* comprender cómo se construye la cadena de confianza.

Este laboratorio introduce herramientas que se utilizarán repetidamente durante el resto del curso.

---

## Herramientas utilizadas

Las principales herramientas que utilizaremos en este laboratorio son:

* **openssl s_client**
  para establecer conexiones TLS manualmente.

* **openssl x509**
  para analizar certificados.

Estas herramientas permiten inspeccionar con detalle cómo funciona TLS y cómo se presentan los certificados durante una conexión.

---

## Laboratorios incluidos

Este laboratorio contiene los siguientes ejercicios:

```id="8a0ajh"
01-conexion-tls.md
02-extraer-certificado.md
03-analizar-certificado.md
04-cadena-confianza.md
```

Cada ejercicio añade una nueva capa de comprensión sobre el funcionamiento de TLS.

---

## Qué deberías comprender al terminar

Al finalizar este laboratorio deberías ser capaz de:

* observar una conexión TLS desde la línea de comandos
* identificar el certificado que presenta un servidor
* analizar la información contenida en un certificado X.509
* comprender cómo los clientes validan certificados utilizando una cadena de confianza.

Este conocimiento servirá como base para los siguientes laboratorios, donde comenzaremos a trabajar con claves criptográficas y certificados generados por nosotros mismos.

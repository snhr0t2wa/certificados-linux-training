# Laboratorio 03 — Certificados X.509

En este laboratorio comenzaremos a trabajar directamente con **certificados X.509**, el formato utilizado en la mayoría de infraestructuras TLS.

Los certificados permiten asociar una **clave pública** con una **identidad**, y son el elemento central que permite establecer comunicaciones seguras en Internet.

Durante este laboratorio generaremos certificados simples, analizaremos sus extensiones y aprenderemos cómo verificar su validez.

---

## Qué aprenderás en este laboratorio

En este laboratorio aprenderás a:

* crear un certificado autofirmado
* examinar la estructura interna de un certificado X.509
* comprender el papel de las extensiones de certificado
* validar certificados utilizando herramientas de línea de comandos.

Este laboratorio marca el paso desde **observar TLS** a **crear certificados reales**.

---

## Conceptos clave

Un certificado X.509 contiene información fundamental para establecer una conexión segura.

Entre los elementos más importantes se encuentran:

* **Subject**
  identidad a la que pertenece el certificado.

* **Issuer**
  entidad que ha firmado el certificado.

* **Clave pública**
  utilizada durante el handshake TLS.

* **Periodo de validez**
  fechas entre las que el certificado es válido.

* **Extensiones**
  restricciones y usos permitidos para el certificado.

Comprender estos elementos es esencial para poder administrar certificados correctamente.

---

## Herramientas utilizadas

Durante este laboratorio utilizaremos principalmente:

* **openssl req**
  para generar certificados.

* **openssl x509**
  para analizar certificados.

* **openssl verify**
  para comprobar la validez de certificados.

Estas herramientas permiten trabajar con certificados X.509 directamente desde la línea de comandos.

---

## Laboratorios incluidos

Este laboratorio contiene los siguientes ejercicios:

```id="k4n9c1"
01-crear-certificado-selfsigned.md
02-extensiones-certificado.md
03-validar-certificados.md
```

Cada ejercicio introduce nuevas características de los certificados y su funcionamiento dentro de TLS.

---

## Qué deberías comprender al terminar

Al finalizar este laboratorio deberías ser capaz de:

* generar un certificado autofirmado
* interpretar la información contenida en un certificado X.509
* comprender el papel de las extensiones en los certificados
* validar certificados utilizando herramientas estándar.

Este conocimiento será fundamental para el siguiente laboratorio, donde construiremos una **infraestructura de certificación (PKI)** completa.

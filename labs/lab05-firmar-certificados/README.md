# Laboratorio 05 — Firmar certificados

En los laboratorios anteriores hemos creado una **infraestructura de certificación (PKI)** compuesta por una **Root CA** y una **Intermediate CA**.

A partir de este punto comenzaremos a utilizar esa infraestructura para emitir certificados reales que puedan ser utilizados por servicios.

El proceso típico para emitir un certificado consiste en:

1. generar una clave privada
2. crear una **CSR (Certificate Signing Request)**
3. firmar esa CSR mediante una autoridad certificadora.

Este flujo es el que utilizan la mayoría de servicios cuando solicitan certificados a una CA interna o externa.

---

## Qué aprenderás en este laboratorio

Durante este laboratorio aprenderás a:

* crear una **CSR**
* firmar certificados utilizando una CA
* construir y verificar la cadena de certificados.

Este laboratorio conecta directamente la infraestructura PKI creada anteriormente con su uso práctico en servicios.

---

## Conceptos clave

El proceso de emisión de certificados suele implicar tres elementos principales.

**Clave privada**

Se genera en el servidor que utilizará el certificado y nunca debe salir de él.

**CSR (Certificate Signing Request)**

Contiene:

* la clave pública
* la identidad del servicio
* información adicional para la emisión del certificado.

La CSR se envía a la autoridad certificadora.

**Certificado firmado**

La CA firma la CSR y genera un certificado que vincula la identidad con la clave pública.

---

## Herramientas utilizadas

Durante este laboratorio utilizaremos principalmente:

* **openssl req**
  para generar solicitudes de certificado.

* **openssl x509**
  para firmar certificados.

* **openssl verify**
  para validar cadenas de certificados.

Estas herramientas permiten reproducir el proceso completo de emisión de certificados dentro de una PKI.

---

## Laboratorios incluidos

Este laboratorio contiene los siguientes ejercicios:

```id="m4d1y6"
01-crear-csr.md
02-firmar-certificado.md
03-verificar-cadena.md
```

Cada ejercicio cubre una fase del proceso de emisión de certificados.

---

## Qué deberías comprender al terminar

Al finalizar este laboratorio deberías ser capaz de:

* generar solicitudes de certificado
* firmar certificados utilizando una CA
* verificar correctamente la cadena de certificados emitidos.

Este conocimiento permitirá en los siguientes laboratorios utilizar certificados en **servicios reales como servidores web o sistemas de correo**.

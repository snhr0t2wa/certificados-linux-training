# Laboratorio 04 — Crear una Autoridad Certificadora

En los laboratorios anteriores hemos trabajado con certificados individuales y certificados autofirmados.
Sin embargo, en entornos reales los certificados se emiten mediante una **infraestructura de clave pública (PKI)**.

Una PKI se basa en **Autoridades Certificadoras (CA)** que emiten y firman certificados para servidores, usuarios o servicios.

En este laboratorio construiremos una **PKI básica**, creando:

* una **Root CA**
* una **Intermediate CA**

Este modelo es el que utilizan la mayoría de infraestructuras de certificación modernas.

---

## Qué aprenderás en este laboratorio

Durante este laboratorio aprenderás a:

* crear la estructura de una PKI
* generar una autoridad certificadora raíz
* crear una autoridad certificadora intermedia
* comprender por qué se utilizan CA intermedias.

Este laboratorio introduce uno de los conceptos fundamentales de la gestión de certificados en entornos empresariales.

---

## Conceptos clave

Una **Autoridad Certificadora (CA)** es una entidad que emite certificados firmando digitalmente solicitudes de certificado.

En una PKI típica existen distintos niveles de autoridades.

**Root CA**

* autoridad certificadora principal
* firma a las CA intermedias
* se mantiene extremadamente protegida.

**Intermediate CA**

* utilizada para emitir certificados a servidores o usuarios
* permite limitar riesgos si una CA se compromete.

Este modelo permite separar responsabilidades y mejorar la seguridad de la infraestructura.

---

## Herramientas utilizadas

Durante este laboratorio utilizaremos principalmente:

* **openssl genrsa / genpkey**
  para generar claves privadas.

* **openssl req**
  para generar certificados.

* **openssl x509**
  para firmar certificados.

Estas herramientas permiten construir una PKI funcional desde la línea de comandos.

---

## Laboratorios incluidos

Este laboratorio contiene los siguientes ejercicios:

```id="s6v2k8"
01-estructura-pki.md
02-crear-root-ca.md
03-crear-intermediate-ca.md
04-ca-interna-vs-externa.md
```

Cada ejercicio introduce una parte de la infraestructura PKI.

---

## Qué deberías comprender al terminar

Al finalizar este laboratorio deberías ser capaz de:

* crear la estructura básica de una PKI
* generar una autoridad certificadora raíz
* crear una autoridad certificadora intermedia
* comprender cómo se construye una jerarquía de certificados.

En el siguiente laboratorio utilizaremos esta PKI para **firmar certificados de servicios reales**.

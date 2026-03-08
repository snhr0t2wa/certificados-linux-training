# Laboratorio 09 — Validación de certificados

Hasta ahora hemos trabajado con certificados y hemos configurado servicios TLS que utilizan esos certificados.
Sin embargo, en una infraestructura real también es necesario **gestionar la revocación de certificados**.

Un certificado puede dejar de ser válido antes de su fecha de expiración por diversos motivos, por ejemplo:

* compromiso de la clave privada
* retirada de un servicio
* error en la emisión del certificado.

Para gestionar estas situaciones las infraestructuras PKI utilizan mecanismos de validación adicionales.

---

## Qué aprenderás en este laboratorio

Durante este laboratorio aprenderás a:

* crear una **CRL (Certificate Revocation List)**
* revocar certificados emitidos por una CA
* validar certificados contra una CRL
* comprender el funcionamiento básico de **OCSP**.

Estos mecanismos permiten a los clientes comprobar si un certificado sigue siendo válido.

---

## Conceptos clave

Existen dos mecanismos principales para comprobar si un certificado ha sido revocado.

**CRL (Certificate Revocation List)**

Es una lista publicada por la autoridad certificadora que contiene los certificados revocados.

Los clientes descargan esta lista y verifican si el certificado aparece en ella.

---

**OCSP (Online Certificate Status Protocol)**

Permite consultar en tiempo real si un certificado es válido.

En lugar de descargar una lista completa, el cliente realiza una consulta directa a un servidor OCSP.

---

## Herramientas utilizadas

Durante este laboratorio utilizaremos principalmente:

* **openssl ca**
* **openssl verify**
* **openssl ocsp**

Estas herramientas permiten gestionar la revocación y verificar el estado de certificados dentro de una PKI.

---

## Laboratorios incluidos

Este laboratorio contiene los siguientes ejercicios:

```id="v3n5s7"
01-crear-crl.md
02-validar-crl.md
03-ocsp-check.md
```

Cada ejercicio explora un mecanismo distinto de validación de certificados.

---

## Qué deberías comprender al terminar

Al finalizar este laboratorio deberías ser capaz de:

* revocar certificados emitidos por una CA
* generar listas de revocación
* verificar certificados utilizando una CRL
* comprender cómo funciona la validación OCSP.

Estos conceptos son esenciales para mantener la seguridad de una infraestructura PKI en entornos reales.

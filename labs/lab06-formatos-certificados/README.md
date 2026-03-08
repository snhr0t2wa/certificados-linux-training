# Laboratorio 06 — Formatos de certificados

Los certificados digitales y las claves criptográficas pueden almacenarse en distintos **formatos**.
Cada formato está diseñado para usos concretos y es habitual tener que convertir certificados entre formatos diferentes.

Por ejemplo:

* servidores web suelen utilizar **PEM**
* aplicaciones Java utilizan **JKS**
* navegadores y aplicaciones empresariales suelen usar **PKCS#12**

En este laboratorio aprenderemos a trabajar con distintos formatos y a convertir certificados entre ellos.

---

## Qué aprenderás en este laboratorio

Durante este laboratorio aprenderás a:

* identificar distintos formatos de certificados
* convertir certificados entre **PEM y DER**
* crear archivos **PKCS#12**
* importar certificados en **keystores Java**.

Estas tareas son habituales en la administración de sistemas donde múltiples aplicaciones utilizan certificados.

---

## Conceptos clave

Los certificados pueden almacenarse en distintos formatos.

**PEM**

* formato basado en texto
* ampliamente utilizado en Linux
* contiene bloques codificados en Base64.

Ejemplo típico:

```id="k3m2r9"
-----BEGIN CERTIFICATE-----
...
-----END CERTIFICATE-----
```

---

**DER**

* formato binario
* representa la misma información que PEM
* utilizado en algunos sistemas o dispositivos.

---

**PKCS#12**

* contenedor que puede incluir

  * certificado
  * clave privada
  * cadena de certificados.

Se utiliza frecuentemente para transportar certificados entre sistemas.

---

**JKS**

* formato utilizado por aplicaciones Java
* gestionado mediante la herramienta `keytool`.

---

## Herramientas utilizadas

Durante este laboratorio utilizaremos principalmente:

* **openssl**
  para convertir certificados entre formatos.

* **keytool**
  para gestionar keystores Java.

* **certtool (GnuTLS)**
  para generar claves y certificados en entornos que utilizan la biblioteca GnuTLS.

Estas herramientas permiten adaptar certificados a diferentes aplicaciones y plataformas.

---

## Laboratorios incluidos

Este laboratorio contiene los siguientes ejercicios:

```id="y9k5v1"
01-convertir-pem-der.md
02-crear-pkcs12.md
03-usar-keytool.md
04-certtool-gnutls.md
```

Cada ejercicio explora un formato o conversión distinta.

---

## Qué deberías comprender al terminar

Al finalizar este laboratorio deberías ser capaz de:

* identificar el formato de un certificado
* convertir certificados entre formatos habituales
* crear archivos PKCS#12
* gestionar certificados en entornos Java.

Este conocimiento será importante en el siguiente laboratorio, donde veremos cómo se utilizan estos certificados dentro de **keystores y truststores utilizados por aplicaciones y sistemas**.

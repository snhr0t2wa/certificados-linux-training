# Laboratorio 02 — Claves criptográficas

Antes de trabajar con certificados es necesario comprender cómo funcionan las **claves criptográficas** que los sustentan.

Los certificados X.509 contienen **claves públicas**, mientras que los servidores mantienen protegidas las **claves privadas** asociadas.
Estas claves permiten establecer conexiones seguras y autenticar la identidad de los servicios.

En este laboratorio generaremos distintos tipos de claves y aprenderemos a inspeccionarlas utilizando herramientas estándar de Linux.

---

## Qué aprenderás en este laboratorio

En este laboratorio aprenderás a:

* generar claves RSA
* generar claves ECDSA
* inspeccionar claves privadas y públicas
* comprender las diferencias entre algoritmos criptográficos.

Este conocimiento es esencial para comprender cómo funcionan los certificados y el protocolo TLS.

---

## Conceptos clave

Las infraestructuras TLS se basan en **criptografía de clave pública**.

Cada identidad dispone de dos claves:

* **clave privada**
  se mantiene protegida en el servidor y nunca debe compartirse.

* **clave pública**
  se incluye dentro del certificado y puede distribuirse libremente.

Cuando un cliente se conecta a un servidor TLS, estas claves permiten:

* verificar la identidad del servidor
* establecer un canal cifrado.

---

## Algoritmos utilizados

Durante este laboratorio exploraremos dos algoritmos muy utilizados:

**RSA**

* ampliamente utilizado durante muchos años
* soportado por prácticamente todos los sistemas
* claves más grandes.

**ECDSA**

* basado en criptografía de curva elíptica
* claves más pequeñas
* mejor rendimiento en muchos casos.

Comprender ambos algoritmos es importante para elegir correctamente el tipo de clave en diferentes infraestructuras.

---

## Herramientas utilizadas

Durante los ejercicios utilizaremos principalmente:

* **openssl genpkey**
* **openssl rsa**
* **openssl ec**

Estas herramientas permiten generar e inspeccionar claves criptográficas desde la línea de comandos.

---

## Laboratorios incluidos

Este laboratorio contiene los siguientes ejercicios:

```id="m7f3q2"
01-generar-clave-rsa.md
02-generar-clave-ecdsa.md
03-inspeccionar-claves.md
```

Cada ejercicio explora distintos aspectos del uso de claves criptográficas.

---

## Qué deberías comprender al terminar

Al finalizar este laboratorio deberías ser capaz de:

* generar distintos tipos de claves criptográficas
* analizar su estructura
* comprender cómo se utilizan dentro de certificados X.509 y conexiones TLS.

Este conocimiento servirá como base para el siguiente laboratorio, donde comenzaremos a generar certificados reales.

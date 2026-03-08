# Laboratorio 00 — Entorno de trabajo

Este laboratorio prepara el entorno que se utilizará durante todo el curso.
El objetivo es disponer de un sistema Linux con las herramientas necesarias para trabajar con **certificados digitales, PKI y TLS**.

A lo largo del curso se utilizarán herramientas habituales en administración de sistemas para generar claves, emitir certificados, analizar conexiones TLS y gestionar almacenes de certificados.

El entorno utilizado es un **Codespace basado en Linux**, lo que permite ejecutar todos los ejercicios sin necesidad de infraestructura adicional.

---

## Qué aprenderás en este laboratorio

En este laboratorio se preparará el entorno que permitirá realizar los siguientes laboratorios del curso.

Durante los ejercicios:

* se verificará el entorno Linux disponible
* se instalarán herramientas criptográficas necesarias
* se comprobará que las utilidades funcionan correctamente
* se preparará un directorio de trabajo para los laboratorios.

Este paso es importante para garantizar que todos los alumnos trabajan en un entorno homogéneo.

---

## Herramientas que utilizaremos durante el curso

A lo largo de los laboratorios se utilizarán diversas herramientas relacionadas con criptografía y certificados.

Algunas de las más importantes son:

* **openssl**
  herramienta fundamental para generar claves, certificados y analizar TLS.

* **curl**
  cliente HTTP utilizado para probar servicios HTTPS.

* **keytool**
  herramienta utilizada en entornos Java para gestionar keystores.

* **certutil (NSS)**
  utilizada por algunas aplicaciones para gestionar almacenes de certificados.

* **pkcs11-tool / SoftHSM**
  herramientas utilizadas para trabajar con tokens PKCS#11.

* **docker**
  utilizado para ejecutar servicios de prueba como servidores HTTPS o SMTP.

Estas herramientas permiten reproducir escenarios reales de administración de certificados en sistemas Linux.

---

## Qué se hará en los ejercicios

Los ejercicios de este laboratorio se centran en:

1. preparar el entorno de trabajo
2. instalar las herramientas necesarias
3. comprobar que las utilidades funcionan correctamente.

Una vez completado este laboratorio, el sistema estará preparado para comenzar a trabajar con **TLS, certificados X.509 y PKI** en los siguientes laboratorios del curso.

---

## Laboratorios incluidos

Este laboratorio contiene los siguientes ejercicios:

```
01-preparar-entorno.md
02-instalar-herramientas.md
```

Cada ejercicio introduce pequeños pasos que permiten validar el entorno antes de avanzar al resto del curso.

---

Una vez completado este laboratorio, el entorno estará listo para comenzar a explorar cómo funcionan las conexiones TLS y cómo se utilizan los certificados digitales en sistemas Linux.

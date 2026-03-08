# Laboratorio 12 — Solaris

Hasta ahora hemos trabajado con certificados y herramientas típicas del ecosistema **Linux**.
Sin embargo, muchas infraestructuras empresariales también utilizan **sistemas Unix comerciales**, entre ellos **Solaris**.

Solaris dispone de su propio **framework criptográfico**, que permite gestionar claves, certificados y dispositivos criptográficos mediante una arquitectura basada en **PKCS#11**.

En este laboratorio exploraremos cómo funcionan estas herramientas y cómo se relacionan con las herramientas utilizadas en Linux.

Aunque no disponemos de un sistema Solaris real en el entorno del curso, utilizaremos herramientas compatibles con **PKCS#11** que permiten reproducir el mismo modelo de funcionamiento.

---

## Qué aprenderás en este laboratorio

Durante este laboratorio aprenderás a:

* comprender cómo Solaris gestiona claves y certificados
* trabajar con tokens PKCS#11
* almacenar claves y certificados en un keystore criptográfico
* comprender las equivalencias entre herramientas Linux y Solaris.

Este laboratorio permite trasladar los conocimientos adquiridos durante el curso a entornos Solaris.

---

## Conceptos clave

Solaris utiliza un **framework criptográfico integrado en el sistema operativo**.

Este framework permite:

* gestionar claves criptográficas
* trabajar con dispositivos criptográficos
* almacenar certificados en tokens PKCS#11.

En este modelo, las claves no siempre se almacenan en archivos como en Linux, sino dentro de **tokens criptográficos**.

---

## Herramientas utilizadas en Solaris

Las herramientas más habituales para trabajar con criptografía en Solaris incluyen:

**pktool**

Permite:

* generar claves
* listar claves
* importar y exportar certificados
* interactuar con tokens PKCS#11.

---

**keystore basado en PKCS#11**

Permite almacenar claves y certificados dentro de tokens criptográficos gestionados por el sistema.

---

**SMF (Service Management Facility)**

Sistema utilizado por Solaris para gestionar servicios, incluyendo aquellos que utilizan certificados.

---

## Simulación en el entorno del curso

En este laboratorio utilizaremos herramientas Linux que trabajan también con **PKCS#11**, permitiendo simular el funcionamiento del framework criptográfico de Solaris.

Para ello utilizaremos:

* **SoftHSM**
* **pkcs11-tool**

Estas herramientas permiten crear tokens criptográficos y gestionar claves y certificados de forma similar a como lo haría Solaris.

---

## Laboratorios incluidos

Este laboratorio contiene los siguientes ejercicios:

```id="s8v1y5"
01-pktool.md
02-keystore-solaris.md
03-smf-servicios-tls.md
```

Cada ejercicio explora un aspecto del modelo criptográfico utilizado en Solaris.

---

## Qué deberías comprender al terminar

Al finalizar este laboratorio deberías ser capaz de:

* comprender cómo Solaris gestiona claves y certificados
* trabajar con tokens PKCS#11
* relacionar las herramientas de Linux con sus equivalentes en Solaris.

Este conocimiento permite aplicar los principios aprendidos durante el curso tanto en **entornos Linux como en sistemas Solaris**.

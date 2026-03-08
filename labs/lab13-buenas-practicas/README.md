# Laboratorio 13 — Buenas prácticas y automatización avanzada

En los laboratorios anteriores has construido y utilizado una PKI completa, integrando certificados en servicios reales y automatizando parte de su gestión.

Este laboratorio recopila **buenas prácticas** y propone ejercicios para diseñar **estrategias de rotación automatizada** de certificados en entornos de producción.

---

## Qué aprenderás en este laboratorio

Durante este laboratorio aprenderás a:

* identificar buenas prácticas en el diseño y operación de una PKI
* definir políticas de validez y rotación de certificados
* proteger y almacenar de forma segura las claves privadas
* diseñar flujos de automatización que combinen scripts, cron, certmonger, Ansible y PKCS#11.

---

## Conceptos clave

En este laboratorio se refuerzan conceptos como:

* separación de roles entre Root CA e Intermediate CA
* elección de periodos de validez adecuados
* gestión de revocaciones y listas CRL
* rotación controlada de certificados en servicios sensibles.

---

## Herramientas utilizadas

En este laboratorio se hace referencia a herramientas ya utilizadas en el curso:

* **bash**
* **cron**
* **certmonger**
* **ansible**
* **pkcs11-tool / SoftHSM**

El enfoque es principalmente de diseño y consolidación de prácticas, más que de nuevos comandos.

---

## Laboratorios incluidos

Este laboratorio contiene los siguientes ejercicios:

```id="b1p9z3"
01-buenas-practicas-pki.md
02-estrategias-rotacion-automatizada.md
```

Cada ejercicio se centra en un aspecto distinto de la operación de certificados en producción.

---

## Qué deberías comprender al terminar

Al finalizar este laboratorio deberías ser capaz de:

* definir buenas prácticas para la gestión de certificados en tu organización
* diseñar una estrategia de rotación automatizada alineada con los requisitos de negocio y seguridad
* integrar los distintos componentes vistos en el curso en una visión coherente de operación de PKI.


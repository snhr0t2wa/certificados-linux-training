# Laboratorio 11 — Diagnóstico TLS

Incluso cuando los certificados y servicios están correctamente configurados, pueden aparecer problemas relacionados con **TLS y validación de certificados**.

Los errores TLS son frecuentes en entornos reales y pueden estar causados por distintos factores:

* certificados caducados
* cadenas de certificados incompletas
* nombres de host incorrectos
* autoridades certificadoras no confiables
* configuraciones incorrectas en los servicios.

En este laboratorio aprenderemos a **diagnosticar este tipo de problemas utilizando herramientas de línea de comandos**.

---

## Qué aprenderás en este laboratorio

Durante este laboratorio aprenderás a:

* analizar conexiones TLS con herramientas de diagnóstico
* inspeccionar el handshake TLS
* detectar problemas en la cadena de certificados
* identificar errores comunes en configuraciones TLS.

Estas habilidades son fundamentales para la resolución de incidencias en sistemas que utilizan certificados.

---

## Conceptos clave

Cuando un cliente establece una conexión TLS se realizan varias comprobaciones.

El cliente verifica que:

* el certificado no esté caducado
* el certificado haya sido emitido por una autoridad confiable
* el nombre del servidor coincida con el certificado
* la cadena de certificados sea válida.

Si alguna de estas comprobaciones falla, la conexión TLS se rechazará.

Comprender cómo funcionan estas validaciones permite identificar rápidamente el origen de los problemas.

---

## Herramientas utilizadas

Durante este laboratorio utilizaremos herramientas de diagnóstico ampliamente utilizadas en administración de sistemas.

**openssl**

Permite inspeccionar conexiones TLS y analizar certificados presentados por los servidores.

---

**curl**

Permite realizar peticiones HTTPS mostrando información detallada sobre la validación TLS.

---

Estas herramientas permiten observar cómo los clientes validan certificados durante una conexión segura.

---

## Laboratorios incluidos

Este laboratorio contiene los siguientes ejercicios:

```id="n7c5q3"
01-debug-openssl.md
02-diagnostico-curl.md
03-analizar-errores.md
```

Cada ejercicio reproduce situaciones reales de diagnóstico de problemas TLS.

---

## Qué deberías comprender al terminar

Al finalizar este laboratorio deberías ser capaz de:

* analizar conexiones TLS utilizando herramientas de diagnóstico
* identificar problemas en certificados y cadenas de confianza
* interpretar errores comunes relacionados con TLS.

Estas habilidades son esenciales para administrar y mantener infraestructuras seguras basadas en certificados digitales.

# Laboratorio 10 — Automatización

En infraestructuras reales la gestión manual de certificados no es viable.
Los certificados tienen una **fecha de expiración** y deben renovarse periódicamente para evitar interrupciones en los servicios.

Muchas incidencias en sistemas productivos se producen precisamente porque un certificado **expira sin que nadie lo detecte a tiempo**.

Por este motivo, las infraestructuras modernas implementan **mecanismos de automatización** que permiten:

* detectar certificados próximos a expirar
* renovar certificados automáticamente
* recargar servicios cuando se actualiza un certificado.

En este laboratorio exploraremos diferentes estrategias de automatización utilizadas en sistemas Linux.

---

## Qué aprenderás en este laboratorio

Durante este laboratorio aprenderás a:

* crear scripts para renovar certificados
* comprender cómo automatizar procesos de renovación
* utilizar herramientas de gestión automática de certificados
* programar tareas periódicas para mantener certificados actualizados.

Estas técnicas son esenciales para administrar certificados en infraestructuras con múltiples servicios.

---

## Conceptos clave

Los certificados suelen tener periodos de validez limitados, por ejemplo:

* 1 año
* 90 días
* periodos incluso más cortos en infraestructuras modernas.

Esto obliga a implementar procesos de renovación.

Existen diferentes enfoques para automatizar este proceso.

---

**Scripts de renovación**

Permiten renovar certificados mediante scripts que ejecutan comandos de generación de CSR y firma del certificado.

---

**Herramientas de gestión automática**

Algunas herramientas permiten gestionar certificados y renovar automáticamente cuando se aproxima su expiración.

---

**Tareas programadas**

Los sistemas Linux permiten ejecutar scripts de mantenimiento periódicamente mediante **cron**, lo que permite automatizar tareas de verificación y renovación.

---

## Herramientas utilizadas

Durante este laboratorio utilizaremos herramientas como:

* **bash**
* **cron**
* **certmonger**
* **sssd**
* **ansible**

Estas herramientas permiten implementar diferentes niveles de automatización en la gestión de certificados.

---

## Laboratorios incluidos

Este laboratorio contiene los siguientes ejercicios:

```id="y2m4g7"
01-renovacion-bash.md
02-certmonger.md
03-automatizar-cron.md
04-sssd-inscripcion-certificados.md
05-ansible-rotacion-certificados.md
```

Cada ejercicio explora una forma diferente de automatizar la gestión de certificados.

---

## Qué deberías comprender al terminar

Al finalizar este laboratorio deberías ser capaz de:

* automatizar la renovación de certificados mediante scripts
* comprender cómo funcionan las herramientas de gestión automática de certificados
* programar tareas periódicas para mantener certificados actualizados.

Este conocimiento es fundamental para evitar incidentes relacionados con certificados caducados en sistemas productivos.

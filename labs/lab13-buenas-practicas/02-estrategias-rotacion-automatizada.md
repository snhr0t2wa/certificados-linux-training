## Lab 02 — Estrategias de rotación automatizada de certificados

En este ejercicio diseñarás **estrategias de rotación de certificados** que combinen las distintas herramientas vistas en el curso.

El objetivo es que puedas definir flujos que reduzcan el riesgo de expiración inesperada de certificados en producción.

---

### 1. Identificar servicios y certificados críticos

Haz una lista de tipos de servicios que suelen depender de certificados:

* servidores web (HTTPS)
* servidores de correo (TLS)
* VPN (OpenVPN)
* servicios internos (APIs, autenticación, etc.).

Para cada tipo de servicio, anota:

* qué podría ocurrir si el certificado expira
* qué impacto tendría (bajo, medio, alto).

---

### 2. Monitorización de fechas de expiración

Diseña un enfoque para monitorizar la caducidad de certificados utilizando:

* scripts en **bash** que usen `openssl x509 -enddate`
* tareas programadas con **cron**
* opcionalmente, herramientas especializadas (como `certmonger`).

Define:

* con cuánta antelación quieres recibir alertas (por ejemplo, 30 días, 15 días)
* qué harás cuando se acerque la fecha de expiración (renovación manual, automatizada, etc.).

---

### 3. Renovación automática con certmonger

Basándote en el laboratorio de **certmonger**, describe:

* qué tipos de certificados son buenos candidatos para renovación automática
* qué precauciones tomarías (por ejemplo, pruebas en preproducción antes de activar la renovación en producción).

Esboza un flujo donde:

1. `certmonger` detecta que un certificado va a expirar.
2. Solicita un nuevo certificado a la CA.
3. Sustituye el certificado antiguo.
4. Notifica a los servicios implicados (o los recarga).

---

### 4. Distribución de certificados con Ansible

Integra el playbook del laboratorio de **Ansible** en un flujo de rotación:

1. Un certificado renovado se genera en un punto central.
2. Un job de Ansible despliega el certificado a todos los servidores que lo necesitan.
3. Los servicios se recargan de forma ordenada.

Define:

* qué variables o parámetros necesitaría tu playbook (rutas, servicios, grupos de hosts)
* cómo verificarías que todos los servidores han recibido el certificado correcto.

---

### 5. Rotación en entornos con PKCS#11

En entornos que utilizan **PKCS#11** (por ejemplo, con SoftHSM o dispositivos HSM reales), reflexiona sobre:

* qué cambia respecto a certificados almacenados en archivos
* cómo se gestionan operaciones de importación, actualización y borrado de claves/certificados
* qué herramientas intervendrían (`pkcs11-tool`, utilidades específicas del proveedor, etc.).

Esboza un flujo conceptual de rotación para un servicio que utiliza un keystore PKCS#11.

---

### 6. Definir una estrategia global

Usando todo lo anterior, redacta un documento breve que describa tu **estrategia de rotación automatizada**, incluyendo:

* alcance (qué servicios y certificados están cubiertos)
* herramientas utilizadas (bash, cron, certmonger, Ansible, PKCS#11, etc.)
* responsabilidades (quién supervisa, quién interviene si algo falla)
* pruebas y validaciones previas al despliegue en producción.

---

### 7. Conclusiones

Al finalizar este ejercicio deberías ser capaz de:

* diseñar una estrategia de rotación de certificados adaptada a tu entorno
* combinar herramientas de automatización para reducir el riesgo de expiraciones
* justificar las decisiones tomadas en función del impacto y la criticidad de los servicios.


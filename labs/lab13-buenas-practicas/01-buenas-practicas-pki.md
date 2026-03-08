## Lab 01 — Buenas prácticas en PKI

En este ejercicio revisarás un conjunto de **buenas prácticas** para diseñar, desplegar y operar una infraestructura de clave pública (PKI) en entornos Linux y Solaris.

El objetivo es que puedas utilizar la PKI del curso como base para definir políticas aplicables en tu entorno real.

---

### 1. Diseño de la jerarquía de certificación

Revisa la PKI que has construido en laboratorios anteriores (Root CA + Intermediate CA) y responde:

1. ¿Por qué es recomendable que la **Root CA**:
   * no firme certificados de servidores directamente
   * tenga una validez más larga que las CA intermedias
   * esté más protegida (offline, acceso restringido, etc.)?
2. ¿Cuántas **CA intermedias** tendría sentido tener en una organización típica y por qué (por ejemplo, separación por tipo de uso o zona de seguridad)?

Anota tus conclusiones en un documento de notas (por ejemplo `politica-pki.md`).

---

### 2. Políticas de validez de certificados

Define criterios para elegir el período de validez de:

* certificados de servidores internos
* certificados de servicios expuestos en Internet
* certificados de usuarios o dispositivos.

Ten en cuenta:

* riesgo de compromiso de claves
* frecuencia de cambios en la infraestructura
* requisitos de cumplimiento normativo (si aplican).

Propón al menos:

* un valor de referencia “conservador” (por ejemplo, 1 año)
* un valor alineado con prácticas modernas (por ejemplo, 90 días o menos).

---

### 3. Protección de claves privadas

Haz una lista de medidas para proteger claves privadas de alta sensibilidad, por ejemplo:

* claves de Root CA
* claves de CA intermedias
* claves de servicios críticos.

Incluye aspectos como:

* almacenamiento físico y lógico (HSM, tokens PKCS#11, cifrado en disco)
* control de acceso y registro de operaciones
* backups y procedimientos de recuperación.

Marca qué prácticas podrían implementarse con las herramientas vistas en el curso (`pkcs11-tool`, SoftHSM, etc.).

---

### 4. Gestión de revocaciones

Revisa el laboratorio de **CRL y OCSP** y define:

1. En qué casos revocarías un certificado (pérdida de dispositivo, sospecha de compromiso, cambio de rol, etc.).
2. Cómo te asegurarías de que:
   * los servicios publican CRL actualizadas
   * los clientes pueden acceder a OCSP/CRL
   * los servicios críticos fallan de forma segura si no pueden validar el estado de un certificado.

Esboza una política breve de revocación que incluya:

* quién puede solicitar revocaciones
* quién las aprueba
* tiempos máximos de reacción.

---

### 5. Segmentación y entornos

Define cómo aplicarías estas prácticas en distintos entornos:

* laboratorio / desarrollo
* preproducción
* producción.

Para cada entorno, indica:

* qué nivel de automatización aceptarías
* qué nivel de rigor en controles aplicarías (por ejemplo, requisitos de documentación, revisiones, cambios aprobados).

---

### 6. Conclusiones

Al finalizar este ejercicio deberías ser capaz de:

* describir buenas prácticas clave para el diseño y operación de una PKI
* adaptar la PKI del curso a políticas realistas de tu organización
* justificar decisiones de validez, revocación y protección de claves.


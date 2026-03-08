## Lab 04 — CA interna vs externa

En este ejercicio compararás el uso de **autoridades certificadoras internas** con el de **autoridades certificadoras externas** (públicas).

El objetivo es entender en qué casos se utiliza cada tipo de CA y cómo se integran con la PKI que has construido en los laboratorios anteriores.

---

### 1. Revisar la PKI interna del laboratorio

1. Sitúate en el directorio donde has creado la PKI del laboratorio.
2. Identifica:
   * la **Root CA** interna
   * la **Intermediate CA** interna
3. Revisa los certificados de ambas CA con:

```bash
openssl x509 -in ca/root-ca.crt -noout -subject -issuer -text | less
openssl x509 -in ca/intermediate-ca.crt -noout -subject -issuer -text | less
```

Observa:

* quién emite a quién (issuer)
* períodos de validez
* extensiones relevantes (CA:TRUE, pathlen, etc.).

---

### 2. Analizar un certificado emitido por una CA pública

1. Elige un sitio web público con HTTPS (por ejemplo, la web de tu organización o un sitio conocido).
2. Utiliza `openssl s_client` para obtener el certificado:

```bash
openssl s_client -connect ejemplo.com:443 -showcerts </dev/null 2>/dev/null | openssl x509 -noout -text | less
```

3. Identifica:
   * el **issuer** del certificado del servidor
   * la **cadena de certificados** hasta la Root CA pública
   * períodos de validez y extensiones clave.

Compara la estructura de este certificado con los certificados emitidos por tu PKI interna.

---

### 3. Comparar casos de uso: interna vs externa

Completa la siguiente tabla de forma conceptual (puedes hacerlo en un archivo de notas o en tu cuaderno):

* **CA interna**
  * casos de uso típicos (servicios internos, entornos de laboratorio, túneles VPN internos, etc.)
  * ventajas (control total, costes, flexibilidad)
  * riesgos (distribución de confianza, gestión de revocaciones, seguridad de la Root CA).

* **CA externa (pública)**
  * casos de uso típicos (servicios expuestos en Internet, APIs públicas, portales de clientes)
  * ventajas (confianza ya distribuida en navegadores y sistemas)
  * riesgos y limitaciones (coste, dependencia del proveedor, políticas estrictas).

---

### 4. Decidir qué CA usar en distintos escenarios

Para cada uno de estos escenarios, indica si utilizarías una **CA interna** o una **CA externa** y por qué:

1. Portal web interno solo accesible desde la red corporativa.
2. API pública consumida por clientes externos en Internet.
3. Servidor OpenVPN que solo usan empleados conectándose desde casa.
4. Sistema de monitorización interno con interfaz web accesible solo desde VPN.

Reflexiona especialmente sobre:

* quién debe confiar en el certificado
* cómo se distribuyen las autoridades de confianza
* políticas de seguridad de la organización.

---

### 5. Conclusiones

Al finalizar este ejercicio deberías ser capaz de:

* explicar las diferencias entre una CA interna y una CA externa
* identificar qué tipo de CA es más adecuada en distintos escenarios
* relacionar tu PKI de laboratorio con los modelos de certificación utilizados en entornos reales.


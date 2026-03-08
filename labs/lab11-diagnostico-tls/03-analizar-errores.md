# Laboratorio 11 — Diagnóstico TLS

## Analizar errores comunes en certificados TLS

Cuando un servicio TLS falla, el problema suele estar relacionado con errores de configuración en los certificados o en la cadena de confianza.

En este ejercicio reproduciremos y analizaremos algunos de los errores más habituales en despliegues TLS.

El objetivo es aprender a identificar rápidamente el origen del problema utilizando herramientas como `openssl` y `curl`.

---

### Paso 1 — Probar una conexión con un nombre de host incorrecto

Los certificados TLS están asociados a nombres de host concretos.

Intenta conectarte al servidor utilizando un nombre diferente al que aparece en el certificado.

```bash id="9k6t2m"
curl --cacert ~/pki-ca/ca.crt https://127.0.0.1:8443
```

La salida puede mostrar un error similar a:

```text id="x7c8y1"
SSL: certificate subject name does not match target host name
```

Esto ocurre porque el certificado fue emitido para un nombre específico (por ejemplo `web.test.local`) y no coincide con la dirección utilizada.

---

### Paso 2 — Verificar el nombre del certificado

Examina el certificado del servidor.

```bash id="1j2u7g"
openssl x509 -in ~/pki-labs/web-server/server.crt -text -noout
```

Busca la sección:

```text id="c8r6v0"
Subject:
```

y también posibles entradas como:

```text id="5p0d2y"
Subject Alternative Name
```

Estos campos indican los nombres de host para los que el certificado es válido.

---

### Paso 3 — Detectar un certificado caducado

Comprueba la fecha de expiración del certificado.

```bash id="g3n5z2"
openssl x509 -in ~/pki-labs/web-server/server.crt -noout -dates
```

La salida mostrará algo similar a:

```text id="d1p8k4"
notBefore=...
notAfter=...
```

Si la fecha actual supera el valor `notAfter`, el certificado ya no será considerado válido.

Esto provocará errores en los clientes TLS.

---

### Paso 4 — Analizar un problema de cadena incompleta

Utiliza OpenSSL para inspeccionar la cadena enviada por el servidor.

```bash id="v7h2m9"
openssl s_client -connect localhost:8443
```

Busca la sección:

```text id="b0g9s3"
Certificate chain
```

Si el servidor solo envía el certificado del servidor y no incluye el certificado intermedio, los clientes no podrán construir la cadena de confianza correctamente.

Esto provoca errores como:

```text id="f6j2c7"
unable to get local issuer certificate
```

---

### Paso 5 — Identificar el problema utilizando curl

Realiza la conexión en modo verbose.

```bash id="6p4q2z"
curl -v https://localhost:8443
```

Busca líneas relacionadas con errores TLS.

Por ejemplo:

```text id="q3k6h8"
SSL certificate problem
```

La salida de curl suele proporcionar pistas claras sobre el tipo de error:

* CA desconocida
* certificado caducado
* hostname incorrecto
* cadena incompleta.

---

En este ejercicio hemos reproducido varios errores habituales en despliegues TLS y hemos utilizado herramientas de diagnóstico para identificar la causa de cada problema.

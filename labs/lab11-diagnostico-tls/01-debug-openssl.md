# Laboratorio 11 — Diagnóstico TLS

## Depurar conexiones TLS con OpenSSL

Cuando un servicio TLS presenta problemas, una de las herramientas más utilizadas para diagnosticar la conexión es **OpenSSL**.

La utilidad `openssl s_client` permite:

* observar el handshake TLS
* analizar los certificados presentados por el servidor
* identificar errores de verificación
* comprobar los protocolos y cifrados utilizados.

En este ejercicio utilizaremos OpenSSL para analizar la conexión con el servidor HTTPS configurado en los laboratorios anteriores.

---

### Paso 1 — Conectarse al servidor TLS

Establece una conexión TLS con el servidor HTTPS del laboratorio.

```bash id="xg4q4c"
openssl s_client -connect localhost:8443
```

La salida mostrará el proceso completo de negociación TLS.

Busca líneas similares a:

```text id="j9m2ts"
CONNECTED(00000003)
```

Esto indica que se ha establecido la conexión TCP con el servidor.

Durante la conexión OpenSSL mostrará también el intercambio de certificados y parámetros criptográficos.

---

### Paso 2 — Identificar el certificado presentado por el servidor

Desplázate por la salida hasta encontrar líneas como:

```text id="v0o3k7"
subject=
issuer=
```

El **subject** corresponde a la identidad del servidor.

El **issuer** corresponde a la autoridad certificadora que firmó el certificado.

Esto permite verificar qué CA emitió el certificado presentado por el servidor.

---

### Paso 3 — Analizar la cadena de certificados

Busca en la salida la sección:

```text id="r5y0b2"
Certificate chain
```

En esta sección OpenSSL mostrará los certificados enviados por el servidor.

Normalmente aparecerán:

* certificado del servidor
* certificado de la autoridad intermedia.

Si falta alguno de estos certificados, los clientes pueden tener problemas para validar la conexión.

---

### Paso 4 — Verificar el resultado de la validación

Busca en la parte inferior de la salida una línea similar a:

```text id="f7u9kl"
Verify return code
```

Por ejemplo:

```text id="o3r4sv"
Verify return code: 20 (unable to get local issuer certificate)
```

Este error indica que el cliente no puede verificar la cadena de confianza.

Esto suele ocurrir cuando la autoridad certificadora no está presente en el almacén de certificados del sistema.

---

### Paso 5 — Probar la verificación utilizando la CA del laboratorio

Indica explícitamente el certificado raíz que debe utilizarse para validar la conexión.

```bash id="h8c0q2"
openssl s_client \
-connect localhost:8443 \
-CAfile ~/pki-ca/ca.crt
```

Vuelve a buscar la línea:

```text id="e6f3nz"
Verify return code
```

Si la cadena de certificados es correcta, el resultado debería ser:

```text id="1l8k0r"
Verify return code: 0 (ok)
```

Esto confirma que el certificado es válido cuando el cliente conoce la autoridad certificadora que lo firmó.

---

En este ejercicio hemos utilizado OpenSSL para inspeccionar una conexión TLS y diagnosticar problemas comunes relacionados con certificados y cadenas de confianza.

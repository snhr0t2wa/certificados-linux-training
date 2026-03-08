# Laboratorio 05 — Firmar certificados

## Firmar un certificado con la autoridad intermedia

En el ejercicio anterior generamos una **CSR (Certificate Signing Request)** para un servicio.
Ahora utilizaremos la **autoridad certificadora intermedia** para firmar esa solicitud y emitir el certificado del servidor.

Este es el proceso habitual mediante el cual los servicios obtienen certificados válidos dentro de una PKI.

---

### Paso 1 — Ir al directorio de la autoridad certificadora

Sitúate en el directorio donde creamos la PKI.

```bash
cd ~/pki-ca
```

Comprueba que el certificado de la autoridad intermedia existe.

```bash
ls intermediate
```

Deberías ver el archivo:

```
intermediate.crt
```

Este certificado identifica a la autoridad intermedia que emitirá los certificados de los servicios.

---

### Paso 2 — Firmar la solicitud de certificado del servidor

Utiliza la autoridad intermedia para firmar la CSR creada anteriormente.

```bash
openssl x509 -req \
-in ~/pki-labs/web-server/server.csr \
-CA intermediate/intermediate.crt \
-CAkey intermediate/private/intermediate.key \
-CAcreateserial \
-out ~/pki-labs/web-server/server.crt \
-days 365 \
-sha256
```

Durante la ejecución aparecerá un mensaje indicando que la solicitud ha sido firmada.

Se generará el archivo:

```
server.crt
```

Comprueba que el certificado se ha creado.

```bash
ls ~/pki-labs/web-server
```

Deberías ver algo similar a:

```
server.key
server.csr
server.crt
```

---

### Paso 3 — Analizar el certificado emitido

Visualiza el contenido del certificado generado.

```bash
openssl x509 -in ~/pki-labs/web-server/server.crt -text -noout
```

Busca las líneas:

```
Subject:
Issuer:
```

El **Subject** corresponderá al nombre del servidor solicitado en la CSR.

El **Issuer** corresponderá a la autoridad intermedia que firmó el certificado.

Esto muestra cómo un certificado contiene tanto la identidad del servidor como la entidad que lo ha firmado.

---

### Paso 4 — Verificar el certificado utilizando la CA raíz

Comprueba el certificado utilizando el certificado raíz como autoridad de confianza.

```bash
openssl verify \
-CAfile ca.crt \
~/pki-labs/web-server/server.crt
```

La verificación puede fallar si falta el certificado intermedio.

Esto ocurre porque para validar correctamente la cadena completa se necesitan todos los certificados implicados.

---

### Paso 5 — Verificar el certificado proporcionando la cadena completa

Incluye también el certificado de la autoridad intermedia durante la verificación.

```bash
openssl verify \
-CAfile ca.crt \
-untrusted intermediate/intermediate.crt \
~/pki-labs/web-server/server.crt
```

La salida debería indicar:

```
server.crt: OK
```

Esto demuestra que el certificado del servidor forma parte de una **cadena de confianza válida**:

```
Servidor
   ↓
CA Intermedia
   ↓
CA Raíz
```

En el siguiente ejercicio veremos cómo construir explícitamente la **cadena de certificados** que utilizan los servicios TLS.

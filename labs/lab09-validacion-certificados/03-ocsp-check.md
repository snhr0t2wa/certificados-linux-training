# Laboratorio 09 — Validación de certificados

## Verificar revocaciones mediante OCSP

Las **CRL** permiten publicar listas completas de certificados revocados, pero requieren descargar listas periódicamente.
Otra técnica utilizada en infraestructuras PKI es **OCSP (Online Certificate Status Protocol)**.

OCSP permite consultar **en tiempo real** si un certificado es válido, revocado o desconocido.

En este ejercicio utilizaremos OpenSSL para simular un **servidor OCSP** basado en nuestra autoridad certificadora.

---

### Paso 1 — Ir al directorio de la autoridad certificadora

Sitúate en el directorio de la PKI.

```bash id="s6o4uh"
cd ~/pki-ca
```

Comprueba que los archivos necesarios existen.

```bash id="y5kqru"
ls
```

Deberías ver archivos similares a:

```id="31o2v6"
ca.crt
private
index.txt
serial
crl
intermediate
```

El archivo `index.txt` contiene el estado de los certificados emitidos.

Este archivo es utilizado por OpenSSL para responder consultas OCSP.

---

### Paso 2 — Iniciar un servidor OCSP

Inicia un servidor OCSP utilizando OpenSSL.

```bash id="bhqv1q"
openssl ocsp \
-index index.txt \
-port 9999 \
-rsigner ca.crt \
-rkey private/ca.key \
-CA ca.crt \
-text
```

El proceso quedará ejecutándose en primer plano.

El servidor OCSP responderá consultas sobre certificados emitidos por la CA.

---

### Paso 3 — Abrir una nueva terminal para realizar la consulta

Abre una nueva terminal en el Codespace o en otra sesión.

Sitúate nuevamente en el directorio de la PKI.

```bash id="5op8s3"
cd ~/pki-ca
```

Ahora realizaremos una consulta OCSP sobre el certificado emitido anteriormente.

---

### Paso 4 — Consultar el estado del certificado

Ejecuta el siguiente comando para consultar el estado del certificado del servidor.

```bash id="f0t0vx"
openssl ocsp \
-issuer ca.crt \
-cert ~/pki-labs/web-server/server.crt \
-url http://localhost:9999 \
-CAfile ca.crt
```

La salida mostrará algo similar a:

```id="d9y5y3"
server.crt: revoked
```

o:

```id="k2z2pr"
server.crt: good
```

Dependiendo de si el certificado fue revocado en el ejercicio anterior.

---

### Paso 5 — Analizar la respuesta OCSP

Observa las líneas adicionales de la salida.

Entre ellas aparecerán campos similares a:

```id="h3m0l1"
This Update
Next Update
Cert Status
```

Estos campos indican:

* cuándo se generó la respuesta OCSP
* hasta cuándo es válida
* el estado del certificado.

Esto permite a los clientes validar certificados en tiempo real sin descargar listas completas de revocación.

---

En este ejercicio hemos utilizado OpenSSL para simular un servidor OCSP y consultar el estado de un certificado emitido por nuestra autoridad certificadora.

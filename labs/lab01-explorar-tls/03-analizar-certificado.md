# Laboratorio 01 — Explorar TLS

## Analizar la estructura de un certificado X.509

En el ejercicio anterior guardamos localmente el certificado de un servidor.
Ahora utilizaremos OpenSSL para inspeccionar su estructura interna y comprender la información que contiene.

---

### Paso 1 — Mostrar el contenido completo del certificado

Utiliza OpenSSL para decodificar el certificado y mostrar todos sus campos.

```bash
openssl x509 -in server.crt -text -noout
```

La salida mostrará una estructura bastante extensa.

Localiza las primeras secciones que describen el certificado:

```
Certificate:
    Data:
        Version
        Serial Number
        Signature Algorithm
```

Estas líneas forman parte de la cabecera del certificado y describen cómo ha sido firmado.

Esto confirma que un certificado es un documento estructurado que contiene múltiples campos definidos por el estándar **X.509**.

---

### Paso 2 — Identificar la identidad del certificado

Dentro de la salida anterior busca la línea:

```
Subject:
```

Para localizarla rápidamente puedes ejecutar:

```bash
openssl x509 -in server.crt -text -noout | grep Subject
```

El campo **Subject** describe la identidad para la que fue emitido el certificado.

En un certificado de servidor normalmente aparecerá el nombre del dominio o la organización a la que pertenece.

---

### Paso 3 — Identificar quién ha emitido el certificado

Busca ahora la línea:

```
Issuer:
```

Puedes filtrarla ejecutando:

```bash
openssl x509 -in server.crt -text -noout | grep Issuer
```

El **Issuer** identifica la autoridad certificadora que firmó el certificado.

Esto significa que el servidor no crea el certificado por sí mismo, sino que ha sido firmado por una **CA (Certificate Authority)** que respalda su autenticidad.

---

### Paso 4 — Comprobar el periodo de validez

Los certificados tienen una duración limitada.

Muestra únicamente las fechas del certificado.

```bash
openssl x509 -in server.crt -noout -dates
```

La salida será similar a:

```
notBefore=...
notAfter=...
```

Estas fechas indican:

* cuándo comienza a ser válido el certificado
* cuándo deja de ser válido.

Si la fecha actual queda fuera de este intervalo, los clientes considerarán el certificado inválido.

---

### Paso 5 — Identificar la clave pública del certificado

Muestra información sobre la clave pública contenida en el certificado.

```bash
openssl x509 -in server.crt -text -noout | grep "Public-Key"
```

La salida mostrará algo similar a:

```
Public-Key: (2048 bit)
```

Esto indica el tamaño de la clave pública utilizada por el servidor.

Durante el establecimiento de la conexión TLS esta clave pública se utiliza para establecer la comunicación segura entre cliente y servidor.

---

En este ejercicio hemos explorado la estructura básica de un certificado X.509 y algunos de sus campos más importantes.

En el siguiente ejercicio analizaremos cómo los clientes verifican la **cadena de confianza** que permite confiar en un certificado.

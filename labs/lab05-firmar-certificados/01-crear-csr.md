# Laboratorio 05 — Firmar certificados

## Crear una solicitud de certificado (CSR)

En los laboratorios anteriores construimos una **autoridad certificadora raíz** y una **autoridad intermedia**.
Ahora comenzaremos a emitir certificados para servicios.

El primer paso para obtener un certificado firmado es crear una **CSR (Certificate Signing Request)**.
Una CSR contiene la clave pública y la identidad que queremos incluir en el certificado.

---

### Paso 1 — Crear un directorio para el servicio que solicitará el certificado

Sitúate en el directorio de trabajo de los laboratorios.

```bash
cd ~/pki-labs
```

Crea un directorio para un servicio de prueba.

```bash
mkdir web-server
cd web-server
```

Comprueba que te encuentras dentro del nuevo directorio.

```bash
pwd
```

Este directorio contendrá la clave privada del servicio y la solicitud de certificado.

---

### Paso 2 — Generar la clave privada del servicio

Cada servicio que solicite un certificado debe generar su propia clave privada.

```bash
openssl genrsa -out server.key 2048
```

Comprueba que el archivo se ha creado.

```bash
ls -l server.key
```

Este archivo contiene la clave privada que utilizará el servidor para establecer conexiones TLS.

---

### Paso 3 — Crear la solicitud de certificado

Utiliza la clave generada para crear una CSR.

```bash
openssl req -new -key server.key -out server.csr
```

Durante el proceso aparecerán varias preguntas.

Cuando se solicite el **Common Name**, introduce el nombre del servicio:

```
web.test.local
```

Cuando el comando finalice se generará el archivo `server.csr`.

Comprueba que el archivo existe.

```bash
ls -l server.csr
```

La CSR contiene la información que será enviada a la autoridad certificadora.

---

### Paso 4 — Examinar el contenido de la CSR

Una CSR puede analizarse antes de enviarse a la autoridad certificadora.

```bash
openssl req -in server.csr -text -noout
```

Busca en la salida las secciones:

```
Subject:
Public-Key
```

Esto muestra:

* la identidad solicitada para el certificado
* la clave pública asociada.

La clave privada nunca se incluye en la CSR.

---

### Paso 5 — Extraer el nombre solicitado en la CSR

Podemos mostrar únicamente el sujeto solicitado en la CSR.

```bash
openssl req -in server.csr -noout -subject
```

La salida debería mostrar algo similar a:

```
CN=web.test.local
```

Esto confirma que la CSR solicita un certificado para ese nombre.

En el siguiente ejercicio utilizaremos la **autoridad intermedia creada anteriormente** para firmar esta solicitud y emitir el certificado del servicio.

# Laboratorio 05 — Firmar certificados

## Construir y verificar la cadena de certificados

En el ejercicio anterior firmamos un certificado de servidor utilizando la autoridad intermedia.
Ahora construiremos la **cadena completa de certificados** que normalmente se utiliza en servicios TLS.

Los servidores suelen enviar:

* el certificado del servidor
* el certificado de la CA intermedia

El cliente ya dispone normalmente del certificado raíz dentro de su almacén de confianza.

---

### Paso 1 — Revisar los certificados disponibles

Sitúate en el directorio donde se encuentra el certificado del servidor.

```bash
cd ~/pki-labs/web-server
```

Lista los archivos disponibles.

```bash
ls
```

Deberías ver algo similar a:

```
server.key
server.csr
server.crt
```

Ahora localiza también el certificado de la CA intermedia.

```bash
ls ~/pki-ca/intermediate
```

Entre los archivos debería aparecer:

```
intermediate.crt
```

Estos dos certificados formarán la cadena que enviará el servidor TLS.

---

### Paso 2 — Crear un archivo con la cadena de certificados

Crea un archivo que contenga el certificado del servidor seguido del certificado intermedio.

```bash
cat server.crt ~/pki-ca/intermediate/intermediate.crt > fullchain.crt
```

Comprueba que el archivo se ha creado.

```bash
ls -l fullchain.crt
```

Este archivo representa la cadena que normalmente configuran los servidores web.

---

### Paso 3 — Inspeccionar el contenido de la cadena

Visualiza el contenido del archivo generado.

```bash
cat fullchain.crt
```

Observa que el archivo contiene dos bloques:

```
-----BEGIN CERTIFICATE-----
...
-----END CERTIFICATE-----
```

El primer certificado corresponde al servidor y el segundo al certificado de la autoridad intermedia.

Los clientes utilizarán estos certificados para construir la cadena de confianza.

---

### Paso 4 — Verificar el certificado del servidor utilizando la cadena

Utiliza OpenSSL para verificar el certificado del servidor indicando la CA raíz y la cadena intermedia.

```bash
openssl verify \
-CAfile ~/pki-ca/ca.crt \
-untrusted ~/pki-ca/intermediate/intermediate.crt \
server.crt
```

La salida debería indicar:

```
server.crt: OK
```

Esto confirma que el certificado del servidor puede validarse correctamente utilizando la cadena completa.

---

### Paso 5 — Probar la verificación utilizando el archivo de cadena

Ahora intenta verificar el certificado utilizando el archivo `fullchain.crt`.

```bash
openssl verify \
-CAfile ~/pki-ca/ca.crt \
-untrusted fullchain.crt \
server.crt
```

El resultado debería ser nuevamente:

```
server.crt: OK
```

Esto demuestra que la verificación funciona siempre que el cliente disponga de:

* el certificado raíz
* los certificados intermedios necesarios para reconstruir la cadena.

---

En este ejercicio hemos construido explícitamente la cadena de certificados y hemos comprobado cómo se utiliza para validar certificados emitidos dentro de una PKI.

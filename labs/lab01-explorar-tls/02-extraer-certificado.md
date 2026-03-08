# Laboratorio 01 — Explorar TLS

## Extraer el certificado de un servidor

En el ejercicio anterior observamos que durante una conexión TLS el servidor envía uno o varios certificados.
En este ejercicio extraeremos uno de esos certificados y lo guardaremos en un archivo para poder analizarlo posteriormente.

---

### Paso 1 — Solicitar al servidor todos los certificados de la cadena

Realiza nuevamente una conexión TLS utilizando OpenSSL, pero solicitando que se muestren todos los certificados enviados por el servidor.

```bash
openssl s_client -connect google.com:443 -showcerts
```

La salida del comando será extensa.
Busca bloques que comiencen con:

```
-----BEGIN CERTIFICATE-----
```

y terminen con:

```
-----END CERTIFICATE-----
```

Normalmente aparecerán varios certificados consecutivos.
El primero suele corresponder al certificado del servidor.

---

### Paso 2 — Guardar el certificado del servidor en un archivo

Vamos a guardar el primer certificado en un archivo local para poder trabajar con él.

Crea un archivo llamado `server.crt`.

```bash
nano server.crt
```

Copia desde la salida anterior todo el bloque del certificado que comienza con:

```
-----BEGIN CERTIFICATE-----
```

y termina con:

```
-----END CERTIFICATE-----
```

Guarda el archivo y cierra el editor.

Comprueba que el archivo existe.

```bash
ls -l server.crt
```

Si el archivo se ha guardado correctamente verás algo similar a:

```
-rw-r--r-- 1 usuario usuario ...
```

Ahora disponemos de una copia local del certificado del servidor.

---

### Paso 3 — Verificar que el archivo contiene un certificado válido

Utiliza OpenSSL para comprobar que el archivo contiene un certificado válido.

```bash
openssl x509 -in server.crt -noout
```

Si el archivo es correcto el comando finalizará sin errores y no mostrará salida.

Esto confirma que OpenSSL reconoce el contenido como un certificado X.509 válido.

---

### Paso 4 — Mostrar información básica del certificado

Ahora muestra algunos campos importantes del certificado.

```bash
openssl x509 -in server.crt -noout -subject -issuer -dates
```

La salida mostrará algo similar a:

```
subject=...
issuer=...
notBefore=...
notAfter=...
```

Estos campos indican:

* la identidad del certificado
* la autoridad que lo ha emitido
* el periodo de validez del certificado.

Esto demuestra que un certificado contiene información sobre la identidad del servidor y la entidad que lo ha firmado.

---

### Paso 5 — Contar cuántos certificados envía el servidor

Vuelve a ejecutar la conexión TLS solicitando los certificados.

```bash
openssl s_client -connect google.com:443 -showcerts
```

Cuenta cuántos bloques de certificados aparecen en la salida.

Cada bloque está delimitado por:

```
-----BEGIN CERTIFICATE-----
```

Esto muestra que los servidores suelen enviar **varios certificados**, no solo uno.

La razón es que el cliente necesita construir una **cadena de confianza** que permita verificar el certificado del servidor.

---

En este ejercicio has extraído un certificado de un servidor real y lo has guardado para poder analizarlo.
En el siguiente ejercicio examinaremos la estructura interna del certificado y sus campos principales.

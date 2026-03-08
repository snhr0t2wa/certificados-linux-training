# Laboratorio 01 — Explorar TLS

## Explorar la cadena de confianza

En los ejercicios anteriores observamos que un servidor envía varios certificados durante el establecimiento de una conexión TLS.
Estos certificados forman una **cadena de confianza** que permite al cliente verificar que el certificado del servidor es válido.

En este ejercicio observaremos esa cadena y veremos cómo el sistema confía en determinadas autoridades certificadoras.

---

### Paso 1 — Mostrar nuevamente la cadena de certificados enviada por el servidor

Realiza una conexión TLS solicitando que se muestren todos los certificados.

```bash
openssl s_client -connect google.com:443 -showcerts
```

Busca en la salida la sección:

```
Certificate chain
```

Debajo aparecerán varios certificados numerados.

Cada entrada representa un certificado dentro de la cadena.

Observa que el primer certificado suele corresponder al servidor al que nos conectamos.

---

### Paso 2 — Identificar los certificados intermedios

En la misma salida observa los certificados que aparecen después del primero.

Cada uno estará delimitado por:

```
-----BEGIN CERTIFICATE-----
```

y

```
-----END CERTIFICATE-----
```

Estos certificados adicionales corresponden normalmente a **autoridades certificadoras intermedias**.

Los servidores suelen enviar estos certificados para ayudar al cliente a reconstruir la cadena de confianza.

---

### Paso 3 — Localizar el certificado raíz esperado

Dentro de la salida del comando observa los campos:

```
subject=
issuer=
```

En los certificados intermedios notarás que el **issuer** corresponde a otra autoridad certificadora.

Esto muestra cómo cada certificado está firmado por otro certificado superior dentro de la jerarquía.

El último certificado de la cadena suele corresponder a una **autoridad raíz** que ya está incluida en el sistema operativo o en el navegador.

---

### Paso 4 — Explorar el almacén de autoridades certificadoras del sistema

Los sistemas operativos incluyen un conjunto de certificados raíz en los que confían por defecto.

Lista algunos de los certificados instalados en el sistema.

```bash
ls /etc/ssl/certs | head
```

Aparecerán numerosos archivos que representan autoridades certificadoras de confianza.

Estos certificados raíz permiten al sistema validar certificados emitidos por múltiples servicios en Internet.

---

### Paso 5 — Intentar verificar el certificado del servidor manualmente

Utiliza OpenSSL para intentar verificar el certificado que guardaste anteriormente.

```bash
openssl verify server.crt
```

Es posible que aparezca un mensaje como:

```
unable to get local issuer certificate
```

Esto ocurre porque para verificar correctamente el certificado también sería necesario proporcionar los certificados intermedios correspondientes.

Este comportamiento demuestra que la confianza en TLS depende de una **cadena completa de certificación**, no únicamente del certificado del servidor.

---

En este laboratorio hemos observado cómo los servidores envían varios certificados y cómo los clientes utilizan una cadena de confianza basada en autoridades certificadoras conocidas para validar la identidad de los servidores.

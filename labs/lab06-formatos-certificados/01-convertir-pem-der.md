# Laboratorio 06 — Formatos de certificados

## Convertir certificados entre formatos

Los certificados y claves pueden almacenarse en distintos formatos.
Aunque el contenido criptográfico es el mismo, cada formato utiliza una codificación diferente.

En este ejercicio trabajaremos con los formatos más comunes:

* **PEM**
* **DER**

Muchos sistemas y aplicaciones requieren formatos específicos, por lo que es habitual convertir certificados entre ellos.

---

### Paso 1 — Verificar el formato del certificado actual

Sitúate en el directorio donde se encuentra el certificado del servidor.

```bash id="v4n4qg"
cd ~/pki-labs/web-server
```

Visualiza el contenido del certificado.

```bash id="afm2n0"
cat server.crt
```

Observa que el certificado comienza con:

```id="5wnj9s"
-----BEGIN CERTIFICATE-----
```

y termina con:

```id="tkmcmv"
-----END CERTIFICATE-----
```

Este formato se denomina **PEM** y utiliza codificación Base64 para representar los datos binarios del certificado.

---

### Paso 2 — Convertir el certificado de PEM a DER

El formato **DER** es una representación binaria del certificado.

Convierte el certificado actual a formato DER.

```bash id="o7s3g7"
openssl x509 \
-in server.crt \
-outform der \
-out server.der
```

Comprueba que el archivo se ha creado.

```bash id="9e22s8"
ls -l server.der
```

Este archivo contiene exactamente el mismo certificado, pero en formato binario.

---

### Paso 3 — Observar la diferencia entre PEM y DER

Visualiza el contenido del archivo DER.

```bash id="qz7aqh"
cat server.der
```

Notarás que aparecen caracteres binarios y la salida no es legible.

Esto ocurre porque DER no utiliza codificación Base64 como PEM.

Este formato se utiliza frecuentemente en sistemas que requieren representaciones binarias de certificados.

---

### Paso 4 — Convertir nuevamente el certificado de DER a PEM

Convierte el certificado DER de nuevo al formato PEM.

```bash id="h8twz3"
openssl x509 \
-in server.der \
-inform der \
-out server-from-der.crt
```

Comprueba que el archivo se ha generado.

```bash id="29z0fx"
ls -l server-from-der.crt
```

---

### Paso 5 — Verificar que ambos certificados son equivalentes

Compara la huella digital de los certificados para comprobar que contienen la misma información.

```bash id="7p4mq1"
openssl x509 -in server.crt -noout -fingerprint -sha256
```

y después:

```bash id="sls2nd"
openssl x509 -in server-from-der.crt -noout -fingerprint -sha256
```

Las huellas deberían ser idénticas.

Esto confirma que el certificado no ha cambiado; únicamente se ha convertido entre distintos formatos de almacenamiento.

---

En este ejercicio hemos visto cómo convertir certificados entre formatos PEM y DER, algo habitual cuando se trabaja con diferentes aplicaciones o plataformas.

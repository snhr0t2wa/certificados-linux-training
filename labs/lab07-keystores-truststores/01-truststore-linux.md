# Laboratorio 07 — Keystores y almacenes de confianza

## Explorar el almacén de certificados del sistema

Los sistemas Linux incluyen un **almacén de certificados de confianza** que contiene autoridades certificadoras utilizadas para validar conexiones TLS.

Cuando una aplicación establece una conexión HTTPS, normalmente utiliza este almacén para comprobar si el certificado del servidor ha sido emitido por una autoridad confiable.

En este ejercicio exploraremos el almacén de certificados del sistema.

---

### Paso 1 — Localizar el directorio de certificados del sistema

Muchos sistemas Linux almacenan certificados raíz en el directorio:

```bash
/etc/ssl/certs
```

Lista algunos de los archivos presentes en ese directorio.

```bash
ls /etc/ssl/certs | head
```

La salida mostrará diferentes archivos que corresponden a certificados de autoridades certificadoras reconocidas.

Estos certificados permiten al sistema confiar en múltiples servicios HTTPS en Internet.

---

### Paso 2 — Contar cuántos certificados contiene el sistema

Cuenta cuántos certificados están disponibles en el almacén del sistema.

```bash
ls /etc/ssl/certs | wc -l
```

El número puede variar dependiendo de la distribución Linux, pero normalmente habrá decenas o incluso cientos de certificados.

Cada uno corresponde a una autoridad certificadora reconocida.

---

### Paso 3 — Inspeccionar uno de los certificados del sistema

Selecciona uno de los certificados y examina su contenido.

Primero localiza un archivo `.pem` o `.crt`.

```bash
ls /etc/ssl/certs/*.pem | head -n 1
```

Copia el nombre del archivo que aparece y examínalo con OpenSSL.

```bash
openssl x509 -in /etc/ssl/certs/archivo.pem -text -noout
```

Reemplaza `archivo.pem` por el nombre real del certificado.

Observa campos como:

* Subject
* Issuer
* Validity

Estos certificados suelen pertenecer a autoridades certificadoras utilizadas en Internet.

---

### Paso 4 — Comprobar qué certificado utiliza una conexión HTTPS

Utiliza OpenSSL para conectarte a un servidor HTTPS y observar el certificado que presenta.

```bash
openssl s_client -connect google.com:443
```

Busca en la salida las líneas:

```
subject=
issuer=
```

El **issuer** debería corresponder a alguna autoridad certificadora que esté incluida en el almacén del sistema.

Esto es lo que permite que el sistema confíe automáticamente en el certificado del servidor.

---

### Paso 5 — Comparar con el certificado raíz creado en los laboratorios

Visualiza el certificado raíz que generamos en los laboratorios anteriores.

```bash
openssl x509 -in ~/pki-ca/ca.crt -text -noout
```

Observa el campo:

```
Subject
```

Este certificado no forma parte del almacén de confianza del sistema, por lo que los clientes no confiarán en certificados emitidos por esta CA a menos que se añada manualmente al almacén.

Esto explica por qué los navegadores y herramientas como `curl` suelen rechazar certificados emitidos por autoridades desconocidas.

---

En este ejercicio hemos explorado cómo los sistemas Linux almacenan autoridades certificadoras de confianza y cómo se utilizan para validar conexiones TLS.

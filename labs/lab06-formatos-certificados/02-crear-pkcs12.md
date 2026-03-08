# Laboratorio 06 — Formatos de certificados

## Crear un contenedor PKCS12

En muchos entornos los certificados y las claves privadas se distribuyen en un único archivo.
El formato **PKCS12** permite agrupar en un solo contenedor:

* certificado del servidor
* clave privada
* certificados intermedios

Este formato se utiliza habitualmente en:

* navegadores
* servidores web
* aplicaciones Java
* sistemas Windows.

---

### Paso 1 — Ir al directorio del certificado del servidor

Sitúate en el directorio donde se encuentran los archivos del servidor.

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
fullchain.crt
```

Estos archivos se utilizarán para construir el contenedor PKCS12.

---

### Paso 2 — Crear el archivo PKCS12

Utiliza OpenSSL para crear un archivo PKCS12 que contenga la clave privada y el certificado del servidor.

```bash
openssl pkcs12 -export \
-inkey server.key \
-in server.crt \
-certfile ~/pki-ca/intermediate/intermediate.crt \
-out server.p12
```

Durante la ejecución OpenSSL solicitará una contraseña para proteger el contenedor.

Introduce una contraseña sencilla para las pruebas del laboratorio.

Cuando el comando finalice se generará el archivo:

```
server.p12
```

Comprueba que el archivo existe.

```bash
ls -l server.p12
```

---

### Paso 3 — Inspeccionar el contenido del contenedor PKCS12

Podemos examinar el contenido del archivo PKCS12 utilizando OpenSSL.

```bash
openssl pkcs12 -info -in server.p12
```

Introduce la contraseña que definiste anteriormente.

En la salida aparecerán secciones que corresponden a:

* certificado del servidor
* certificado intermedio
* clave privada.

Esto confirma que el contenedor puede almacenar múltiples elementos relacionados con el certificado.

---

### Paso 4 — Extraer el certificado desde el contenedor PKCS12

Extrae únicamente el certificado del servidor desde el contenedor.

```bash
openssl pkcs12 -in server.p12 -clcerts -nokeys -out extracted-cert.crt
```

Introduce la contraseña cuando se solicite.

Comprueba que el archivo se ha creado.

```bash
ls -l extracted-cert.crt
```

Este archivo contiene el certificado almacenado dentro del contenedor.

---

### Paso 5 — Extraer la clave privada desde el contenedor

Ahora extrae la clave privada.

```bash
openssl pkcs12 -in server.p12 -nocerts -out extracted-key.key
```

Introduce la contraseña del contenedor cuando se solicite.

Comprueba que el archivo existe.

```bash
ls -l extracted-key.key
```

Este archivo contiene la clave privada originalmente utilizada para crear el contenedor.

---

En este ejercicio hemos creado un contenedor PKCS12 que agrupa certificado, clave privada y certificados intermedios, y hemos comprobado cómo extraer sus componentes.

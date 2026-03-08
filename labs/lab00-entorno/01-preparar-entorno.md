# Laboratorio 00 — Preparar entorno

## Preparar el entorno de trabajo

En este primer ejercicio prepararemos un espacio de trabajo donde realizaremos todos los laboratorios del curso y verificaremos que las herramientas básicas necesarias están disponibles en el sistema.

Todo el trabajo se realizará desde la terminal utilizando utilidades estándar de Linux.

---

### Paso 1 — Crear el directorio de trabajo

Crea un directorio donde almacenaremos todos los archivos generados durante los ejercicios del curso.

```bash
mkdir -p ~/pki-labs
cd ~/pki-labs
```

Comprueba que te encuentras dentro del directorio creado.

```bash
pwd
```

La salida debería mostrar algo similar a:

```
/home/usuario/pki-labs
```

Este directorio será el espacio de trabajo donde se generarán claves, certificados y otros archivos durante los laboratorios.

---

### Paso 2 — Crear una estructura básica para los ejercicios

Vamos a preparar algunos directorios que utilizaremos para organizar los archivos que generaremos durante los laboratorios.

```bash
mkdir certs
mkdir private
mkdir csr
mkdir output
```

Comprueba que los directorios se han creado correctamente.

```bash
ls
```

La salida debería mostrar algo similar a:

```
certs
csr
output
private
```

Esta estructura permitirá mantener separados distintos tipos de archivos durante el curso.

---

### Paso 3 — Verificar que OpenSSL está disponible

OpenSSL es la herramienta principal que utilizaremos para trabajar con criptografía y certificados.

Comprueba si está instalada en el sistema.

```bash
openssl version
```

La salida debería mostrar algo similar a:

```
OpenSSL 3.x.x
```

Esto confirma que el sistema dispone de las utilidades necesarias para trabajar con certificados X.509 y TLS.

---

### Paso 4 — Localizar el ejecutable de OpenSSL

Ahora identificaremos dónde se encuentra el binario de OpenSSL en el sistema.

```bash
which openssl
```

La salida suele ser algo parecido a:

```
/usr/bin/openssl
```

Esto indica que el sistema puede localizar correctamente la herramienta.

---

### Paso 5 — Explorar las utilidades disponibles en OpenSSL

OpenSSL incluye numerosas utilidades relacionadas con certificados y criptografía.

Muestra la ayuda general del comando:

```bash
openssl help
```

Observa la lista de comandos disponibles y localiza algunos que utilizaremos durante el curso, como:

```
x509
req
verify
pkcs12
genrsa
```

Estas utilidades permiten generar claves, crear certificados, analizar certificados existentes y verificar cadenas de confianza.

---

En este ejercicio hemos preparado el entorno básico de trabajo y verificado que OpenSSL está disponible en el sistema. En el siguiente ejercicio instalaremos y verificaremos otras herramientas que utilizaremos durante los laboratorios.

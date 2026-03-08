# Laboratorio 07 — Keystores y almacenes de confianza

## Crear y explorar una base de datos NSS

Algunos sistemas y aplicaciones no utilizan directamente el almacén de certificados del sistema o los keystores Java.
En su lugar utilizan una **base de datos NSS (Network Security Services)**.

NSS es utilizado por aplicaciones como:

* Firefox
* algunas herramientas de autenticación
* ciertos componentes del sistema en distribuciones Linux.

En este ejercicio crearemos una base de datos NSS y añadiremos certificados.

---

### Paso 1 — Instalar las herramientas NSS

Comprueba si la utilidad `certutil` está disponible.

```bash id="yq0nqj"
certutil -H
```

Si el comando no existe, instala el paquete necesario.

```bash id="3d3i4m"
sudo apt install -y libnss3-tools
```

Una vez instalado, vuelve a ejecutar:

```bash id="druqgm"
certutil -H | head
```

Esto confirma que la herramienta está disponible.

---

### Paso 2 — Crear un directorio para la base de datos NSS

Crea un directorio donde almacenaremos la base de datos.

```bash id="nccvsy"
mkdir ~/nss-db
cd ~/nss-db
```

Inicializa una nueva base de datos NSS.

```bash id="4g3t7c"
certutil -N -d sql:.
```

Durante la ejecución se solicitará una contraseña para proteger la base de datos.

Introduce una contraseña sencilla para el laboratorio.

Se crearán varios archivos que representan la base de datos.

Compruébalo:

```bash id="yqafrq"
ls
```

Deberías ver archivos similares a:

```id="3nm96l"
cert9.db
key4.db
pkcs11.txt
```

---

### Paso 3 — Importar el certificado raíz en la base de datos NSS

Importa la autoridad certificadora creada en los laboratorios anteriores.

```bash id="8c4x80"
certutil -A \
-d sql:. \
-n "Training Root CA" \
-t "CT,C,C" \
-i ~/pki-ca/ca.crt
```

Este comando añade el certificado raíz a la base de datos NSS con un nombre identificador.

---

### Paso 4 — Listar los certificados almacenados en la base NSS

Muestra los certificados presentes en la base de datos.

```bash id="hm3p9l"
certutil -L -d sql:.
```

La salida debería mostrar algo similar a:

```id="s1wq8g"
Training Root CA
```

Esto confirma que el certificado raíz ha sido importado correctamente.

---

### Paso 5 — Mostrar detalles del certificado almacenado

Examina el certificado almacenado en la base NSS.

```bash id="q2h4b0"
certutil -L -d sql:. -n "Training Root CA"
```

La salida mostrará la información del certificado, incluyendo:

* sujeto
* emisor
* fechas de validez
* huella digital.

Esto demuestra cómo diferentes sistemas pueden gestionar certificados utilizando almacenes distintos, como NSS en lugar del almacén global del sistema.

---

En este ejercicio hemos creado una base de datos NSS e importado una autoridad certificadora, un mecanismo utilizado por diversas aplicaciones para gestionar certificados de confianza.

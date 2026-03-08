# Laboratorio 10 — Automatización

## Renovar certificados mediante un script Bash

En infraestructuras reales los certificados deben renovarse periódicamente.
Si un certificado expira, los servicios que lo utilizan dejarán de funcionar correctamente.

En este ejercicio construiremos un **script Bash que automatiza la renovación de un certificado** utilizando la autoridad certificadora creada en los laboratorios anteriores.

El objetivo es simular el flujo típico de renovación:

* detectar el certificado actual
* generar una nueva CSR
* firmar el certificado
* reemplazar el certificado antiguo.

---

### Paso 1 — Ir al directorio del servicio

Sitúate en el directorio del servidor que utiliza el certificado.

```bash
cd ~/pki-labs/web-server
```

Comprueba que el certificado actual existe.

```bash
ls
```

Deberías ver archivos como:

```
server.key
server.csr
server.crt
```

Estos archivos representan la identidad actual del servicio.

---

### Paso 2 — Crear un script de renovación

Crea un archivo llamado:

```bash
nano renew-cert.sh
```

Añade el siguiente contenido:

```bash
#!/bin/bash

SERVICE_DIR=~/pki-labs/web-server
CA_DIR=~/pki-ca

echo "Generando nueva CSR..."

openssl req -new \
-key $SERVICE_DIR/server.key \
-out $SERVICE_DIR/server.csr \
-subj "/CN=web.test.local"

echo "Firmando nuevo certificado..."

openssl x509 -req \
-in $SERVICE_DIR/server.csr \
-CA $CA_DIR/intermediate/intermediate.crt \
-CAkey $CA_DIR/intermediate/private/intermediate.key \
-CAcreateserial \
-out $SERVICE_DIR/server.crt \
-days 365 \
-sha256

echo "Certificado renovado correctamente"
```

Guarda el archivo.

Este script reproduce el proceso habitual de renovación dentro de una PKI interna.

---

### Paso 3 — Hacer ejecutable el script

Cambia los permisos del archivo.

```bash
chmod +x renew-cert.sh
```

Comprueba que el script es ejecutable.

```bash
ls -l renew-cert.sh
```

---

### Paso 4 — Ejecutar el proceso de renovación

Ejecuta el script.

```bash
./renew-cert.sh
```

El script realizará automáticamente:

1. generación de una CSR
2. firma del certificado
3. reemplazo del certificado anterior.

---

### Paso 5 — Verificar el nuevo certificado

Analiza el certificado renovado.

```bash
openssl x509 -in server.crt -noout -dates
```

La salida mostrará algo similar a:

```
notBefore=...
notAfter=...
```

Observa que el periodo de validez se ha actualizado.

Esto confirma que el certificado ha sido renovado correctamente.

---

Este tipo de scripts suele utilizarse en entornos donde la renovación automática debe integrarse con procesos de despliegue o sistemas de gestión de configuración.

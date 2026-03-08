# Laboratorio 12 — Solaris

## Simular un keystore Solaris basado en PKCS#11

En sistemas **Solaris**, muchas aplicaciones no utilizan archivos PEM directamente.
En su lugar utilizan el **framework criptográfico PKCS#11**, que permite almacenar claves y certificados dentro de tokens gestionados por el sistema.

En Solaris, herramientas como `pktool` interactúan con estos tokens.

En este laboratorio utilizaremos nuevamente **SoftHSM** para simular un **keystore PKCS#11**, permitiendo comprender cómo se gestionan certificados en un entorno similar al de Solaris.

---

### Paso 1 — Verificar el token creado anteriormente

Lista los tokens disponibles en SoftHSM.

```bash id="2v3t1f"
softhsm2-util --show-slots
```

Deberías ver el token creado en el laboratorio anterior con una salida similar a:

```text id="q9f6k8"
Slot ...
Token label: training-token
```

Este token actuará como nuestro **keystore PKCS#11**.

---

### Paso 2 — Importar un certificado en el token

Utilizaremos el certificado generado anteriormente en los laboratorios PKI.

Comprueba que el certificado del servidor existe.

```bash id="j8c5y2"
ls ~/pki-labs/web-server/server.crt
```

Ahora importa el certificado en el token.

```bash id="w6r3m1"
pkcs11-tool \
--module /usr/lib/softhsm/libsofthsm2.so \
--login \
--write-object ~/pki-labs/web-server/server.crt \
--type cert \
--label server-cert
```

Introduce el **User PIN** cuando se solicite.

Esto almacenará el certificado dentro del token PKCS#11.

---

### Paso 3 — Listar los certificados almacenados

Muestra los objetos almacenados en el token.

```bash id="9z4x8p"
pkcs11-tool \
--module /usr/lib/softhsm/libsofthsm2.so \
--login \
--list-objects
```

Deberías ver entradas similares a:

```text id="3g1c7w"
Private Key Object
Label: server-key

Certificate Object
Label: server-cert
```

Esto indica que el token contiene tanto la clave privada como el certificado.

---

### Paso 4 — Exportar el certificado del token

Extrae el certificado almacenado en el token.

```bash id="6n7k5b"
pkcs11-tool \
--module /usr/lib/softhsm/libsofthsm2.so \
--login \
--read-object \
--type cert \
--label server-cert \
--output-file extracted-server.crt
```

Comprueba que el archivo se ha generado.

```bash id="t1p8c6"
ls extracted-server.crt
```

Ahora puedes inspeccionar el certificado.

```bash id="5y4h9n"
openssl x509 -in extracted-server.crt -text -noout
```

---

### Paso 5 — Relacionar el ejercicio con Solaris

En Solaris, el mismo flujo se realizaría con herramientas como:

```text id="8f3s2d"
pktool import
pktool list
pktool export
```

Estas herramientas interactúan con el **framework PKCS#11 del sistema Solaris** para gestionar claves y certificados.

En este laboratorio hemos simulado el mismo modelo utilizando:

```text id="7p1r9m"
SoftHSM + pkcs11-tool
```

Esto permite comprender cómo funcionan los **keystores PKCS#11 utilizados por Solaris** sin necesidad de disponer de un sistema Solaris real.

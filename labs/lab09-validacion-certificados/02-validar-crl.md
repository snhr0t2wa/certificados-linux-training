# Laboratorio 09 — Validación de certificados

## Revocar un certificado

En el ejercicio anterior generamos una **CRL vacía**.
Ahora revocaremos un certificado emitido por nuestra PKI para observar cómo cambia la lista de revocación.

Este proceso es necesario cuando:

* una clave privada ha sido comprometida
* un servicio deja de ser válido
* se detecta un error en el certificado emitido.

---

### Paso 1 — Identificar el certificado que vamos a revocar

Utilizaremos el certificado del servidor que generamos anteriormente.

Comprueba que el archivo existe.

```bash id="d0g4j7"
ls ~/pki-labs/web-server/server.crt
```

Examina su contenido para identificar su número de serie.

```bash id="0b4g2h"
openssl x509 -in ~/pki-labs/web-server/server.crt -noout -serial
```

La salida será similar a:

```id="41nj1v"
serial=1001
```

Cada certificado emitido por una CA tiene un número de serie único.

---

### Paso 2 — Revocar el certificado utilizando la CA

Sitúate en el directorio de la autoridad certificadora.

```bash id="3hrkpo"
cd ~/pki-ca
```

Revoca el certificado utilizando OpenSSL.

```bash id="5v9g7t"
openssl ca \
-config openssl-ca.cnf \
-revoke ~/pki-labs/web-server/server.crt
```

OpenSSL registrará la revocación en el archivo `index.txt`.

Comprueba el contenido del índice.

```bash id="1g4r31"
cat index.txt
```

Debería aparecer una entrada marcada como revocada.

---

### Paso 3 — Generar una nueva CRL

Después de revocar un certificado es necesario generar una nueva lista de revocación.

```bash id="c41rjv"
openssl ca \
-config openssl-ca.cnf \
-gencrl \
-out crl/ca.crl
```

Esto actualizará la lista de certificados revocados.

---

### Paso 4 — Comprobar el contenido de la CRL

Examina la CRL generada.

```bash id="kskz9a"
openssl crl -in crl/ca.crl -text -noout
```

Busca la sección:

```id="o4a5tq"
Revoked Certificates
```

Debajo debería aparecer el número de serie del certificado revocado.

Esto indica que el certificado ya no debe considerarse válido.

---

### Paso 5 — Verificar un certificado teniendo en cuenta la CRL

Comprueba el certificado del servidor utilizando la CRL generada.

```bash id="p0t3u2"
openssl verify \
-CAfile ca.crt \
-CRLfile crl/ca.crl \
-crl_check \
~/pki-labs/web-server/server.crt
```

La salida debería indicar algo similar a:

```id="1avlq1"
certificate revoked
```

Esto demuestra que el certificado ya no es considerado válido porque ha sido revocado por la autoridad certificadora.

---

En este ejercicio hemos revocado un certificado emitido por nuestra PKI y hemos comprobado cómo la lista de revocación permite detectar certificados que ya no deben ser utilizados.

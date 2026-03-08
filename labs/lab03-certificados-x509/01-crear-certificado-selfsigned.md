# Laboratorio 03 — Certificados X.509

## Crear un certificado autofirmado

En este ejercicio crearemos nuestro primer certificado X.509.
Para ello utilizaremos una de las claves generadas anteriormente y crearemos un **certificado autofirmado (self-signed)**.

Este tipo de certificado se firma a sí mismo utilizando su propia clave privada.
Aunque no es adecuado para servicios públicos en Internet, es muy útil para pruebas, entornos internos o desarrollo.

---

### Paso 1 — Crear un certificado autofirmado a partir de una clave existente

Utilizaremos la clave RSA generada en el laboratorio anterior para crear un certificado autofirmado.

Ejecuta:

```bash
openssl req -new -x509 -key rsa-private.key -out server.crt -days 365
```

Durante la ejecución se solicitarán varios campos que formarán parte del certificado.

Aparecerán preguntas similares a:

```
Country Name
State or Province Name
Locality Name
Organization Name
Common Name
```

Introduce valores simples para los campos.
Cuando aparezca **Common Name**, introduce por ejemplo:

```
test.local
```

Una vez finalizado el proceso se generará un archivo llamado `server.crt`.

Comprueba que el certificado se ha creado.

```bash
ls -l server.crt
```

---

### Paso 2 — Inspeccionar el contenido del certificado generado

Utiliza OpenSSL para ver la información contenida en el certificado.

```bash
openssl x509 -in server.crt -text -noout
```

Observa las primeras secciones del certificado.

Busca especialmente las líneas:

```
Subject:
Issuer:
```

Notarás que ambos campos contienen la misma información.

Esto ocurre porque el certificado es **autofirmado**.

---

### Paso 3 — Verificar que el certificado contiene la clave pública

Un certificado contiene la **clave pública** asociada a la clave privada utilizada para generarlo.

Extrae la clave pública incluida en el certificado.

```bash
openssl x509 -in server.crt -pubkey -noout
```

La salida comenzará con:

```
-----BEGIN PUBLIC KEY-----
```

Esto demuestra que el certificado incluye la clave pública que los clientes utilizarán durante el establecimiento de conexiones TLS.

---

### Paso 4 — Verificar el periodo de validez del certificado

Los certificados tienen un periodo de validez limitado.

Muestra únicamente las fechas de validez del certificado.

```bash
openssl x509 -in server.crt -noout -dates
```

La salida mostrará algo similar a:

```
notBefore=...
notAfter=...
```

Estas fechas indican:

* cuándo comienza a ser válido el certificado
* cuándo deja de ser válido.

Cuando un certificado expira, los clientes rechazan las conexiones TLS que lo utilicen.

---

### Paso 5 — Verificar el certificado con OpenSSL

Intenta verificar el certificado que acabamos de crear.

```bash
openssl verify server.crt
```

Es probable que aparezca un mensaje similar a:

```
self-signed certificate
```

Esto ocurre porque el certificado no ha sido firmado por una autoridad certificadora reconocida.

Los sistemas y navegadores solo confían en certificados firmados por autoridades que están incluidas en sus almacenes de confianza.

---

En este ejercicio hemos creado nuestro primer certificado X.509 y hemos observado cómo se construye a partir de una clave privada y cómo contiene la clave pública correspondiente.

En el siguiente ejercicio analizaremos las extensiones que pueden incluir los certificados y cómo influyen en su uso.

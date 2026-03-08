# Laboratorio 02 — Claves criptográficas

## Generar una clave ECDSA

En el ejercicio anterior generamos una clave RSA.
En este ejercicio generaremos una clave utilizando **criptografía de curva elíptica (ECDSA)**, un algoritmo moderno que ofrece niveles de seguridad similares utilizando claves mucho más pequeñas.

Esto permite conexiones TLS más eficientes y rápidas.

---

### Paso 1 — Generar una clave privada ECDSA

Genera una clave utilizando la curva `prime256v1`, una de las curvas más utilizadas en certificados TLS.

```bash
openssl ecparam -name prime256v1 -genkey -noout -out ecdsa-private.key
```

Comprueba que el archivo se ha creado.

```bash
ls -l ecdsa-private.key
```

La salida debería mostrar un archivo similar a:

```
-rw------- 1 usuario usuario ...
```

Esto indica que la clave privada se ha generado correctamente.

---

### Paso 2 — Inspeccionar el contenido de la clave privada

Visualiza el contenido del archivo.

```bash
cat ecdsa-private.key
```

La salida comenzará con algo similar a:

```
-----BEGIN EC PRIVATE KEY-----
```

y terminará con:

```
-----END EC PRIVATE KEY-----
```

Al igual que en las claves RSA, el contenido está codificado en **formato PEM**.

Este formato es habitual para almacenar claves y certificados.

---

### Paso 3 — Mostrar los detalles de la clave ECDSA

Utiliza OpenSSL para inspeccionar la estructura de la clave.

```bash
openssl ec -in ecdsa-private.key -text -noout
```

En la salida busca la línea que indica la curva utilizada:

```
ASN1 OID: prime256v1
```

Esto confirma que la clave está basada en la curva elíptica `prime256v1`.

Las curvas elípticas permiten ofrecer seguridad criptográfica equivalente a RSA con claves mucho más pequeñas.

---

### Paso 4 — Extraer la clave pública

Deriva la clave pública a partir de la clave privada.

```bash
openssl ec -in ecdsa-private.key -pubout -out ecdsa-public.key
```

Comprueba que el archivo se ha generado.

```bash
ls -l ecdsa-public.key
```

Ahora deberías tener dos archivos:

```
ecdsa-private.key
ecdsa-public.key
```

Esto representa el par de claves asociado.

---

### Paso 5 — Comparar el tamaño de las claves generadas

Compara el tamaño de las claves RSA y ECDSA que has generado.

```bash
ls -lh *.key
```

Observa el tamaño de los archivos:

* `rsa-private.key`
* `ecdsa-private.key`

Normalmente la clave ECDSA será significativamente más pequeña.

Esto ilustra una de las ventajas principales de la criptografía de curva elíptica:
proporciona un nivel de seguridad alto con claves más compactas.

---

En este ejercicio hemos generado una clave basada en curvas elípticas y hemos comparado su tamaño con una clave RSA.

En el siguiente ejercicio examinaremos con más detalle las claves generadas y veremos cómo se utilizan posteriormente para crear certificados.

# Laboratorio 02 — Claves criptográficas

## Generar una clave RSA

En los laboratorios anteriores observamos certificados emitidos por servidores reales.
Antes de crear nuestros propios certificados es necesario generar **claves criptográficas**.

En este ejercicio generaremos una clave privada utilizando el algoritmo **RSA**, uno de los algoritmos más utilizados históricamente en certificados TLS.

---

### Paso 1 — Generar una clave privada RSA

Genera una clave privada RSA de 2048 bits.

```bash
openssl genrsa -out rsa-private.key 2048
```

El comando generará un archivo llamado `rsa-private.key`.

Comprueba que el archivo se ha creado.

```bash
ls -l rsa-private.key
```

Deberías ver una salida similar a:

```
-rw------- 1 usuario usuario ...
```

Este archivo contiene una **clave privada**, por lo que debe mantenerse protegida.

---

### Paso 2 — Inspeccionar el contenido de la clave privada

Visualiza el contenido del archivo generado.

```bash
cat rsa-private.key
```

La salida comenzará con algo similar a:

```
-----BEGIN RSA PRIVATE KEY-----
```

y terminará con:

```
-----END RSA PRIVATE KEY-----
```

El contenido está codificado en **Base64** dentro de un formato llamado **PEM**.

Este formato se utiliza habitualmente para almacenar claves y certificados.

---

### Paso 3 — Mostrar información estructurada de la clave

Utiliza OpenSSL para mostrar los detalles de la clave generada.

```bash
openssl rsa -in rsa-private.key -text -noout
```

La salida mostrará diferentes componentes matemáticos de la clave RSA.

Busca especialmente la línea:

```
Private-Key: (2048 bit)
```

Esto indica el tamaño de la clave que acabamos de generar.

---

### Paso 4 — Extraer la clave pública correspondiente

A partir de una clave privada RSA se puede derivar su clave pública.

Ejecuta:

```bash
openssl rsa -in rsa-private.key -pubout -out rsa-public.key
```

Esto generará un nuevo archivo llamado `rsa-public.key`.

Comprueba que el archivo existe.

```bash
ls -l rsa-public.key
```

Ahora disponemos de:

* una **clave privada**
* una **clave pública** asociada.

---

### Paso 5 — Examinar la clave pública

Visualiza el contenido de la clave pública.

```bash
cat rsa-public.key
```

La salida tendrá un formato similar a:

```
-----BEGIN PUBLIC KEY-----
```

y

```
-----END PUBLIC KEY-----
```

La clave pública es la parte que puede compartirse con otros sistemas.

En el contexto de TLS, esta clave pública se incluye dentro de los certificados que presentan los servidores.

---

En este ejercicio hemos generado nuestra primera clave RSA y hemos visto cómo se relacionan la clave privada y la clave pública.

En el siguiente ejercicio generaremos claves utilizando otros algoritmos criptográficos y compararemos sus características.

# Laboratorio 01 — Explorar TLS

## Conectar manualmente a un servidor TLS

En este ejercicio utilizaremos **OpenSSL como cliente TLS** para conectarnos directamente a un servidor HTTPS y observar qué ocurre durante el establecimiento de la conexión.

Esto nos permitirá ver información que normalmente permanece oculta cuando utilizamos un navegador.

---

### Paso 1 — Abrir una conexión TLS desde la terminal

Utiliza OpenSSL para establecer una conexión TLS con un servidor público.

```bash
openssl s_client -connect google.com:443
```

El comando iniciará una conexión TLS y mostrará el proceso completo de negociación.

Desplázate por la salida hasta localizar líneas similares a:

```
CONNECTED
SSL handshake has read ...
```

Estas líneas indican que se ha iniciado el proceso de negociación TLS entre el cliente y el servidor.

Para finalizar la conexión puedes presionar:

```
CTRL + C
```

---

### Paso 2 — Repetir la conexión observando la información del certificado

Vuelve a ejecutar el comando anterior y observa con más detalle la salida generada.

```bash
openssl s_client -connect google.com:443
```

Busca una sección similar a:

```
Certificate chain
```

Debajo aparecerán varias entradas de certificados.

Observa especialmente las líneas que comienzan con:

```
subject=
issuer=
```

Estas líneas indican:

* la identidad del certificado presentado
* la autoridad certificadora que lo ha firmado.

Esto es parte fundamental del proceso de autenticación del servidor en TLS.

---

### Paso 3 — Identificar el protocolo TLS negociado

Desplázate hacia la parte inferior de la salida del comando.

Localiza una línea similar a:

```
Protocol  : TLSv1.3
```

y también una línea como:

```
Cipher    : TLS_AES_256_GCM_SHA384
```

Estas líneas indican qué versión del protocolo TLS y qué algoritmo de cifrado se han acordado entre cliente y servidor para proteger la comunicación.

---

### Paso 4 — Probar la conexión con otro servidor

Repite el mismo comando pero conectándote a otro servidor HTTPS.

```bash
openssl s_client -connect github.com:443
```

Observa nuevamente:

* la sección `Certificate chain`
* las líneas `subject=` e `issuer=`
* el protocolo TLS utilizado
* el cifrado negociado.

Aunque el procedimiento de conexión es el mismo, notarás que los certificados y algunos parámetros pueden variar entre servidores.

---

### Paso 5 — Observar cómo cambia la información según el servidor

Realiza una tercera conexión con otro servicio HTTPS.

```bash
openssl s_client -connect wikipedia.org:443
```

Busca nuevamente las mismas secciones:

```
Certificate chain
subject=
issuer=
Protocol
Cipher
```

Cada servidor utiliza certificados emitidos por diferentes autoridades certificadoras y puede negociar distintos parámetros criptográficos.

Esto demuestra que cada conexión TLS implica:

* presentación de certificados
* validación de identidad
* negociación de algoritmos de cifrado.

---

En este ejercicio has utilizado OpenSSL para establecer conexiones TLS manuales y observar información sobre certificados y parámetros criptográficos.

En el siguiente ejercicio extraeremos el certificado de un servidor para analizar su estructura en detalle.

# Laboratorio 02 — Claves criptográficas

## Inspeccionar y comparar claves generadas

En los ejercicios anteriores generamos dos tipos de claves:

* una clave **RSA**
* una clave **ECDSA**

En este ejercicio analizaremos ambas claves para comprender mejor sus características y cómo se utilizan posteriormente en certificados y conexiones TLS.

---

### Paso 1 — Ver información resumida de la clave RSA

Muestra información básica de la clave RSA que generaste anteriormente.

```bash
openssl rsa -in rsa-private.key -text -noout
```

La salida mostrará diferentes parámetros matemáticos utilizados por el algoritmo RSA.

Busca especialmente una línea similar a:

```
Private-Key: (2048 bit)
```

Esto indica el tamaño de la clave RSA generada.

Las claves RSA suelen utilizar tamaños de:

* 2048 bits
* 3072 bits
* 4096 bits

Cuanto mayor es el tamaño, mayor es la seguridad, aunque también aumenta el coste computacional.

---

### Paso 2 — Ver información resumida de la clave ECDSA

Ahora muestra los detalles de la clave ECDSA.

```bash
openssl ec -in ecdsa-private.key -text -noout
```

En la salida localiza una línea similar a:

```
ASN1 OID: prime256v1
```

Esto indica la curva elíptica utilizada para generar la clave.

Las curvas más comunes utilizadas en TLS son:

* prime256v1 (P-256)
* secp384r1
* secp521r1

Estas curvas ofrecen niveles de seguridad equivalentes a RSA con claves mucho más pequeñas.

---

### Paso 3 — Extraer únicamente la clave pública de ambas claves

Extrae nuevamente las claves públicas para poder compararlas.

Primero la clave RSA:

```bash
openssl rsa -in rsa-private.key -pubout -out rsa-public.key
```

Ahora la clave ECDSA:

```bash
openssl ec -in ecdsa-private.key -pubout -out ecdsa-public.key
```

Comprueba que los archivos existen.

```bash
ls -l *public.key
```

Ahora dispones de dos claves públicas:

```
rsa-public.key
ecdsa-public.key
```

Estas claves públicas son las que normalmente se incluyen dentro de los certificados.

---

### Paso 4 — Inspeccionar las claves públicas

Muestra el contenido de las claves públicas.

```bash
cat rsa-public.key
```

y después:

```bash
cat ecdsa-public.key
```

Ambas estarán codificadas en formato **PEM** y comenzarán con:

```
-----BEGIN PUBLIC KEY-----
```

Aunque el formato exterior es el mismo, el contenido interno representa algoritmos criptográficos diferentes.

---

### Paso 5 — Comparar el tamaño de las claves

Compara el tamaño de las claves generadas.

```bash
ls -lh *.key
```

Observa especialmente:

* tamaño de `rsa-private.key`
* tamaño de `ecdsa-private.key`

Normalmente la clave RSA será considerablemente más grande.

Esto demuestra una de las ventajas principales de las curvas elípticas:
ofrecen seguridad criptográfica equivalente utilizando claves más pequeñas y operaciones más eficientes.

---

En este ejercicio hemos inspeccionado y comparado claves RSA y ECDSA, observando cómo se representan y cómo se derivan sus claves públicas.

En los siguientes laboratorios utilizaremos estas claves para generar certificados digitales.

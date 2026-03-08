# Laboratorio 04 — Crear una autoridad certificadora

## Crear la clave y el certificado raíz de la CA

En el ejercicio anterior preparamos la estructura de directorios necesaria para una autoridad certificadora.
Ahora crearemos la **clave privada de la CA** y el **certificado raíz** que firmará los certificados emitidos posteriormente.

Este certificado raíz será la base de la cadena de confianza de nuestra PKI.

---

### Paso 1 — Crear la clave privada de la autoridad certificadora

Sitúate dentro del directorio de la CA si aún no estás allí.

```bash
cd ~/pki-ca
```

Genera la clave privada de la CA.

```bash
openssl genrsa -out private/ca.key 4096
```

Comprueba que el archivo se ha creado.

```bash
ls -l private
```

La salida debería mostrar el archivo:

```
ca.key
```

La clave privada de la CA es uno de los elementos más sensibles de una PKI.
En entornos reales suele almacenarse en sistemas protegidos o incluso en módulos de seguridad hardware (HSM).

---

### Paso 2 — Inspeccionar la clave privada generada

Visualiza los detalles de la clave creada.

```bash
openssl rsa -in private/ca.key -text -noout
```

Busca una línea similar a:

```
Private-Key: (4096 bit)
```

Esto confirma el tamaño de la clave utilizada por la autoridad certificadora.

Las autoridades raíz suelen utilizar claves más grandes para reforzar la seguridad de toda la infraestructura.

---

### Paso 3 — Crear el certificado raíz de la CA

Utiliza la clave generada para crear el certificado raíz.

```bash
openssl req -new -x509 \
-key private/ca.key \
-out ca.crt \
-days 3650
```

Durante el proceso se solicitarán varios campos que formarán parte del certificado.

Introduce valores simples para los distintos campos.
Cuando se solicite el **Common Name**, introduce por ejemplo:

```
Training Root CA
```

Una vez finalizado el proceso se generará el archivo `ca.crt`.

Comprueba que el certificado existe.

```bash
ls -l ca.crt
```

---

### Paso 4 — Examinar el certificado raíz generado

Visualiza el contenido del certificado.

```bash
openssl x509 -in ca.crt -text -noout
```

Busca las líneas:

```
Subject:
Issuer:
```

En un certificado raíz ambos campos serán iguales.

Esto indica que el certificado está **autofirmado**, algo normal en las autoridades certificadoras raíz.

---

### Paso 5 — Verificar el certificado raíz

Comprueba el certificado utilizando OpenSSL.

```bash
openssl verify -CAfile ca.crt ca.crt
```

La salida debería indicar:

```
ca.crt: OK
```

Esto ocurre porque estamos utilizando el propio certificado como autoridad de confianza.

Este certificado raíz será utilizado en los siguientes ejercicios para firmar certificados de servidores dentro de nuestra PKI.

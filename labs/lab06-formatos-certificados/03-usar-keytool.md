# Laboratorio 06 — Formatos de certificados

## Gestionar certificados con keytool

Muchas aplicaciones Java utilizan un formato de almacén llamado **Java KeyStore (JKS)** o su variante moderna basada en **PKCS12**.

La herramienta `keytool`, incluida en el JDK, permite gestionar estos almacenes: importar certificados, listar entradas o eliminar claves.

En este ejercicio importaremos el certificado que generamos anteriormente en un **keystore Java**.

---

### Paso 1 — Verificar que keytool está disponible

Comprueba que `keytool` está instalado en el sistema.

```bash id="r7z8gx"
keytool -help | head
```

Si el comando existe aparecerá una lista de operaciones disponibles.

Entre ellas podrás ver comandos como:

```
-importcert
-list
-delete
-genkeypair
```

Estas utilidades permiten gestionar certificados dentro de almacenes Java.

---

### Paso 2 — Crear un nuevo keystore

Genera un nuevo almacén Java que contendrá un par de claves.

```bash id="crnntg"
keytool -genkeypair \
-alias testserver \
-keyalg RSA \
-keystore keystore.jks \
-keysize 2048
```

Durante la ejecución se solicitará una contraseña para el keystore.

Introduce una contraseña sencilla para el laboratorio.

También aparecerán varias preguntas sobre la identidad asociada a la clave.

Cuando se solicite el **Common Name**, introduce por ejemplo:

```
web.test.local
```

Cuando finalice el proceso se generará el archivo:

```
keystore.jks
```

---

### Paso 3 — Listar el contenido del keystore

Examina las entradas almacenadas en el keystore.

```bash id="s02a3v"
keytool -list -keystore keystore.jks
```

Introduce la contraseña cuando se solicite.

La salida mostrará una entrada similar a:

```
testserver, PrivateKeyEntry
```

Esto indica que el keystore contiene un par de claves asociado al alias `testserver`.

---

### Paso 4 — Importar el certificado de la autoridad certificadora

Importa el certificado raíz que creamos anteriormente para que el keystore confíe en esa autoridad.

```bash id="f5mghp"
keytool -importcert \
-alias training-root-ca \
-file ~/pki-ca/ca.crt \
-keystore keystore.jks
```

Durante el proceso aparecerá el contenido del certificado y se preguntará si deseas confiar en él.

Responde:

```
yes
```

Esto añadirá la autoridad certificadora al keystore.

---

### Paso 5 — Verificar el contenido actualizado del keystore

Vuelve a listar el contenido del keystore.

```bash id="c7l3oz"
keytool -list -keystore keystore.jks
```

Ahora deberías ver dos entradas:

```
testserver
training-root-ca
```

Esto indica que el almacén contiene tanto la clave del servidor como el certificado de la autoridad certificadora.

---

En este ejercicio hemos creado un keystore Java y hemos importado certificados utilizando `keytool`, una herramienta ampliamente utilizada en aplicaciones Java y servidores de aplicaciones.

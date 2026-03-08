# Laboratorio 03 — Certificados X.509

## Verificar certificados

En los ejercicios anteriores generamos certificados y examinamos su estructura.
Ahora veremos cómo utilizar OpenSSL para verificar certificados y entender qué condiciones deben cumplirse para que un certificado sea considerado confiable.

---

### Paso 1 — Intentar verificar el certificado autofirmado

Utiliza OpenSSL para verificar el certificado que generaste anteriormente.

```bash
openssl verify server.crt
```

La salida probablemente mostrará un mensaje similar a:

```
error 18 at 0 depth lookup: self-signed certificate
```

Este resultado indica que el certificado no puede verificarse porque está firmado por sí mismo y no pertenece a una autoridad certificadora conocida por el sistema.

Esto demuestra que la verificación de certificados depende de la existencia de una **autoridad certificadora de confianza**.

---

### Paso 2 — Indicar explícitamente el certificado como autoridad de confianza

Podemos pedir a OpenSSL que considere nuestro certificado como una autoridad válida durante la verificación.

Ejecuta:

```bash
openssl verify -CAfile server.crt server.crt
```

La salida debería mostrar:

```
server.crt: OK
```

Esto ocurre porque hemos indicado manualmente que ese certificado debe considerarse una autoridad de confianza.

En entornos reales esto es similar a añadir una CA al **almacén de certificados confiables** del sistema.

---

### Paso 3 — Extraer la huella digital del certificado

Los certificados suelen identificarse mediante una **huella digital (fingerprint)** calculada mediante funciones hash.

Muestra la huella SHA256 del certificado.

```bash
openssl x509 -in server.crt -noout -fingerprint -sha256
```

La salida será similar a:

```
SHA256 Fingerprint=...
```

Esta huella permite identificar de forma única un certificado.

En muchos procesos de seguridad se comparan estas huellas para verificar que un certificado es exactamente el esperado.

---

### Paso 4 — Comparar la huella entre certificados

Muestra también la huella del certificado generado con SAN en el ejercicio anterior.

```bash
openssl x509 -in server-san.crt -noout -fingerprint -sha256
```

Compara ambas huellas.

Aunque ambos certificados pueden tener información similar, las huellas serán diferentes porque:

* fueron generados en momentos distintos
* contienen datos diferentes
* han sido firmados de forma distinta.

Esto demuestra que incluso pequeñas diferencias producen un identificador criptográfico completamente distinto.

---

### Paso 5 — Probar el certificado en un servicio real utilizando Docker

Vamos a utilizar Docker para iniciar un servidor web sencillo que utilice el certificado generado.

Primero crea un directorio para el servidor.

```bash
mkdir web-test
cd web-test
```

Crea un archivo HTML simple.

```bash
echo "Servidor TLS de prueba" > index.html
```

Inicia un contenedor NGINX utilizando el certificado que generaste.

```bash
docker run -d \
-p 8443:443 \
-v $(pwd):/usr/share/nginx/html \
-v ~/pki-labs/server.crt:/etc/nginx/server.crt \
-v ~/pki-labs/rsa-private.key:/etc/nginx/server.key \
nginx
```

Ahora intenta conectarte al servicio desde el entorno Codespace.

```bash
curl -k https://localhost:8443
```

El parámetro `-k` permite a curl aceptar certificados no confiables.

Esto demuestra cómo un certificado puede utilizarse para proteger un servicio TLS aunque no esté firmado por una autoridad reconocida.

---

En este ejercicio hemos utilizado OpenSSL para verificar certificados, comparar huellas digitales y comprobar cómo un certificado puede emplearse para proteger un servicio real.

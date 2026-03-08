# Laboratorio 08 — TLS en servicios

## Probar acceso entre contenedores usando TLS

En los ejercicios anteriores accedimos al servidor HTTPS desde el propio entorno del host.
Ahora realizaremos la conexión desde **otro contenedor Docker**, simulando el comportamiento de un cliente independiente.

Esto permite reproducir escenarios reales de comunicación entre servicios.

---

### Paso 1 — Identificar el contenedor del servidor NGINX

Primero localiza el contenedor que ejecuta el servidor HTTPS.

```bash
docker ps
```

En la salida aparecerá un contenedor basado en la imagen `nginx`.

Observa especialmente la columna:

```
PORTS
```

Debería aparecer algo similar a:

```
0.0.0.0:8443->443/tcp
```

Esto indica que el puerto HTTPS del contenedor está expuesto en el puerto `8443` del entorno.

---

### Paso 2 — Crear un contenedor cliente temporal

Lanzaremos un contenedor que actuará como cliente para probar la conexión TLS.

```bash
docker run -it --rm curlimages/curl sh
```

Se abrirá una shell dentro del contenedor.

Desde este contenedor intentaremos conectarnos al servidor HTTPS.

---

### Paso 3 — Probar la conexión HTTPS desde el contenedor cliente

Dentro del contenedor ejecuta:

```bash
curl https://host.docker.internal:8443
```

Es posible que aparezca un error relacionado con el certificado.

Esto ocurre porque el contenedor cliente no confía en la autoridad certificadora que creamos en los laboratorios.

Esto reproduce el comportamiento habitual cuando un cliente intenta conectarse a un servicio que utiliza una CA desconocida.

---

### Paso 4 — Probar la conexión ignorando la verificación del certificado

Para observar la respuesta del servidor, repite la conexión ignorando la verificación del certificado.

```bash
curl -k https://host.docker.internal:8443
```

La salida debería mostrar el contenido del servidor:

```
Servidor HTTPS de prueba
```

Esto confirma que la conexión TLS funciona correctamente aunque el cliente no confíe en la CA.

---

### Paso 5 — Probar la conexión confiando en la CA del laboratorio

Primero copia el certificado raíz desde el host.

En otra terminal del Codespace ejecuta:

```bash
cat ~/pki-ca/ca.crt
```

Copia el contenido completo del certificado.

Dentro del contenedor cliente crea un archivo para ese certificado.

```bash
nano ca.crt
```

Pega el contenido del certificado y guarda el archivo.

Ahora realiza la conexión indicando explícitamente la CA.

```bash
curl --cacert ca.crt https://host.docker.internal:8443
```

Esta vez la conexión debería realizarse sin errores de certificado.

Esto demuestra cómo los clientes pueden confiar en certificados cuando disponen de la autoridad certificadora correspondiente.

---

En este ejercicio hemos simulado una comunicación entre servicios utilizando contenedores Docker y hemos observado cómo influye la confianza en las autoridades certificadoras durante las conexiones TLS.

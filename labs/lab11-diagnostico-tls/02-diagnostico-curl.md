# Laboratorio 11 — Diagnóstico TLS

## Diagnosticar conexiones TLS con curl

Además de OpenSSL, una de las herramientas más utilizadas para comprobar servicios TLS es **curl**.
curl permite realizar peticiones HTTP y HTTPS mostrando información detallada sobre la conexión.

Esto permite detectar problemas comunes como:

* certificados no confiables
* errores en la cadena de certificados
* problemas de hostname
* errores de verificación TLS.

En este ejercicio utilizaremos curl para analizar la conexión con el servidor HTTPS configurado en los laboratorios anteriores.

---

### Paso 1 — Probar la conexión HTTPS básica

Realiza una petición HTTPS al servidor del laboratorio.

```bash id="9y1u9p"
curl https://localhost:8443
```

Es posible que aparezca un error similar a:

```text id="y5c1a0"
curl: (60) SSL certificate problem: unable to get local issuer certificate
```

Este error indica que curl no puede verificar el certificado del servidor.

Esto ocurre cuando la autoridad certificadora no está presente en el almacén de confianza del sistema.

---

### Paso 2 — Mostrar información detallada de la conexión

Utiliza el modo verbose de curl para ver el proceso TLS.

```bash id="x0g7m3"
curl -v https://localhost:8443
```

En la salida aparecerán líneas relacionadas con TLS, por ejemplo:

```text id="a0h8u2"
* SSL connection using TLSv1.3
* Server certificate:
```

También aparecerá información sobre el certificado presentado por el servidor.

---

### Paso 3 — Conectarse ignorando la verificación del certificado

Para comprobar si el servidor responde correctamente, ignora temporalmente la verificación TLS.

```bash id="8u7p2m"
curl -k https://localhost:8443
```

La opción `-k` indica a curl que no verifique el certificado del servidor.

Si el servidor funciona correctamente deberías recibir la respuesta HTTP del servicio.

Esto confirma que el problema está relacionado con la validación del certificado.

---

### Paso 4 — Especificar manualmente la autoridad certificadora

Indica a curl qué autoridad certificadora debe utilizar para validar el certificado.

```bash id="9c1p5z"
curl --cacert ~/pki-ca/ca.crt https://localhost:8443
```

Esta vez la conexión debería realizarse correctamente sin errores TLS.

Esto demuestra cómo los clientes pueden confiar en un certificado cuando conocen la autoridad certificadora que lo firmó.

---

### Paso 5 — Analizar el certificado del servidor con curl

Ejecuta nuevamente curl en modo verbose.

```bash id="5u4l1r"
curl -v --cacert ~/pki-ca/ca.crt https://localhost:8443
```

Observa en la salida líneas similares a:

```text id="3g9v6t"
subject:
issuer:
SSL certificate verify ok
```

Estas líneas confirman que curl ha podido validar correctamente el certificado del servidor utilizando la CA proporcionada.

---

En este ejercicio hemos utilizado curl para diagnosticar problemas de validación TLS y comprobar cómo influye la configuración del almacén de certificados en la verificación de conexiones seguras.

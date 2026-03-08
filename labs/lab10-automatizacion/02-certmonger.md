# Laboratorio 10 — Automatización

## Solicitar y renovar certificados con certmonger

En entornos Linux empresariales es habitual utilizar **certmonger** para gestionar certificados de forma automática.

certmonger es un servicio que:

* solicita certificados a una autoridad certificadora
* monitoriza su expiración
* renueva automáticamente los certificados antes de que caduquen.

En este ejercicio utilizaremos `certmonger` para solicitar y gestionar un certificado.

---

### Paso 1 — Instalar certmonger

Comprueba si `certmonger` está disponible en el sistema.

```bash id="d1t9h3"
getcert list
```

Si el comando no existe, instala el paquete correspondiente.

En sistemas basados en Debian o Ubuntu:

```bash id="9r3t0c"
sudo apt install -y certmonger
```

Una vez instalado, inicia el servicio.

```bash id="3u1m7x"
sudo systemctl start certmonger
```

Comprueba que el servicio está activo.

```bash id="5h8l4k"
sudo systemctl status certmonger
```

---

### Paso 2 — Crear un directorio para el certificado gestionado

Crea un directorio donde certmonger almacenará los certificados.

```bash id="r3n8d5"
mkdir -p ~/certmonger-test
cd ~/certmonger-test
```

Este directorio contendrá la clave privada y el certificado gestionados por certmonger.

---

### Paso 3 — Solicitar un certificado

Solicita un certificado indicando dónde almacenar la clave y el certificado.

```bash id="6k4v2y"
getcert request \
-f ~/certmonger-test/server.crt \
-k ~/certmonger-test/server.key \
-N CN=web.test.local
```

Este comando indica a certmonger que:

* genere una clave privada
* cree una solicitud de certificado
* gestione el certificado resultante.

---

### Paso 4 — Ver el estado de la solicitud

Muestra las solicitudes de certificados gestionadas por certmonger.

```bash id="7s3x6p"
getcert list
```

La salida mostrará información sobre el certificado gestionado, incluyendo:

* identificador de la solicitud
* ubicación del certificado
* estado actual.

Esto permite comprobar si el certificado está activo o pendiente de renovación.

---

### Paso 5 — Simular la renovación del certificado

certmonger supervisa automáticamente la expiración de certificados.

Para forzar una renovación manual utiliza el identificador mostrado en el paso anterior.

```bash id="2n4f1y"
getcert resubmit -i <ID_DE_SOLICITUD>
```

Sustituye `<ID_DE_SOLICITUD>` por el identificador real que aparece en `getcert list`.

Después vuelve a consultar el estado.

```bash id="4p8v2d"
getcert list
```

Esto permite comprobar cómo certmonger gestiona automáticamente la renovación del certificado.

---

En este ejercicio hemos utilizado certmonger para solicitar y gestionar certificados de forma automática, un mecanismo utilizado frecuentemente en infraestructuras Linux empresariales.

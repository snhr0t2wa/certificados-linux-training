# Laboratorio 00 — Preparar entorno

## Instalar herramientas necesarias

En este ejercicio instalaremos algunas herramientas que utilizaremos durante el curso para analizar conexiones TLS, resolver nombres de red y probar servicios HTTPS.

Estas utilidades complementan el uso de OpenSSL y facilitan el diagnóstico de problemas relacionados con certificados y comunicaciones seguras.

---

### Paso 1 — Actualizar el índice de paquetes del sistema

Antes de instalar nuevas herramientas es recomendable actualizar la información de paquetes disponibles en el sistema.

Ejecuta:

```bash
sudo apt update
```

Durante la ejecución se descargarán los índices de paquetes desde los repositorios configurados.

Observa cómo aparecen diferentes servidores de repositorio y se actualizan las listas de paquetes disponibles.

Cuando el comando finalice volverás al prompt de la terminal.

---

### Paso 2 — Instalar herramientas de diagnóstico de red

Instalaremos varias utilidades que utilizaremos en distintos laboratorios para probar conexiones seguras y analizar servicios.

Ejecuta:

```bash
sudo apt install -y curl wget dnsutils net-tools
```

Durante la instalación el sistema descargará e instalará los paquetes necesarios.

Estas herramientas permiten:

* realizar peticiones HTTP y HTTPS
* comprobar conectividad de red
* resolver nombres DNS
* inspeccionar interfaces y conexiones de red.

Cuando el comando finalice vuelve al prompt de la terminal.

---

### Paso 3 — Verificar que curl puede establecer conexiones HTTPS

Utilizaremos `curl` para realizar una petición HTTPS a un servidor público.

Ejecuta:

```bash
curl https://google.com
```

Deberías recibir una respuesta HTML del servidor.

Este comando establece una conexión HTTPS completa, lo que implica:

* resolución DNS del servidor
* negociación TLS
* validación del certificado del servidor
* descarga del contenido solicitado.

Esto confirma que el sistema puede validar certificados correctamente utilizando el almacén de autoridades certificadoras del sistema operativo.

---

### Paso 4 — Mostrar detalles de la conexión TLS

Podemos pedir a `curl` que muestre información detallada sobre la conexión TLS.

Ejecuta:

```bash
curl -v https://google.com
```

Observa en la salida líneas similares a:

```
SSL connection using TLS
```

y también:

```
Server certificate
```

Estas líneas muestran información sobre el protocolo TLS utilizado y el certificado presentado por el servidor.

Este tipo de información será útil más adelante cuando analicemos problemas de certificados o configuraciones TLS incorrectas.

---

### Paso 5 — Comprobar la resolución DNS de un servidor

Las conexiones TLS dependen también de que el nombre del servidor pueda resolverse correctamente mediante DNS.

Comprueba la resolución de un dominio público.

```bash
dig google.com
```

Busca en la salida la sección:

```
ANSWER SECTION
```

Ahí aparecerá la dirección IP asociada al nombre del servidor.

Esto confirma que el sistema puede resolver correctamente el nombre del servidor antes de establecer una conexión segura.

---

Con estas herramientas instaladas y verificadas el entorno está preparado para comenzar a explorar conexiones TLS y certificados en los siguientes laboratorios.

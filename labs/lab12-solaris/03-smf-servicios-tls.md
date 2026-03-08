## Lab 03 — SMF y servicios TLS en Solaris (conceptos)

En este ejercicio revisarás el papel de **SMF (Service Management Facility)** en la gestión de servicios en Solaris, y cómo se relaciona con el uso de certificados TLS.

Aunque el entorno del curso se ejecuta sobre Linux, este laboratorio te ayudará a entender los paralelismos entre ambos sistemas.

---

### 1. ¿Qué es SMF?

Investiga y anota:

* qué es SMF y qué problema resuelve
* qué componentes principales tiene (manifiestos, métodos de servicio, estados, etc.)
* cómo se definen y gestionan servicios en Solaris mediante SMF.

Puedes buscar en la documentación oficial de Solaris o en recursos de referencia que tengas disponibles.

---

### 2. Relación entre SMF y certificados

En Solaris, los servicios que utilizan TLS pueden depender de:

* keystores basados en PKCS#11
* certificados gestionados por el framework criptográfico del sistema
* parámetros de configuración que indican qué claves/certificados utilizar.

Reflexiona sobre:

* cómo SMF puede garantizar que un servicio solo arranque si el keystore y los certificados necesarios están disponibles
* cómo se podrían definir dependencias entre servicios (por ejemplo, un servicio que requiere que el framework criptográfico esté inicializado).

---

### 3. Paralelismo con systemd en Linux

Aunque Linux utiliza principalmente `systemd` en muchas distribuciones modernas, hay similitudes conceptuales:

* unidades de servicio (`.service`)
* dependencias (`Requires=`, `After=`, etc.)
* integración con scripts de inicialización y recarga.

En tu sistema Linux, inspecciona la definición de un servicio que utilice TLS (por ejemplo, `nginx.service` o `apache2.service`):

```bash
systemctl cat nginx.service
```

Anota:

* cómo se gestiona el arranque y la recarga del servicio
* qué comandos se utilizan para iniciar/detener/recargar.

Imagina cómo conceptos similares se expresan en SMF.

---

### 4. Escenario conceptual: rotación de certificados en Solaris

Diseña (en texto o esquemáticamente) un flujo de alto nivel para rotar certificados en un servicio Solaris gestionado por SMF:

1. Actualización de claves/certificados en un keystore PKCS#11.
2. Notificación o recarga del servicio mediante SMF.
3. Verificación de que el servicio está utilizando el nuevo certificado.

Piensa en:

* qué comandos o herramientas de Solaris podrían intervenir (por ejemplo, `svcadm`, `svcs`)
* cómo se podría integrar con un proceso de automatización más amplio (scripts, herramientas de gestión de configuración).

---

### 5. Relación con lo aprendido en Linux

Compara:

* gestión de servicios TLS con `systemd` + archivos de configuración de aplicaciones
* gestión de servicios TLS con SMF + framework criptográfico de Solaris.

Identifica al menos tres similitudes y tres diferencias relevantes.

---

### 6. Conclusiones

Al finalizar este ejercicio deberías ser capaz de:

* describir el papel de SMF en la gestión de servicios en Solaris
* explicar cómo SMF puede integrarse con el uso de certificados y keystores PKCS#11
* establecer paralelismos entre la gestión de servicios TLS en Linux y en Solaris.


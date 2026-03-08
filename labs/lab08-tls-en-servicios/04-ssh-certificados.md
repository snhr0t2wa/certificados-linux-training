## Lab 04 — Uso de certificados en SSH

En este ejercicio explorarás el uso de **autoridades certificadoras SSH** para firmar claves de usuarios y servidores.

Aunque SSH no utiliza certificados X.509, la idea de una **CA que firma identidades** es similar a la de una PKI.

---

### 1. Crear una CA SSH

1. Crea un directorio de trabajo:

```bash
mkdir -p ~/labs/ssh-ca
cd ~/labs/ssh-ca
```

2. Genera una clave para la CA SSH:

```bash
ssh-keygen -f ssh_ca -N "" -C "SSH CA del laboratorio"
```

Esto crea:

* `ssh_ca` (clave privada de la CA)
* `ssh_ca.pub` (clave pública de la CA).

---

### 2. Generar una clave para un usuario

1. Genera una clave para un usuario (por ejemplo `alice`):

```bash
ssh-keygen -f alice -N "" -C "alice@lab"
```

2. Observa los archivos generados:

* `alice`
* `alice.pub`

---

### 3. Firmar la clave del usuario con la CA SSH

1. Utiliza la CA SSH para firmar la clave pública de `alice`:

```bash
ssh-keygen -s ssh_ca -I alice-cert -n alice alice.pub
```

2. Comprueba que se ha generado un nuevo archivo:

* `alice-cert.pub`

3. Analiza el certificado SSH:

```bash
ssh-keygen -L -f alice-cert.pub
```

Observa:

* identidad (`Key ID`)
* nombres permitidos (`Principals`)
* período de validez.

---

### 4. Configurar un servidor SSH para confiar en la CA

En un servidor de pruebas (por ejemplo, un contenedor o una máquina local), añade la clave pública de la CA SSH a la configuración de `sshd`.

1. Copia `ssh_ca.pub` al servidor.
2. Edita la configuración de `sshd` (normalmente `/etc/ssh/sshd_config`) y añade:

```text
TrustedUserCAKeys /etc/ssh/ca_trusted_keys.pub
```

3. Coloca `ssh_ca.pub` en `/etc/ssh/ca_trusted_keys.pub`.
4. Reinicia o recarga el servicio `sshd`.

> Nota: en el entorno del curso puedes simular esta configuración en un contenedor o en una máquina virtual de laboratorio.

---

### 5. Probar el acceso con certificados SSH

1. Copia `alice`, `alice.pub` y `alice-cert.pub` al cliente que se conectará al servidor.
2. Configura un archivo `~/.ssh/config` para usar el certificado:

```text
Host servidor-lab
    HostName <IP_o_nombre_del_servidor>
    User alice
    IdentityFile ~/labs/ssh-ca/alice
    IdentitiesOnly yes
```

3. Asegúrate de que el archivo `alice-cert.pub` esté en el mismo directorio que `alice`.
4. Intenta conectarte:

```bash
ssh servidor-lab
```

Si el servidor confía en la CA SSH, aceptará el certificado de `alice`.

---

### 6. Relación con la PKI del curso

Reflexiona sobre las similitudes y diferencias entre:

* una CA SSH que firma claves de usuario
* una CA X.509 que firma certificados de servidor.

Piensa especialmente en:

* cómo se distribuye la confianza (AuthorizedKeys vs TrustedUserCAKeys, keystores, etc.)
* períodos de validez y rotación de claves/certificados.

---

### 7. Conclusiones

Al finalizar este ejercicio deberías ser capaz de:

* crear una CA SSH y firmar claves de usuario
* configurar un servidor SSH para confiar en una CA
* relacionar el modelo de confianza de SSH con el modelo PKI utilizado en TLS.


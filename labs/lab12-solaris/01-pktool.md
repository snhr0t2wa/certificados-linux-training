# Laboratorio 12 — Solaris

## Simular pktool utilizando PKCS#11 en Linux

En sistemas **Solaris**, la herramienta habitual para gestionar claves criptográficas es **pktool**, que trabaja sobre el framework **PKCS#11** del sistema.

Como no disponemos de un sistema Solaris dentro del Codespace, en este laboratorio utilizaremos herramientas Linux que trabajan también con **PKCS#11**, permitiendo reproducir el mismo modelo de funcionamiento.

En particular utilizaremos:

* **SoftHSM** para simular un token criptográfico
* **pkcs11-tool** para interactuar con ese token.

Esto permite comprender cómo funcionan los almacenes PKCS#11 que Solaris gestiona mediante `pktool`.

---

### Paso 1 — Instalar las herramientas PKCS#11

Instala las herramientas necesarias.

```bash id="1qk2n8"
sudo apt update
sudo apt install -y softhsm2 opensc
```

Comprueba que las herramientas están disponibles.

```bash id="g6y8p3"
softhsm2-util --version
```

y:

```bash id="4k2h9x"
pkcs11-tool --version
```

Estas herramientas nos permitirán simular un token PKCS#11 dentro del sistema.

---

### Paso 2 — Inicializar un token PKCS#11

Crea un nuevo token en SoftHSM.

```bash id="q1h3v7"
softhsm2-util --init-token --slot 0 --label training-token
```

Durante la inicialización se solicitarán dos PIN:

* **SO PIN** (administrador del token)
* **User PIN** (usuario del token)

Introduce valores sencillos para el laboratorio.

Comprueba que el token se ha creado.

```bash id="2z8s1r"
softhsm2-util --show-slots
```

Deberías ver un token con la etiqueta:

```text id="f3v0n5"
training-token
```

---

### Paso 3 — Generar un par de claves dentro del token

Utiliza `pkcs11-tool` para generar un par de claves RSA dentro del token.

```bash id="5m7j2c"
pkcs11-tool \
--module /usr/lib/softhsm/libsofthsm2.so \
--login \
--keypairgen \
--key-type rsa:2048 \
--label server-key
```

Introduce el **User PIN** cuando se solicite.

Este comando crea un par de claves dentro del token PKCS#11.

---

### Paso 4 — Listar las claves almacenadas

Muestra los objetos almacenados en el token.

```bash id="8c9r1v"
pkcs11-tool \
--module /usr/lib/softhsm/libsofthsm2.so \
--login \
--list-objects
```

La salida debería mostrar algo similar a:

```text id="u4y7e3"
Private Key Object
Label: server-key
```

Esto confirma que la clave se encuentra almacenada dentro del token PKCS#11.

---

### Paso 5 — Relacionar el ejercicio con Solaris

En Solaris, la herramienta equivalente sería:

```text id="v7m3d9"
pktool genkey
pktool list
pktool delete
```

Estas operaciones utilizan el **framework PKCS#11 del sistema Solaris** para almacenar claves en tokens criptográficos.

En este laboratorio hemos reproducido el mismo modelo utilizando:

```text id="x5k2p8"
SoftHSM + pkcs11-tool
```

Esto permite comprender cómo funcionan los almacenes PKCS#11 utilizados por herramientas como `pktool` en Solaris.

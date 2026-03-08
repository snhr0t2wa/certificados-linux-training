## Lab 05 — Rotación de certificados con Ansible

En este ejercicio crearás un ejemplo sencillo de **playbook de Ansible** para desplegar certificados en un servicio y aplicar una rotación controlada.

El objetivo es ver cómo Ansible puede integrarse con los procesos de renovación que has automatizado con scripts o herramientas como `certmonger`.

---

### 0. Lanzar un contenedor gestionado (en Codespaces)

En el entorno de Codespaces, Ansible se ejecutará en el propio Codespace como **nodo de control** y gestionará uno o varios contenedores Docker como **hosts remotos**.

1. Lanza un contenedor de pruebas con un servicio web (por ejemplo `nginx`) y SSH habilitado. Un ejemplo orientativo podría ser:

```bash
docker run -d --name web1 -p 2222:22 -p 8080:80 imagen-con-nginx-y-sshd
```

> Nota: adapta la imagen y la configuración de SSH según las instrucciones del curso o de tu entorno de laboratorio. El objetivo es disponer de un contenedor accesible por SSH desde el Codespace.

---

### 1. Preparar el entorno de Ansible

1. Crea un directorio de trabajo:

```bash
mkdir -p ~/labs/ansible-certs
cd ~/labs/ansible-certs
```

2. Crea un inventario mínimo (`hosts.ini`) donde el host gestionado sea el contenedor `web1` al que te conectas por SSH desde el Codespace:

```ini
[webservers]
web1 ansible_host=127.0.0.1 ansible_port=2222 ansible_user=root ansible_connection=ssh
```

---

### 2. Estructura básica del playbook

1. Crea un archivo `deploy_certs.yml` con una estructura básica:

```yaml
- name: Desplegar certificados TLS
  hosts: webservers
  become: true

  vars:
    cert_source_dir: "/path/a/tus/certificados"   # ajusta esta ruta
    cert_dest_dir: "/etc/ssl/local"

  tasks:
    - name: Crear directorio de certificados
      file:
        path: "{{ cert_dest_dir }}"
        state: directory
        owner: root
        group: root
        mode: '0755'

    - name: Copiar certificado y clave
      copy:
        src: "{{ cert_source_dir }}/{{ item }}"
        dest: "{{ cert_dest_dir }}/{{ item }}"
        owner: root
        group: root
        mode: '0640'
      loop:
        - server.crt
        - server.key

    - name: Recargar servicio web si cambia el certificado
      service:
        name: nginx
        state: reloaded
      notify: []

  handlers: []
```

2. Ajusta las rutas y el nombre del servicio (`nginx`, `apache2`, etc.) según tu entorno de laboratorio.

---

### 3. Hacer el playbook idempotente

El módulo `copy` de Ansible ya comprueba si los archivos han cambiado.

Comprueba este comportamiento:

1. Ejecuta el playbook por primera vez:

```bash
ansible-playbook -i hosts.ini deploy_certs.yml
```

2. Ejecuta el playbook una segunda vez sin cambiar nada.

Observa en la salida:

* qué tareas aparecen como `changed`
* qué tareas aparecen como `ok`.

La idea es que el servicio solo se recargue cuando cambie el certificado o la clave.

---

### 4. Simular una rotación de certificado

1. Genera un nuevo certificado para el mismo servicio utilizando la PKI del curso.
2. Sustituye `server.crt` y `server.key` en `cert_source_dir` por las nuevas versiones.
3. Ejecuta de nuevo el playbook:

```bash
ansible-playbook -i hosts.ini deploy_certs.yml
```

Comprueba que:

* las tareas de copia marcan `changed`
* el servicio se recarga para aplicar los nuevos certificados.

---

### 5. Relación con cron y certmonger

Reflexiona sobre un flujo posible en producción:

* un script o `certmonger` renueva el certificado y lo deja en una ubicación central
* un job de Ansible (lanzado manualmente o desde un scheduler) despliega el certificado en todos los servidores afectados
* los servicios se recargan de forma controlada.

Anota este flujo paso a paso en un archivo de notas (`flujo-rotacion-ansible.md`).

---

### 6. Conclusiones

Al finalizar este ejercicio deberías ser capaz de:

* escribir un playbook sencillo para desplegar certificados TLS
* aplicar la rotación de certificados de forma idempotente
* integrar Ansible en una estrategia más amplia de automatización de certificados (scripts, cron, certmonger).


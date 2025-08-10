# Obligatorio Taller Linux Julio 2025

Hecho por: Fabricio Barreiro y Santiago Hoaguy

## Uso de los playbooks de Ansible

Este proyecto utiliza **Ansible** para automatizar la configuración de servidores y servicios, adaptado específicamente a nuestro entorno para el Obligatorio de Taller Linux 2025.

### Requisitos previos

Seguir los pasos documentados en el archivo instalaciondehosts.md

Luego podra continuar aqui.

#### Estructura del repositorio

```plaintext
tallerLinux2025/
├── inventory.ini              # Archivo de inventario con los hosts gestionados
├── nfs_setup.yml              # Playbook para configurar el servidor NFS en CentOS
├── hardening.yml              # Playbook para aplicar hardening en sistemas Ubuntu
└── README.md                  # Documentación del proyecto y uso de los playbooks
Documentos/
├── imagenes/               # Carpeta que contiene capturas de pantalla usadas en la documentación.
├── ansible_basics.md       # Explicación sobre ansible, que es y como funciona.
├── comandosad-hoc.md       # Lista de comandos ad-hoc usados en el proyecto.
├── instalaciondehosts.md   # Procedimiento de instalación del setup general, hosts y bastion.
└── prueba-de-conexion.md   # Comprobación de conexión con los hosts.
```


#### 1. Servidor donde corre Ansible

- **Ansible** (última versión estable) instalado.
- **Python 3** instalado.
- Colección `ansible.posix` instalada (requerida para tareas de manejo de usuarios y permisos POSIX).
  También se requiere la colección `community.general`.
- **Conexión SSH** funcionando hacia los hosts gestionados, preferentemente usando **clave pública** (sin password para mayor seguridad y automatización).

Instalamos ansible con el siguiente comando en el caso de Centos
```bash
sudo dnf install ansible-core
```
Generamos un juego de llaves con el comando:
```bash
ssh-keygen
```
Una vez que generé las claves debo copiarlas a los equipos que voy a controlar, usando el comando
```bash
ssh-copy-id
```
En el caso de ubuntu debemos instalar SSH con:
```bash
sudo apt install openssh-server
```

Ejemplo para instalar las colecciones necesarias:
```bash
ansible-galaxy collection install community.general
ansible-galaxy collection install ansible.posix
```

#### 2. Hosts gestionados (servidores a administrar)

- **Python 3** instalado.
- Usuario con permisos de **sudo** (puede requerir contraseña; Ansible la pedirá con `-K`).
- Servicio **SSH** habilitado y accesible desde el servidor Ansible.
- **Clave pública SSH** del servidor Ansible agregada al usuario remoto para autenticación sin password.

#### Notas específicas por sistema operativo

- El playbook `nfs_setup.yml` está diseñado para sistemas CentOS/RedHat.
- El playbook `hardening.yml` está diseñado para sistemas Ubuntu/Debian.
- En nuestro caso, el usuario con el que accedemos por SSH remoto debe estar agregado al grupo `sudoers` para poder ejecutar tareas con privilegios elevados.

> **Nota:** El archivo de inventario `inventory.ini` ya está incluido en este repositorio. Debe adaptarse para reflejar las IPs y nombres de los hosts de tu entorno antes de ejecutar los playbooks.

#### Problemas comunes y soluciones rápidas

- **Error de permiso SSH:**
  Si Ansible no puede conectarse vía SSH, asegúrate de que la clave pública del servidor Ansible esté correctamente copiada al archivo `~/.ssh/authorized_keys` del usuario remoto y que los permisos de los archivos y directorios `.ssh` sean correctos.

- **Contraseña sudo solicitada o denegada:**
  Verifica que el usuario remoto tenga permisos sudo configurados correctamente y que el parámetro `-K` se utilice para solicitar la contraseña si es necesario. También puedes configurar sudoers para no pedir contraseña si la política lo permite.

- **Problemas instalando colecciones de Ansible:**
  Asegúrate de tener conexión a internet y permisos adecuados para instalar colecciones. Usa `ansible-galaxy collection install ansible.posix` para instalar la colección requerida para tareas POSIX.

---

### Ejecución de los playbooks del proyecto

Este proyecto incluye dos playbooks principales:

#### 1. `nfs_setup.yml`

- **¿Qué hace?**
Configura un servidor NFS sobre CentOS, instalando los paquetes necesarios, creando los directorios de exportación y configurando los permisos y servicios requeridos para compartir archivos vía NFS.

Especificamente cumple los siguientes puntos:
- El servidor NFS esté instalado
- Se asegura que el servicio NFS esté iniciado y funcionando
- El firewall permita conexiones al puerto 2049
- Exista el directorio /var/nfs_shared, que pertenece al usuario/grupo nobody/nobody y tiene
permisos 777
- El directorio está compartido por NFS.
- Debe haber un handler que actualice relea el archivo /etc/exports si este cambia.

- **Comando de ejecución:**
```bash
ansible-playbook -i inventory.ini nfs_setup.yml -K
```
El parámetro `-K` solicitará la contraseña de sudo para los hosts gestionados, si es necesaria.

#### 2. `hardening.yml`

- **¿Qué hace?**
Este playbook aplica mejoras de seguridad y realiza la actualización de paquetes en sistemas Ubuntu. Incluye tareas como la actualización de los paquetes del sistema (`apt update` y `apt upgrade`), la configuración de parámetros de seguridad recomendados, y otros ajustes de hardening del sistema operativo para mejorar su protección frente a amenazas comunes.

Especificamente cumple los siguientes puntos:
- Actualizar todos los paquetes
- Que esté habilitado ufw, bloqueando todo el tráfico entrante y permitiendo solo ssh.
- Que solo se pueda hacer login con clave pública, y que root no pueda hacer login.
- Que esté instalado fail2ban y bloquee intentos fallidos de conexión SSH. El servicio debe
quedar habilitado y activado.
- Debe haber un handler que reinicie el sistema si se actualizan paquetes.
- Debe haber un handler que reinicie ssh si cambia la configuración.

- **Comando de ejecución:**
```bash
ansible-playbook -i inventory.ini hardening.yml -K
```

---

### Buenas prácticas recomendadas

- Antes de aplicar cambios, es recomendable probar el playbook en modo "dry-run" usando el parámetro `--check`:
  ```bash
  ansible-playbook -i inventory.ini nfs_setup.yml -K --check
  ```
  Esto muestra qué cambios se realizarían, sin modificar nada.

---

### verificacion de playbooks

- **Salida esperada al ejecutar:**

- Salida del playbook nfs_setup.yml

  ![Texto alternativo](imagenes/EjecucionDeNfs_setup.yml.png)


---

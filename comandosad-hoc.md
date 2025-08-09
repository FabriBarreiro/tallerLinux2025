# Parte 3: Ejecución de comandos ad-hoc

Se muestran los comandos utilizados, con explicacion y resultado de los mismos.

---

## 1. Listar todos los usuarios en servidor Ubuntu

```bash
ansible ubuntu -i inventory.ini -m shell -a 'getent passwd | cut -d: -f1 | sort'
```

- `getent passwd` consulta la base de datos de contraseñas y muestra información sobre las cuentas de usuario.
- `cut -d: -f1` se queda solo con el nombre de usuario.
- `sort` los ordena alfabéticamente.

---

## 2. Mostrar el uso de memoria en todos los servidores

```bash
ansible linux -i inventory.ini -m command -a 'free -h'
```

- Ejecuta `free -h` en Ubuntu y CentOS (porque el grupo `linux` incluye a ambos).
- Muestra memoria total, usada, libre, caché y swap en formato legible.

---

## 3. Que el servicio chrony esté instalado y funcionando en servidor CentOS

### Instalar chrony
```bash
ansible centos -i inventory.ini -b -m package -a 'name=chrony state=present'
```

- `-b` / `--become`: eleva privilegios (instalar requiere sudo).
- `package`: usa el gestor apropiado (en CentOS Stream 9 es `dnf`).
- Es un comando idempotente: si ya está, no cambia nada.

### Iniciar y habilitar el servicio chronyd
```bash
ansible centos -i inventory.ini -b -m service -a 'name=chronyd state=started enabled=yes'
```

- `enabled=yes`: el servicio debe iniciar al bootear.
- `state=started`: el estado debe ser iniciado.
- Idempotente: si ya está activo el servicio y habilitado, no hace nada.
---

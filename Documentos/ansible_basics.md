---
# Parte 5:
---

## 1) ¿Qué es Ansible? Mencione dos actividades que se puedan hacer con Ansible

**Definición.** Ansible es una herramienta de automatización de TI de código abierto que permite **declarar el estado deseado** de sistemas y aplicaciones mediante *playbooks* en YAML. Es **agentless** (no requiere instalar agentes en los hosts administrados) y, por defecto, usa **SSH/OpenSSH** para conectarse, con foco en **simplicidad** e **idempotencia** (si un sistema ya está en el estado pedido, no realiza cambios adicionales). [1][2]

**Actividades típicas (ejemplos):**
- **Gestión de configuración**: instalar paquetes, administrar archivos y usuarios, y asegurar servicios (por ejemplo, tener `chronyd` instalado y activo).
- **Orquestación y despliegue**: ejecutar pasos ordenados en múltiples servidores (desplegar una aplicacion web, reinicios coordinados, etc.). [1][2]

---

## 2) ¿Qué es un playbook de Ansible?

Un **playbook** es el archivo (YAML) donde definís **qué** tareas ejecutar (**módulos**) y **en qué hosts** (inventario/grupos). Es reutilizable, versionable y sirve tanto para **configuración** como para **orquestación** multi‑máquina. Cada playbook contiene una o más *plays*; cada *play* ejecuta una lista de **tareas**. [3][4]

---

## 3) ¿Qué información contiene un inventario de Ansible?

El **inventario** define los **hosts** administrados y cómo agruparlos; puede ser un archivo (INI/YAML), múltiples archivos o fuentes **dinámicas** (plugins para nubes, etc.). En el inventario también podés asociar **variables** a hosts/grupos y declarar **jerarquías** (grupos *children*). [5][6]

---

## 4) Explique qué es un módulo de Ansible y dé un ejemplo

Un **módulo** es una **unidad de código** que realiza una acción concreta (gestionar paquetes, servicios, archivos, usuarios, hablar con APIs cloud, etc.). Los módulos **aceptan parámetros** y **devuelven JSON** con resultados; pueden ejecutarse **ad‑hoc** o dentro de playbooks. [7][8]

---

## 5) ¿Qué ventajas tiene Ansible sobre otros métodos de automatización?

- **Agentless por defecto (menos componentes que mantener; usa OpenSSH).** Esto significa que Ansible no necesita que instales software adicional en los servidores que querés administrar. En lugar de eso, usa protocolos estándar como SSH para conectarse y ejecutar tareas remotamente. Esto simplifica mucho la gestión porque no hay que preocuparse por actualizar o mantener agentes en cada máquina, lo que reduce la complejidad y los puntos de falla en tu infraestructura. Además, al usar OpenSSH, se aprovechan las características de seguridad y confiabilidad que ya están bien establecidas en sistemas Unix/Linux. [2][3]

- **Simplicidad y legibilidad: YAML legible por humanos, curva de entrada baja.** Ansible utiliza archivos en formato YAML para definir las tareas y configuraciones, lo que hace que los playbooks sean fáciles de leer y entender incluso para personas que no son programadoras expertas. La sintaxis clara y estructurada permite que cualquiera pueda aprender a escribir y modificar automatizaciones rápidamente, lo que facilita la colaboración entre equipos y acelera el desarrollo de infraestructuras automatizadas sin necesidad de conocimientos profundos en programación. Esto también ayuda a mantener el código limpio y organizado. [3]

- **Idempotencia y previsibilidad: si el sistema ya está conforme al playbook, no cambia nada al re‑ejecutar.** Una de las características clave de Ansible es que sus tareas son idempotentes, lo que quiere decir que podés correr el mismo playbook varias veces sin que se produzcan cambios innecesarios si el sistema ya está en el estado deseado. Esto evita errores, cambios accidentales y facilita la gestión continua, porque siempre podés aplicar la automatización sin miedo a romper algo o a generar efectos secundarios inesperados. Además, esta previsibilidad permite que las operaciones sean reproducibles y auditable. [3][4]

- **Orquestación multi‑máquina y despliegues complejos (rolling updates, delegación, etc.).** Ansible no solo sirve para configurar máquinas individualmente, sino que también permite coordinar tareas en múltiples servidores al mismo tiempo, controlando el orden y la lógica de ejecución. Esto es fundamental para despliegues de aplicaciones, actualizaciones escalonadas (rolling updates) y operaciones que requieren pasos coordinados entre diferentes sistemas. La capacidad de delegar tareas a diferentes hosts y controlar flujos complejos facilita la automatización de procesos que antes requerían mucha intervención manual y scripts personalizados. [4]

- **Extensible: miles de módulos y plugins; soporta inventarios dinámicos y múltiples SOs/nubes.** Ansible cuenta con una gran comunidad y un ecosistema muy amplio que ofrece módulos para casi cualquier tarea imaginable, desde administrar servicios locales hasta interactuar con APIs de proveedores cloud o sistemas de terceros. Además, soporta inventarios dinámicos que se adaptan automáticamente según la infraestructura real (por ejemplo, detectando máquinas en la nube), y funciona en muchos sistemas operativos diferentes, lo que lo hace muy flexible y adaptable a entornos heterogéneos. Esta extensibilidad permite que puedas ampliar y personalizar Ansible según tus necesidades específicas. [4][6]

- **Integración con control de versiones: los playbooks se versionan y documentan cambios (auditable).** Como los playbooks son archivos de texto, podés almacenarlos en sistemas de control de versiones como Git. Esto permite llevar un registro detallado de todos los cambios realizados, quién los hizo y cuándo, facilitando la colaboración entre equipos y la auditoría de configuraciones. Además, esta práctica mejora la calidad y confiabilidad de la automatización al permitir revertir cambios y mantener un historial claro, algo que no es tan sencillo con métodos manuales o scripts sin control. [3]

---

## Referencias (fuentes primarias)

[1] **Introducción a Ansible** – Qué es, principios (agentless, simplicidad, idempotencia) y uso de playbooks.
https://docs.ansible.com/ansible/latest/getting_started/introduction.html

[2] **How Ansible Works (Red Hat)** – Diseño, uso de OpenSSH, enfoque de seguridad y simplicidad.
https://www.redhat.com/en/ansible-collaborative/how-ansible-works

[3] **Ansible Playbooks (Introducción)** – Qué son, sintaxis YAML, desired state e idempotencia.
https://docs.ansible.com/ansible/latest/playbook_guide/playbooks_intro.html

[4] **Working with playbooks** – Funciones de configuración/orquestación y capacidades avanzadas.
https://docs.ansible.com/ansible/latest/playbook_guide/playbooks.html

[5] **How to build your inventory** – Formatos, variables, grupos y ubicación por defecto; múltiples fuentes.
https://docs.ansible.com/ansible/latest/inventory_guide/intro_inventory.html

[6] **Working with dynamic inventory** – Plugins de inventario y fuentes externas (nubes, CMDB).
https://docs.ansible.com/ansible/latest/inventory_guide/intro_dynamic_inventory.html

[7] **Using Ansible modules and plugins** – Qué es un módulo y cómo se usa.
https://docs.ansible.com/ansible/latest/module_plugin_guide/index.html

[8] **Developing modules (visión técnica)** – Qué es un módulo, interfaz/argumentos y retorno JSON.
https://docs.ansible.com/ansible/latest/dev_guide/developing_modules_general.html

---

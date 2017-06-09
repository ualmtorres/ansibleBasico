# Ejemplos básicos de Ansible

Requisitos previos:

* Es necesario tener instalado Ansible en el nodo controlador.
* Las máquinas controladas sólo tienen que tener Python instalado. 
* Modifica el archivo `hosts.cfg` con el _inventario_ de servidores que quieres configurar

## Scaffolding

Usaremos una estructura como la descrita que incluirá:

* Archivo de configuración `ansible.cfg`
* Playbook `playbook.yml`
* Carpeta de roles (_grupos de operaciones_). Incluye una carpeta para cada rol 
** En cada rol: `/tasks/main.yml` incluye las operaciones del rol

```
ansible.cfg          # Opciones de configuración (p.e. arvhivo de inventario predetermi$
playbook.yml         # Receta a ejecutar (suele incluir hosts, superusuario y roles (su$
roles                # Directorio con roles (subrecetas) a ejecutar (p,e, desde playboo$
└── rol_1            # Nombre del rol (p.e. actualizar)
    └── tasks        # Carpeta que incluye las tareas del rol
        └── main.yml # Operaciones a ejecutar desde el playbook
...
└── rol_n            # Nombre del rol (p.e. config)
    └── tasks        # Carpeta que incluye las tareas del rol
        └── main.yml # Operaciones a ejecutar desde el playbook
```

### Ejemplo de `playbook.yml` 

```
- hosts: all
  become: true
  roles:
    - actualizar
```

### Ejemplo de rol (`roles/actualizar/tasks/mail.yml`)

```
- name: "Actualizar cache de paquetes"
  apt: 
    update_cache: yes

- name: "Actualizar paquetes"
  apt: 
    upgrade: dist
```

## Operaciones básicas


### Comprobar conexión con hosts a configurar

```
ansible -m ping all

# Indicando explícitamente el inventario de hosts 
ansible -i hosts.cfg -m ping all
```

### Ejecutar un comando desde el controlador

```
ansible -i hosts.cfg -m shell -a "df -h" all

ansible -i hosts.cfg -m shell -a "whoami" all

```

### Ejecutar un _playbook_

```
ansible-playbook playbook.yml
```

### Continuar un _playbook_ que ha fallado

Tras solucionar el error en el _playbook_, usar el nombre de la tarea por la que debe continuar en este comando

```
ansible-playbook playbookFile.yml --start-at-task="nombre de la tarea por la que continuar"
```

## Ejemplos

* [Update && Upgrade](roles/actualizar/tasks/main.yml)
* [Crear un directorio y descargar un archivo](./02descargar_logo.yml)
* [Instalación de un paquete](./03instalar_map.yml)
* [Instalación de varios paquetes](./04instalar_varios.yml)
* [Operaciones con archivos (copia, permisos, descomprimir, añadir texto, ...)](./05files.yml)
* [Git](./06git.yml)


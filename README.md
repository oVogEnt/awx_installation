
# Playbook de Instalación de AWX

## Introducción

Este proyecto proporciona un playbook de Ansible para instalar y configurar AWX.

## Requisitos

- Ansible 2.9 o superior
- Acceso a una máquina CentOS o RHEL con privilegios de root
- Acceso a Internet para descargar los paquetes y software necesarios

## Instalación

1. **Clonar el repositorio**:
   ```sh
   git clone https://github.com/oVogEnt/awx_installation.git
   cd awx_installation
   ```

2. **Instalar las collections necesarias de Ansible**:
   ```sh
   ansible-galaxy collection install -r collections/requirements.yml
   ```

3. **Ejecutar el playbook**:
   ```sh
   ansible-playbook setup_awx.yml -i inventory/hosts -u root -kK
   ```

   - `-i inventory/hosts`: Especifica el archivo de inventario.
   - `-u root`:  Usa el usuario root para las conexiones SSH. También puede usar cualquier otro usuario con derechos de sudo.
   - `-k`: Solicita la contraseña SSH.
   - `-K`: Solicita la contraseña sudo.

## Resumen del Playbook

El playbook realiza los siguientes pasos:

1. **Pre-tareas**:
   - Instalar los paquetes necesarios (por ejemplo, `epel-release`, `git`, `make`, `firewalld`, `podman`, `python3-pip`).

2. **Tareas**:
   - Asegurarse de que `firewalld` esté instalado, iniciado y configurado para usar `iptables`.
   - Descargar e instalar `k3s` (una distribución ligera de Kubernetes).
   - Esperar a que `k3s` despliegue los servicios predeterminados.
   - Instalar la biblioteca de Python para Kubernetes.
   - Copiar el `kubeconfig` al directorio home.
   - Obtener la última versión del AWX Operator desde GitHub si no está ya definida.
   - Clonar el repositorio de AWX Operator.
   - Crear un namespace de Kubernetes para AWX.
   - Crear y aplicar el archivo `kustomization.yaml` para desplegar el AWX Operator.
   - Crear y aplicar el archivo `awx.yml` para desplegar la instancia de AWX.
   - Esperar a que la instancia de AWX esté lista.
   - Recuperar la contraseña de administrador de AWX y el NodePort del servicio.
   - Guardar los detalles de acceso a AWX en un archivo (`/tmp/awx_access_details.txt`).

## Detalles de Acceso

Después de ejecutar el playbook, la contraseña de administrador de AWX y la URL se guardarán en `/tmp/awx_access_details.txt`.

Inicie sesión con el usuario `admin`
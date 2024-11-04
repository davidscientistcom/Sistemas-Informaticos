### 2. Conceptos Básicos

En esta sección, nos adentraremos en los conceptos clave de Vagrant, que forman la base para comprender cómo se crean y gestionan máquinas virtuales en esta herramienta. Estos conceptos son importantes para entender cómo configurar, personalizar y automatizar las máquinas virtuales de manera eficaz.

#### 2.1 Conceptos Clave

- **Boxes (Cajas)**: Una "box" es una imagen base de una máquina virtual, que contiene el sistema operativo y la configuración inicial. Las boxes actúan como plantilla para las máquinas virtuales. Vagrant utiliza estas imágenes base y, a partir de ellas, genera entornos que pueden personalizarse según las necesidades del usuario. Existen boxes para diversas distribuciones de Linux y versiones de Windows, que pueden descargarse desde el repositorio de Vagrant en [https://app.vagrantup.com/boxes/search](https://app.vagrantup.com/boxes/search).

- **Vagrantfile**: Es el archivo de configuración principal de un proyecto en Vagrant. Este archivo define cómo se crea y configura la máquina virtual, desde la selección de la box hasta las configuraciones de red, la asignación de recursos, la provisión, y cualquier otra personalización requerida.

- **Providers (Proveedores)**: Vagrant soporta múltiples proveedores de virtualización. El proveedor es la plataforma que Vagrant utiliza para ejecutar la máquina virtual. VirtualBox es el proveedor más común, pero también se pueden utilizar otros como VMware, Hyper-V y proveedores en la nube (AWS, Google Cloud). La elección del proveedor determina la compatibilidad y las capacidades de virtualización.

- **Provisioning (Provisión)**: Es el proceso mediante el cual se instala y configura software en la máquina virtual. Aunque aquí no vamos a profundizar en la provisión (que se verá con Ansible), incluiremos un ejemplo básico de configuración de SSH para conexión desde Windows mediante un bloque `provision` ejecutado "once".

#### 2.2 Primeros Pasos: Creación de un Vagrantfile

El **Vagrantfile** es la pieza central en Vagrant. Este archivo define todas las configuraciones necesarias para crear y administrar una máquina virtual. Para iniciar un nuevo proyecto en Vagrant, sigue estos pasos básicos:

1. Crea un directorio de proyecto:
   ```bash
   mkdir mi_proyecto_vagrant
   cd mi_proyecto_vagrant
   ```

2. Inicializa un nuevo Vagrantfile:
   ```bash
   vagrant init
   ```
   Este comando genera un archivo `Vagrantfile` con la configuración mínima, el cual puedes personalizar para ajustar la máquina virtual según tus necesidades.

A continuación, veremos un ejemplo de un **Vagrantfile** básico que utiliza una box de Ubuntu.

**Ejemplo de Vagrantfile básico**

```ruby
Vagrant.configure("2") do |config|
  config.vm.box = "ubuntu/bionic64"
end
```

Este archivo define una máquina virtual basada en la box `ubuntu/bionic64` (Ubuntu 18.04). A medida que avancemos en el capítulo, añadiremos configuraciones adicionales para personalizar la máquina.

#### 2.3 Introducción a la CLI de Vagrant

La **Interfaz de Línea de Comandos (CLI)** de Vagrant permite gestionar las máquinas virtuales a través de una serie de comandos intuitivos. Los comandos más usados en Vagrant son:

- **vagrant up**: Crea y enciende la máquina virtual.
- **vagrant halt**: Apaga la máquina virtual sin eliminarla.
- **vagrant destroy**: Destruye la máquina virtual y borra todos sus datos.
- **vagrant reload**: Reinicia la máquina virtual y aplica los cambios en el Vagrantfile.

Veamos un ejemplo práctico:

1. Inicia la máquina virtual:
   ```bash
   vagrant up
   ```

2. Apaga la máquina virtual:
   ```bash
   vagrant halt
   ```

3. Reinicia la máquina virtual para aplicar cambios:
   ```bash
   vagrant reload
   ```

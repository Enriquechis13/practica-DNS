# Práctica DNS

## 1. Instrucciones para ejecutar

Este repositorio contiene los archivos necesarios para la creación de máquinas virtuales utilizando Vagrant.

## Instrucciones para ejecutar

1. Clona este repositorio:

2. Inicia las máquinas virtuales con Vagrant:

3. Verifica que Apache se haya instalado correctamente accediendo a la dirección IP de la máquina virtual.

## Transferencia de Zona

![Imagen vagrant](imagenes/vagrant1.png)

## Requisitos

- [Vagrant](https://www.vagrantup.com/downloads)
- [VirtualBox](https://www.virtualbox.org/wiki/Downloads)

## Licencia

Este proyecto está bajo la licencia MIT.

## 2. Datos del problema

### 2.1 Red
Las dos máquinas virtuales están configuradas en la red privada `192.168.57.0/24`. A cada máquina se le asigna una IP fija dentro de este rango, y se les configura un nombre FQDN (Fully Qualified Domain Name).

### 2.2 Equipos

| Equipo               | FQDN                | IP              |
|----------------------|---------------------|-----------------|
| Debian texto         | venus.sistema.test   | 192.168.57.102  |
| Debian texto         | tierra.sistema.test  | 192.168.57.103  |

### Cómo levantar las máquinas

Para levantar las máquinas, sigue estos pasos:

1. Clona este repositorio:
   ```bash
   git clone https://github.com/Enriquechis13/practica-DNS.git
   cd practica-DNS
    ```
## Inicio Venus y Tierra

![Imagen vagrant](imagenes/vagrant2.png)
![Imagen vagrant](imagenes/vagrant3.png)

## 3. Datos del DNS

El proyecto está basado en la configuración de un servidor DNS usando **Bind9**. Se ha configurado una arquitectura maestro-esclavo:

- **Tierra (maestro)**: Controla la zona directa e inversa.
- **Venus (esclavo)**: Sincroniza sus registros DNS con `tierra`.

### 3.1. Configuraciones principales:

1. **IPv4 activado solamente:** En la configuración de Bind, se fuerza el uso de solo IPv4 añadiendo `OPTIONS="-u bind -4"` en `/etc/default/named`.

2. **DNSSEC habilitado:** La opción `dnssec-validation` está establecida en `yes`.

3. **Configuración de ACL para Consultas Recursivas**

Para permitir que el servidor DNS acepte consultas recursivas solo desde ciertas redes, sigue estos pasos:

 Abre el archivo de configuración de Bind9:
```bash
   sudo nano /etc/bind/named.conf.options

   acl "allowed_clients" {
    127.0.0.0/8;           // Permitir localhost
    192.168.57.0/24;       // Permitir la red privada
    };

    options {
        directory "/var/cache/bind";

        dnssec-validation yes; // Validación DNSSEC habilitada
        recursion yes;         // Habilitar la recursión
        allow-recursion { "allowed_clients"; }; // Especificar la ACL

        auth-nxdomain no;      // Conformidad con RFC8482
        listen-on-v6 { any; }; // Asegúrate de que IPv6 no esté habilitado si solo usas IPv4.
    };
```

4. **Servidor maestro:** `tierra.sistema.test` tiene autoridad sobre la zona directa e inversa.
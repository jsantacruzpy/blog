---
date: 2025-10-17
authors: ["Jose Santacruz"]
draft: true
title: 'Configurar Archive Cisco Ios'
tags: ["networking", "cisco"]
---

## Descripción
En esta guía veremos cómo **configurar copias automáticas de seguridad** de la configuración de equipos Cisco IOS utilizando la función **`archive`**, almacenando los archivos en un **servidor FTP remoto**.  
Esta práctica permite mantener un historial de configuraciones y aplicar rollbacks en caso de ser necesario.

---
## Requerimientos
- Un **servidor FTP** previamente configurado y accesible desde el dispositivo Cisco.  
- Credenciales de usuario con permisos de escritura en el repositorio FTP.
## Detalles 
- Ftp server: 192.168.100.10
---

## 1. Configurar credenciales para autenticación FTP
Primero se definen las credenciales que el dispositivo utilizará para conectarse al servidor FTP:

```bash
Router(config)# ip ftp username <usuario>
Router(config)# ip ftp password <contraseña>
```
Sugerencia: Es recomendable usar un usuario dedicado exclusivamente para el respaldo, con permisos limitados al directorio de backup.

## 2. Configurar el servicio Archive
El comando archive en Cisco IOS permite crear y mantener versiones automáticas de la configuración.
En este caso, configuraremos el envío de los backups al servidor FTP y guardaremos los archivos con el formato hostname-fecha-version

```bash
Router(config)# archive
Router(config-archive)# path ftp://192.168.100.10/$h-$t
Router(config-archive)# maximum 10
Router(config-archive)# write-memory
Router(config-archive)# exit
```

## 3. Verificar y aplicar backups
Una vez configurado el archive, podemos revisar los backups existentes y restaurar la configuración deseada:
```bash
# Ver backups existentes
Router# show archive

# Restaurar un backup específico desde FTP
Router# configure replace ftp://192.168.100.10/<nombre_del_backup>
```

+++
date = '2025-10-17T17:07:35-03:00'
draft = true
title = 'Configurar Archive Cisco Ios'
+++
## Descripción

En esta guía veremos cómo **configurar copias automáticas de seguridad (backups)** de la configuración de equipos Cisco IOS utilizando la función **`archive`**, almacenando los archivos en un **servidor FTP remoto**.  
Esta práctica permite mantener un historial de configuraciones y simplificar la recuperación ante errores o cambios no deseados.

---

## Requerimientos

- Un **servidor FTP** previamente configurado y accesible desde el dispositivo Cisco.  
- Credenciales de usuario con permisos de escritura en el repositorio FTP.

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
En este caso, configuraremos el envío de los backups al servidor FTP.

```bash
Router(config)# archive
Router(config-archive)# path ftp://192.168.150.8/$h-$t
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
Router# configure replace ftp://192.168.150.8/<nombre_del_backup>
```

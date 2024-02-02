# 10-Aprovisionar Velero TKGS Tanzu
En el siguiente video explico como desplegar la solución de Velero en TKGS en vSphere with Tanzu como solución de Backup para Kubernetes: https://youtu.be/H-aDyVJtqKw


Este manual esta preparado para la versión 1.24.11 de Kubernetes que corre sobre ESXi versión 7, y la versión de Velero es la 1.9.7.

Descargar los binarios desde el portal de VMWARE https://customerconnect.vmware.com. Hay que hacer login para descargar paquetes.

Filtrar por "Tanzu Kubernetes Grid Download".

![imagen](https://github.com/ticveintitres/tanzu/assets/153328087/9ce33729-cec9-4847-adf8-2834d2710617)
![imagen](https://github.com/ticveintitres/tanzu/assets/153328087/b2eb13e3-4ad9-4152-8ff2-7724a4e85b2f)
![imagen](https://github.com/ticveintitres/tanzu/assets/153328087/21508c85-d9cf-452d-891d-b056a8adb9cb)
![imagen](https://github.com/ticveintitres/tanzu/assets/153328087/3de9c2bc-52d7-4c12-a8ab-c5ef3f3115e5)

Dirigirse a minIO. Crear un bucket y un Access Key

![imagen](https://github.com/ticveintitres/tanzu/assets/153328087/bd933b26-64b6-474a-960c-37d5ead3aea0)
![imagen](https://github.com/ticveintitres/tanzu/assets/153328087/8dfb4ae1-c029-406a-9355-2c6d55d109e3)

Subimos el fichero en la maquina de bootstrap, en mi caso Linux Debian 12
Descomprimir el fichero, cambiar los permisos a ejecución, mover a la carpeta de binarios y activar el Bash completion

```
gunzip velero-linux-v1.9.7+vmware.1.gz
chmod +x velero-linux-v1.9.7+vmware.1
mv velero-linux-v1.9.7+vmware.1 /usr/local/bin/velero
echo 'source <(velero completion bash)' >>~/.bashrc
```

Crear un fichero llamado secret con el AccessKey como este ejemplo:

```
[default]
aws_access_key_id = EyoZMQ6y3QDfAoAn
aws_secret_access_key = qZIBchiXAtpNvjHia7tbsBASWN5GO5xP
```

Conectarse al contexto del Espacio de nombres donde esté alojado el cluster

```
kubectl config use-context nombrecontexto
```

Instalar velero utilizando el comando "velero" como muestro mas abajo. 

```
velero install \
--provider aws \
--plugins velero/velero-plugin-for-aws:latest \
--bucket velero \
--secret-file secret \
--use-volume-snapshots=false \
--use-restic \
--backup-location-config region=ticveintitres-local,s3ForcePathStyle="true",s3Url=https://alminio01.ticveintitres.local:9000 \
--cacert alminio01-ticveintitres-local.pem
```

Desglosando el comando junto a sus parámetros.
- Provider utilizo aws compatible con minIO
- Plugins uso el contenedor de AWS compatible con minIO. Este contenedor se añade junto al de Velero dentro del propio Pod de Velero.
- Bucket, el nombre del Bucket en minIO
- Secret File es el fichero con los credenciales de acceso para el Bucket.
- Use Volume Snapshots, en "false" ya que usaremos Restic para los backup de PV
- Use restic, para indicar que queremos usar Restic
- Backup Location Config, hay que definir la region, que en mi caso es "ticveintitres-local" porque así lo defini en minIO al instalarlo, añadir la URL de minIO
- CACERT lo añado porque minIO lo tengo configurado con un certificado SSL, añado el certificado de la página en formato PEM.

Una vez ejecutado esperamos a que levante el pod de Velero. Aquí se pueden hacer varios pasos mientras despliega todo.  Revisar los logs, revisar el backup-location entre otros.


```
kubectl logs deployment/velero -n velero
velero backup-location get
```
El resultado de "velero backup-location get" debe mostrarnos algo así, con estado PHASE "Available"

```
NAME      PROVIDER   BUCKET/PREFIX   PHASE       LAST VALIDATED                  ACCESS MODE   DEFAULT
default   aws        velero          Available   2024-01-28 10:33:15 +0100 CET   ReadWrite     true
```

Si todo salio correctamente, los pods están corriendo y esta "Available" el bucket de destino, Velero ya estaría aprovisionado. Si falla algo hay que revisar los logs.

https://www.youtube.com/@ticveintitres

Creado con ❤️ por Alejandro Martínez Capilla

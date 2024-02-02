# 11-Backup&Restore Velero TKGS Tanzu
En el siguiente video explico como backup y restores con Velero en TKGS en vSphere with Tanzu como solución de Backup para Kubernetes: 


Este manual esta preparado para la versión 1.24.11 de Kubernetes que corre sobre ESXi versión 7, y la versión de Velero es la 1.9.7.

Para esta prueba he dejado un fichero llamado prueba.yaml, que instala un Wordpress en el cluster, que tiene dos persistent volumes, uno para Wordpress y otro para MySQL.

```

Conectarse al contexto del Espacio de nombres donde esté alojado el cluster

```
kubectl config use-context nombrecontexto
```

Instalar la aplicación de Wordpres con el fichero de prueba. 

```
kubectl apply -f prueba.yaml
```

Revisar que IP le ha dado AVI NSX a la aplicación

```
kubectl get service -n wordpress
```

Acceder via web, configurar Wordpress y escribir una entrada a modo de prueba.

---aqui imagen---

Realizar el backup de Wordpress con Velero. Para ello la forma más básica es lanzando un backup a demanda

```
velero create backup prueba-wordpress --include-namespaces wordpress --default-volumes-to-restic=true
```

Revisar el estado del backup , y comprobar que este completado y sin errores
```
velero get backup
```

Ahora para comprobar que el backup se ha realizado correctamente y se han guardado los datos nuevos, borrar la aplicación de Wordpress de prueba.

```
kubectl delete -f prueba.yaml
```

Una vez borrada del todo ( comprobar que no exista el Namespace y los Persistent Volumes), se hace un Restore desde Velero
```
velero restore create prueba-restore --from-backup prueba-wordpress
```

Se comprueba que el trabajo de Restore de Velero ha finalizado.
```
velero get restore
```

Se comprueba que se han desplegado todos los objetos de Wordpress
```
kubectl get all -n wordpress
kubectl get pv
```

Por último ingresar via web al Wordpress y comprobar que todo se ha recuperado.

--aqui imagen---

Si todo salio correctamente, se ha recuperado la aplicación de Wordpress desde una copia de seguridad con Velero

https://www.youtube.com/@ticveintitres

Creado con ❤️ por Alejandro Martínez Capilla

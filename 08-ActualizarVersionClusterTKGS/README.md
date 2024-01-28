# 08-ActualizarVersionClusterTKGS
En el siguiente video explico como actualizar un cluster de TKGS en vSphere with Tanzu: https://youtu.be/I7S1PyGVi7o?feature=shared


En un terminal, conectarse al contexto del Espacio de nombres donde esté alojado el cluster

```
kubectl config use-context nombrecontexto
```

Enumere las versiones disponibles de Tanzu Kubernetes filtrando por "True"

```
kubectl get tkr | grep True
```

Ejecute el siguiente comando para editar el manifiesto del clúster.

```
kubectl edit tanzukubernetescluster/CLUSTER-NAME
```

Dependiendo de la versión:

En Alpha1 , cambiar el nombre en el campo "distribution"

En Alpha2 , cambiar el nombre en el campo "tkr"

Guardar los cambios y esperar a que se desplieguen las nuevas máquinas virtuales y las viejas se borren.

Téneis en el siguiente fichero un ejemplo.

https://www.youtube.com/@ticveintitres

Creado con ❤️ por Alejandro Martínez Capilla

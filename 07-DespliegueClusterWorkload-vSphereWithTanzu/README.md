# 07-DespligueClusterWorkload-vSphereWithTanzu
En el siguiente video explico como desplegar un cluster de Workload en vSphere with Tanzu:


En un terminal, conectese al cluster de Tanzu. Revise los contextos y conectese al contexto deseado.

```
kubectl config use-context nombrecontexto
```

Enumere los enlaces de clase de máquina virtual que están disponibles en el espacio de nombres de vSphere de destino.

```
kubectl get virtualmachineclassbindings
```

Solo puede utilizar las clases de máquina virtual que están enlazadas al espacio de nombres de destino. 
Si no ve ninguna clase de máquina virtual, compruebe que espacio de nombres de vSphere tenga las clases de máquina virtual predeterminadas agregadas.

Lo siguiente es obtener las clases de almacenamiento de volumen persistente disponibles.

```
kubectl describe storageclasses
```

Lista las versiones de Tanzu Kubernetes disponibles:
Puede utilizar cualquiera de los siguientes comandos para realizar esta operación:

```
kubectl get tkr

kubectl get tanzukubernetesreleases
```

Solo puede utilizar esas versiones que devuelve este comando. Si no ve ninguna versión o las versiones que desea, compruebe que haya sincronizado los archivos OVA deseados con la biblioteca de contenido.
Si no muestra nada, entrar al vCenter desde la VAMI y reiniciar el servicio de ContentLibrary. Si eso no solventa nada, crear otra Content Library

Por último , crear el archivo YAML para aprovisionar un clúster de Tanzu Kubernetes utilizando los siguientes ficheros , desde algo básico a un cluster mas complejo con diferentes configuraciones.

https://www.youtube.com/@ticveintitres

Creado con ❤️ por Alejandro Martínez Capilla

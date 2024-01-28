# 09-Mi Primera APP en TKGS Tanzu
En el siguiente video explico como desplegar una apliación sencilla en TKGS en vSphere with Tanzu: https://youtu.be/PIC_AecmVsI?feature=shared


En un terminal, conectarse al contexto del Espacio de nombres donde esté alojado el cluster

```
kubectl config use-context nombrecontexto
```

Desplegar una aplicación sencilla, en este caso un servidor apache sencillo

```
kubectl run --image=httpd:latest test
```

Ejecute el siguiente comando para ver el POD levantado en estado "Running"

```
kubectl get pods
```

Para exponer el pod como LoadBalancer ejecutar este comando:

```
kubectl expose pods test --type=LoadBalancer --port=80
```

Revisando el Balanceador de carga de AVI NSX se ha levantado un servicio con una IP:

![imagen](https://github.com/ticveintitres/tanzu/assets/153328087/b337cedf-3ac8-483b-9566-12afac62de37)

Navegando a la URL se puede ver que Apache esta levantado

![imagen](https://github.com/ticveintitres/tanzu/assets/153328087/447a55bd-3988-43ca-9b66-21e8067aefd5)

https://www.youtube.com/@ticveintitres

Creado con ❤️ por Alejandro Martínez Capilla

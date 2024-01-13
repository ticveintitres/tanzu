# 09-Mi Primera APP en TKGS Tanzu
En el siguiente video explico como desplegar una apliación sencilla en TKGS en vSphere with Tanzu:


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

![Alt text](image.png)

Navegando a la URL se puede ver que Apache esta levantado

![Alt text](image-1.png)

https://www.youtube.com/@ticveintitres

Creado con ❤️ por Alejandro Martínez Capilla

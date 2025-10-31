# Динамическое масштабирование контейнеров

Start minikube cluster

```
minikube start
minikube addons enable metrics-server
```

Create namespace and deploy application

```
minikube image load ghcr.io/yandex-practicum/scaletestapp:latest --overwrite
kubectl create namespace scaletest
kubectl apply -f scaletestapp.yaml -n scaletest
minikube service scaletestapp-service --url -n scaletest
```

Add HPA

```
kubectl apply -f scaletestapp-hpa.yaml -n scaletest
kubectl get hpa -n scaletest
```

Monitoring
```
kubectl get pods -n scaletest
```

Testing:

Sreenshots before:
 - [Kubernetes dashboard](./dashboard1.png)
 - [HPA](./hpa1.png)
 - [Deployment](./deploy1.png)

Screenshots after:
 - [Locust](./Locust.png)
 - [Kubernetes dashboard](./dashboard2.png)
 - [HPA](./hpa2.png)
 - [Deployment](./deploy2.png)
 - [Pods](./pods.png)

Clear

```
kubectl delete deployment scaletestapp -n scaletest
kubectl delete namespace scaletest
```

Fix problems with dns

```
# зайти в ноду
minikube ssh
# (внутри) временно подменить resolv.conf
echo -e "nameserver 8.8.8.8\nnameserver 1.1.1.1" | sudo tee /etc/resolv.conf
```
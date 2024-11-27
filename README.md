# production-deployment.yaml generation

## 1. Create prod env, configmaps, and secret
```
k create ns production
```

```
kubectl create configmap app-config \
  --from-literal=APP_ENV=production \
  --namespace=production
```

```
kubectl create secret generic app-secret \
  --from-literal=DB_PASSWORD=****************\
  --namespace=production
```

## 2. Generate the `app-deployment-template.yaml`
```
vim app-deployment-template.yaml
```

## 3. Generate [`production-deployment.yaml`](https://github.com/jgzhernandez/FECP_envconfig_activity/blob/main/production-deployment.yaml) from `app-deployment-template.yaml`
```
sed -e 's/PLACEHOLDER_NAMESPACE/production/' \
> -e 's/replicas: .*$/replicas: 3/' app-deployment-template.yaml > production-deployment.yaml
```

## 4. Apply template
```
k apply -f production-deployment.yaml
```

## 5. Expose env
```
kubectl expose deployment nginx-app \
  --type=NodePort \
  --name=nginx-service \
  --port=80 \
  --target-port=80 \
  --namespace=production
```

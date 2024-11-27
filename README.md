# production-deployment.yaml generation

## 1. Generate the `app-deployment-template.yaml`
```
vim app-deployment-template.yaml
```

## 2. Generate [`production-deployment.yaml`](https://github.com/jgzhernandez/FECP_envconfig_activity/blob/main/production-deployment.yaml) from `app-deployment-template.yaml`
```
sed -e 's/PLACEHOLDER_NAMESPACE/production/' \
> -e 's/replicas: .*$/replicas: 3/' app-deployment-template.yaml > production-deployment.yaml
```

## 3. Apply template
```
k apply -f production-deployment.yaml
```

## Expose env
```
kubectl expose deployment nginx-app \
  --type=NodePort \
  --name=nginx-service \
  --port=80 \
  --target-port=80 \
  --namespace=production
```

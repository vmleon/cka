# Certified Kubernetes Administrator Guide

## RBAC

| Subject         | API Resource | Verbs  |
| --------------- | ------------ | ------ |
| Groups          | Config Map   | Create |
| Users           | Pod          | List   |
| ServiceAccounts | Deployment   | Watch  |
|                 | Node         | Delete |
|                 | ...          | ...    |

Authentication strategy

- X.509 client certificate  Uses an OpenSSL client certificate to authenticate
- Base Authentication       Uses username and password to authenticate
- Bearer tokens             Uses OpenID (over OAuth2) or webhooks to authenticate

```
mkdir cert && cd cert
```

```
openssl genrsa -out johndoe.key 2048
```

```
openssl req -new -key johndoe.key -out johndoe.csr -subj "/CN=johndoe/O=cka-study-group"
```

Sign the CSR with Kubernetes cluster CA, usually on `/etc/kubernetes/pki`.
```
openssl x509 -req -in johndoe.csr -CA /.minikube/ca.crt -CAkey /.minikube/ca.key -CAcreateserial -out johndoe.crt -days 364
```


```
kubectl config set-credentials johndoe \
  --client-certificate=johndoe.crt --client-key=johndoe.key
```

```
kubectl config set-context johndoe-context --cluster=minikube \
  --user=johndoe
```

```
kubectl config use-context johndoe-context
```

```
kubectl config current-context
```


### Service Account

```
kubectl create serviceaccount build-bot
```

```
kubectl get serviceaccounts
```

```
kubectl describe serviceaccounts build-bot
```

```
kubectl get secrets
```

```yaml
apiVersion: v1
kind: Pod
metadata:
  name: build-observer
spec:
  serviceAccountName: build-bot
```
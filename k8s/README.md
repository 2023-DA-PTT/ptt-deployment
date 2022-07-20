# Performance Testing Tool k8s

## Setup

### Setting your ghcr credentials

base64 encode following String by replacing the `<token>` and `<username>` with your github-credentials.

```
<username>:<token>
```

Copy the base64-encoded string and replace it with `<placeholder>`

```json
{"auths":{"ghcr.io":{"auth":"<placeholder>"}}}
```

Encode this whole String again and you have your ghcr-json-key.

Create a file called `k8s-ghcr-credentials.yml` and paste following lines by replacing the `<ghcr-json-key>` with your base64-encoded json key.

```yml
kind: Secret
type: kubernetes.io/dockerconfigjson
apiVersion: v1
metadata:
  name: dockerconfigjson-github-com
  namespace: ptt
  labels:
    app: frontend-git-pat
data:
  .dockerconfigjson: <ghcr-json-key>
```

Finally, run `kubectl create namespace ptt && kubectl apply -f k8s-ghcr-credentials.yml`

### Setting up TLS Encryption keys

Run following command to download letsencrypt for traefik ingress controller:

`kubectl apply -f https://github.com/jetstack/cert-manager/releases/download/v1.8.2/cert-manager.yaml `

Create a file called `k8s-cert.yml` and replace `<email>` with your email

```yml
apiVersion: cert-manager.io/v1
kind: ClusterIssuer
metadata:
  name: letsencrypt
  namespace: cert-manager
spec:
  acme:
    server: https://acme-v02.api.letsencrypt.org/directory
    email: <email>
    privateKeySecretRef:
      name: letsencrypt
    solvers:
    - http01:
        ingress:
          class: nginx
```

Execute `kubectl apply -f k8s-cert.yml`

## Deployment

Deploy the k8s Performance Testing Tool by doing `kubectl apply -f frontend/k8s && kubectl apply -f backend/k8s`
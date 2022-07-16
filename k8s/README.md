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
  labels:
    app: frontend-git-pat
data:
  .dockerconfigjson: <ghcr-json-key>
```

Finally, run `kubectl apply -f k8s-ghcr-credentials.yml`

## Deployment

Deploy the k8s Performance Testing Tool by doing `kubectl apply -f k8s-prod.yml`
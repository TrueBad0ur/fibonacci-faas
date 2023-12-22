## Setup OpenFaaS via helm
```bash
helm repo add openfaas https://openfaas.github.io/faas-netes
helm install my-openfaas openfaas/openfaas --version 14.2.7 -f openfaas.yml

functionNamespace: default
ingress:
  enabled: true

Get password for admin:
echo $(kubectl -n default get secret basic-auth -o jsonpath="{.data.basic-auth-password}" | base64 --decode)
```

## Build and deploy function
```bash
#faas-cli new --lang python hello-python
faas-cli build -f ./fibonacci.yml
docker push image
faas-cli deploy -f ./fibonacci.yml
faas-cli login --password <PASSWORD> --gateway http://127.0.0.1:33333
```

Access web UI - http://127.0.0.1:33333/ui

33333 - your forwarded port
## Testing
```bash
curl 127.0.0.1:33333/function/fibonacci -d 50
12586269025

curl 127.0.0.1:33333/function/fibonacci -d 100
354224848179261915075

faas-cli remove fibonacci --gateway 127.0.0.1:33333
```

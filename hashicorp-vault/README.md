# Hashicorp Vault Helm Deployment

Running Hashicorp Vault
<br>

Documentation: 
[Hashicorp Vault](https://developer.hashicorp.com/vault/docs?ajs_aid=1b19504f-2853-4f52-8a44-54ba9b57a916&product_intent=vault)

# Deploying Vault

### Steps 

First, Deploy vault to your cluster
```bash
helm repo add hashicorp https://helm.releases.hashicorp.com
helm repo update
helm upgrade -i vault hashicorp/vault -f override-values.yaml
```

Second, Unseal and init Vault server
```bash
kubectl exec -it svc/vault -- vault operator init
# -- This will print a set of 5 unseal keys and a root token. 
# -- SAVE THESE KEYS
kubectl exec -it svc/vault -- sh
vault operator unseal # Use 3 of the 5 keys and run this command three times
vault login ROOT_TOKEN # Replace ROOT_TOKEN with the root token from init
exit
```

Afterwords, you can connect to the ui via port-forward
```bash
kubectl port-forward svc/vault -n default 8200:8200
```
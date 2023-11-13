# Kube Promethus Stack Helm Chart w override values


Add kube-promethus-stack repo
```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
```

Apply chart to your current cluster 
```bash
helm upgrade -i prometheus prometheus-community/kube-prometheus-stack -f override-values.yaml
```
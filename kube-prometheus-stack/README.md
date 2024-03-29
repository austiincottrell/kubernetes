# Kubernetes GCP Self Managed Kube Prometheus Deployment

Running kube-prometheus-stack
<br>

# Helpful Tips / Best Practices

Commands to run to deploy a self managed prometheus deployment in GKE.
```bash
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
helm upgrade -i prometheus prometheus-community/kube-prometheus-stack -f override-values.yaml
```

[GCP Managed Service for Prometheus Docs](https://cloud.google.com/stackdriver/docs/managed-prometheus)

# Deploying Kube Prometheus 

Step 1) You will need to create a SA (service account) within GCP. This will give permissions for prometheus to send metrics to Cloud Montioring inside GCP. 

### Permissions needed on the SA

```

  roles/monitoring.metricWriter

```

^^^ You will want to add this to the SA when created OR your terraform deployment of your SA. Depending on your security practices and automation I would recommend using terraform/automation in a production env. If you are testing/POC then create the SA in the GCP console.

For now: You will be able to annotate the SA via the helm chart which will take one manual step out. I'll point out the location of where to do that in the dev-env.yaml file located in the helm-override. 

Step 2) After you add the annotation the only other command you will also need to run prior or after deploying the helm chart is to allow the kubernetes deployment permission to use the GCP SA. Please update the name of the deployment and the namespace to the corresponding names you use.

For Example: gmp-test is the name of the helm deployment and default is the name of the namespace used. 

```bash

gcloud iam service-accounts add-iam-policy-binding \
  --role roles/iam.workloadIdentityUser \
  --member "serviceAccount:PROJECT_ID.svc.id.goog[gmp-test/default]" \
  gmp-test-sa@PROJECT_ID.iam.gserviceaccount.com \

```

Step 3) Validate everything is working correctly. Running the <em>kubectl get pods -n default</em> will show you the pods deployed in that namespace. You want to see all the pods running without any crashloopbackoff's happening. If you see those then come threw your configuration and see if anything is out of place.. indentation is common error.

You will also want to go into the GCP console and navigate to the Cloud Monitoring UI. Once you are in the Cloud Monitoring section press the managed prometheus on the left side of the page to navigate to a query bar. Inside the query bar run the query UP. This will show if all the prometheus node-exporter,prometheus,and prometheus operator is up.

Cheers! Happy Configuring!
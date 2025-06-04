Bunch-of-stuff that I use on my homelab.

To initialize the cluster, cd into argo/argo-cd and run:
```
helm install argo-cd . -n argo-cd --create-namespace
```
This is so argo-cd can be used to deploy the other apps - and manage itself.

Then, apply the applicationset to deploy the rest of the apps:
```
kubectl apply -f manifests/appset/appset.yaml
```

---

Notes:

External Secrets Operator uses a secret in-cluster to retrieve stuff from AWS SSM. 

To create the secret, after the initial Argo deploy, run:

```
kubectl create secret generic awssm-secret \
  --from-literal=access-key="YOUR_AWS_ACCESS_KEY_ID" \
  --from-literal=secret-access-key="YOUR_AWS_SECRET_ACCESS_KEY" \
  -n external-secrets-operator
```

STRRL will require a Cloudflare account and a domain name. The API Key is stored in a secret in the cluster. It is available via the CustomResource ExternalSecret.


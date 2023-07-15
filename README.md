# kcd-multi-tenancy-vcluster-crossplane

## ArgoCD 
```bash
helm install argocd-system -n argocd-system --create-namespace argocd/argo-cd
kubectl -n argocd-system get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```
## Crossplane
```bash
kubectl apply -f apps/crossplane/application.yaml
kubectl apply -f configs/crossplane/providers/gcp/config/provider.yaml
kubectl create secret generic gcp-secret -n crossplane-system --from-file=creds=./gcp-sa.json
kubectl apply -f configs/crossplane/providers/gcp/config/provider-config.yaml
```

# Tenant
```bash
kubectl apply -f projects/tenants.yaml
kubecl apply -f tenants/tenant-2/vcluster/vcluster.yaml
vcluster list
vcluster connect vcluster-tenant-2 -n tenant-2
```
# Cloud Resources
```bash
kubectl apply -f tenants/tenant-2/apps/gcp.yaml
```
# Sample Application
```bash
kubectl -n demo create secret generic postgres-creds  --from-literal=username=demo-user-2 --from-literal=password=changeme
kubectl -n demo apply -f demo-app/demo.yaml
kubectl -n demo port-forward svc/jira 8080:80
```

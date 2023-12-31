# kcd-multi-tenancy-vcluster-crossplane-demo

![Alt text](./img/argocd_crossplane_vcluster_architecture_demo.png)

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
kubectl apply -f tenants/tenant-2/vcluster/vcluster.yaml
vcluster list
vcluster connect vcluster-tenant-2 -n tenant-2
```
# Cloud Resources
```bash
kubectl apply -f tenants/tenant-2/apps/gcp.yaml
```

# Sample Application
```bash
vcluster list
vcluster connect vcluster-tenant-2 -n tenant-2
kubectl create ns demo
kubectl -n demo create secret generic postgres-creds  --from-literal=username=demo-user-2 --from-literal=password=changeme
cd demo-app
helm install demo -n demo . -f values.yaml
kubectl -n demo port-forward svc/demo-jira 8080:80
```

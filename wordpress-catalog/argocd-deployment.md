# GitOps 
### Deploy ArgoCD
- Use Argo `Gitops tool` for the CD aspect
```
kubectl create namespace argocd
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```
- Deploy ArgoCD ApplicationSet Controller
```
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj-labs/applicationset/v0.2.0/manifests/install.yaml
```
- Create route for accessing argocd instance
```
spec:
  host: <argocd-argocd.apps.rpa-multi-tenant.cp.fyre.ibm.com>
  to:
    kind: Service
    name: argocd-server
    weight: 100
  port:
    targetPort: https
  tls:
    termination: passthrough
    insecureEdgeTerminationPolicy: Redirect
  wildcardPolicy: None
```
```
- Rrequires for openshift deployment
oc adm policy add-scc-to-user privileged  -z argocd-redis
oc adm policy add-scc-to-user anyuid  -z argocd-redis
```
- Get initial ArgoCD password
```
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```
- Deploy Argocd CLI
    ```
    curl -sSL -o /usr/local/bin/argocd https://github.com/argoproj/argo-cd/releases/latest/download/argocd-linux-amd64
    chmod +x /usr/local/bin/argocd
    argo version --short
    ```
- Update ConfigMap `argocd-cm`to customize sync behavior
```
data:
    resourceCustomizations: |
        operators.coreos.com/CatalogSource:
        health.lua: |
        hs = {}
        if obj.status ~= nil then
            if obj.status.connectionState ~= nil then
            if obj.status.connectionState.lastObservedState ~= nil then
                if obj.status.connectionState.lastObservedState == "READY" then
                hs.status = "Healthy"
                return hs
                end
            end
            end
        end
        hs.status = "Progressing"
        hs.message = "Unknown"
        return hs
        operators.coreos.com/Subscription:
        health.lua: |
        hs = {}
        if obj.status ~= nil then
            if obj.status.installedCSV ~= nil then
            hs.status = "Healthy"
            hs.message = "CSV Installed"
            if obj.status.state ~= nil then
                hs.message = obj.status.state
            end
            return hs
            end
        end
        hs.status = "Progressing"
        hs.message = "Unknown"
        if obj.status.state ~= nil then
            hs.message = obj.status.state
        end
        return hs
        operators.coreos.com/OperatorGroup:
        health.lua: |  
        hs = {}
        if obj.status ~= nil then
            if obj.status.lastUpdated ~= nil then
            hs.status = "Healthy"
            hs.message = "OperatorGroup Installed"
            return hs
            end
            return hs
        end
        hs.status = "Progressing"
        hs.message = "Unknown"
        return hs
```

### Create application
- Simple wordpress application 
    - in the namespace make sure to run these commands for now 
    ```
    oc adm policy add-scc-to-user anyuid -z default
    oc adm policy add-scc-to-user privileged -z default
    ```
    - Using ApplicationSet
        https://github.com/amitabhprasad/applicationset/tree/master
    - Using Application
        https://github.com/amitabhprasad/kustomize-demo/blob/main/wordpress-catalog/allinone/all-in.one.yaml

### Reference 
    https://www.youtube.com/watch?v=c4v7wGqKcEY
    https://100daysofkubernetes.io/tools/argo.html
    https://argo-cd.readthedocs.io/en/stable/operator-manual/health/
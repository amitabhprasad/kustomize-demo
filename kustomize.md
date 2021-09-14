# What is kustomize
- Configuration management solution that uses layering to preserve the base settings of the applications and components by overlaying declerative yaml artifacts (patches) that selectively overrides default setting without changing the original file

- Allow us to change several aspects of the state of deployments based on env etc.
- Allow us to specify different values of the kubernetes resources for different situations
    - basically allows you to override the defualt value or defnine new attributes not in the base file.
- Helps to make YAML resources more dynamic
- Kustomize is part of kubectl so it should work without additional installation using ```kubectl -k``` to specify that you want to use kustomize.
- With kustomize, we want to have our yaml templates and then customize the values provided to that resource manifest. However, each directory that is referenced within kustomized should have it's own ```kustomization.yaml``` file.

### Commands
- ```kubectl apply -k .```
- ```kustomize build . > mydeployment.yaml```
- ```kustomize build . | kubectl apply -f -```

### Example
- 
├── base
│   ├── deployment.yaml
│   ├── kustomization.yaml
│   └── service.yaml
└── overlays
    ├── dev
    │   └── kustomization.yaml
    ├── production
    │   ├── kustomization.yaml
    │   ├── rollout-replica.yaml
    │   └── service-loadbalancer.yaml
    └── staging
        ├── kustomization.yaml
        └── service-nodeport.yaml

### References
- overview :
    - https://www.youtube.com/watch?v=Twtbg6LFnAg&t=137s
    - https://www.densify.com/kubernetes-tools/kustomize
- https://blog.scottlowe.org/2019/09/13/an-introduction-to-kustomize/
- https://www.youtube.com/watch?v=ASK6p2r-Yrk
- https://kubectl.docs.kubernetes.io/references/kustomize/kustomization/
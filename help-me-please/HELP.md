### NGINX ArgoCD application:
```yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: nginx
  namespace: argocd
  finalizers:
    - resources-finalizer.argocd.argoproj.io # Adding the finalizer enables cascading deletes when implementing the App of Apps pattern. If this isn't used, when you remove the application yaml from git, the application will be removed from ArgoCD but the resources will remain active in the cluster
spec:  
  destination:
    namespace: nginx
    server: https://kubernetes.default.svc
  project: default
  source:
    repoURL: https://github.com/tomasferrarisenda/kubevela-minikube-lab.git # This value was modified by the initial-setup python script
    path: helm-charts/infra/nginx
    directory:
      recurse: true
      jsonnet: {}
  syncPolicy:
    automated:
      selfHeal: true
      prune: true
    syncOptions:
      - CreateNamespace=true
```

### NGINX KubeVela application:
```yaml
apiVersion: core.oam.dev/v1beta1
kind: Application
metadata:
  name: nginx
spec:
  components:
  - name: nginx
    properties:
      image: nginx:1.20.0
      ports:
      - expose: true
        port: 80
    traits:
    - properties:
        http:
          /: 80
      type: gateway
    type: webservice
```


### Argo UI
<p title="Banner" align="center"> <img src="https://i.imgur.com/G64AWEM.png"> </p>

<p title="Banner" align="center"> <img src="https://i.imgur.com/0xC253X.png"> </p>

# argocd app of apps instruction:

### 1. create a blank project on your git repo

### 2. create argocd application and connect to previously created project:

**replace the name, namespace, project, source path, repoURL and target revision with your required values**

```
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: app-of-apps
  namespace: argocd
spec:
  destination:
    namespace: argocd
    server: https://kubernetes.default.svc
  project: default
  source:
    path: app-of-apps
    repoURL: https://git.whatever.ir/app-of-apps/argocd.git
    targetRevision: uat
```

### 3. on the git repo push directories and files of your actual argocd applications. like this:

```
git.whatever.ir/app-of-apps/argocd/app-of-apps/elasticserach/elasticsearch.yaml
git.whatever.ir/app-of-apps/argocd/app-of-apps/fluent/fluent-bit.yaml
git.whatever.ir/app-of-apps/argocd/app-of-apps/kafka/kafka.yaml
git.whatever.ir/app-of-apps/argocd/app-of-apps/redis/redis.yaml
```
**you can customize directory structure of your apps on the git**

**here is a sample for elasticsearch:**
```
git.whatever.ir/app-of-apps/argocd/app-of-apps/elasticserach/elasticsearch.yaml:
```

```
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: elasticsearch
  namespace: argocd
  labels:
    app.kubernetes.io/instance: app-of-apps
    app.kubernetes.io/managed-by: argocd
    category: platform
    environment: uat
    stack: elk
    tier: infrastructure
spec:
  destination:
    namespace: elastic-system
    server: https://kubernetes.default.svc
  project: default
  source:
    path: elasticsearch
    repoURL: https://git.whatever.ir/services/elasticsearch.git
    targetRevision: uat
```


### 4. and last but not least, sync your argocd app of apps. that's all :)

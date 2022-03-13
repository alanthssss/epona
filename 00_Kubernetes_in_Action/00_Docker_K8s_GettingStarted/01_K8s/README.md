# K8s

## Prerequisites

### docker-desktop

### Minikube

> https://minikube.sigs.k8s.io/docs/start/

```zsh
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-darwin-amd64
sudo install minikube-darwin-amd64 /usr/local/bin/minikube
```

`minikube start`

### kubectl

> - https://kubernetes.io/docs/tasks/tools/install-kubectl-macos/
> - https://kubernetes.io/docs/tasks/tools/install-kubectl-macos/#optional-kubectl-configurations-and-plugins

`brew install kubectl`

```zsh
# KUBECTL
autoload -Uz compinit
compinit
source <(kubectl completion zsh)
alias k=kubectl
compdef __start_kubectl k
```

`source ~/.zshrc`

`k cluster`

`k get node`

`k describe node docker-desktop`

### kubectx

> https://github.com/ahmetb/kubectx

`brew install kubecix`

## First Application on K8s

`k create ns alan`

`kubens alan`

<!-- `k run kubia --image=nthssss/kubia --port=8080 --generator=run/v1` -->
<!-- `k run kubia --image=nthssss/kubia --port=8080` -->
```zsh
cat <<EOF | k apply -f -
apiVersion: v1
kind: ReplicationController
metadata:
  name: kubia
spec:
  replicas: 1
  selector:
    app: kubia
  template:
    metadata:
      name: kubia
      labels:
        app: kubia
    spec:
      containers:
      - name: kubia
        image: nthssss/kubia
        ports:
        - containerPort: 8080
EOF
```

`k get po`

`k expose rc kubia --type=LoadBalancer --name kubia-http`
<!-- `k expose po kubia --type=LoadBalancer --name kubia-http` -->

`k get svc`

`curl localhost:8080`

`k get rc`

`k scale rc kubia --replicas=3`

`k get rc`

`k get po`

```zsh
# this trige do not have different results on my cluster as we expect.
curl localhost:8080
curl localhost:8080
curl localhost:8080
```

`k describe po kubia-57bb6`

### Dashboard

> https://kubernetes.io/docs/tasks/access-application-cluster/web-ui-dashboard/

```zsh
cat <<EOF | k apply -f -
apiVersion: v1
kind: ServiceAccount
metadata:
  name: admin-user
  namespace: kubernetes-dashboard
EOF
```

```zsh
cat <<EOF | k apply -f -
apiVersion: rbac.authorization.k8s.io/v1
kind: ClusterRoleBinding
metadata:
  name: admin-user
roleRef:
  apiGroup: rbac.authorization.k8s.io
  kind: ClusterRole
  name: cluster-admin
subjects:
- kind: ServiceAccount
  name: admin-user
  namespace: kubernetes-dashboard
EOF
```

`kubectl -n kubernetes-dashboard get secret $(kubectl -n kubernetes-dashboard get sa/admin-user -o jsonpath="{.secrets[0].name}") -o go-template="{{.data.token | base64decode}}" > admin-user.token`

`k proxy`

Navigate `http://localhost:8001/api/v1/namespaces/kubernetes-dashboard/services/https:kubernetes-dashboard:/proxy/`

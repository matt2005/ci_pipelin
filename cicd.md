# installation

based on [jenkins.io](https://www.jenkins.io/doc/book/installing/kubernetes/)

## Setup Helm
```powershell
choco install kubernetes-helm -y
```


```

- Setup jenkins
```powershell
kubectl create namespace jenkins
kubectl get namespaces
helm repo add jenkinsci https://charts.jenkins.io
helm repo update
kubectl apply -f jenkins-volume.yaml
kubectl apply -f jenkins-sa.yaml
$chart="jenkinsci/jenkins"
helm install jenkins -n jenkins -f jenkins-values.yaml $chart
kubectl -n jenkins get all
# Jenkins password
[System.Text.Encoding]::ASCII.GetString([System.Convert]::FromBase64String($(kubectl get secret --namespace jenkins jenkins -o jsonpath="{.data.jenkins-admin-password}")))

$POD_NAME=$(kubectl get pods --namespace jenkins -l "app.kubernetes.io/component=jenkins-master" -l "app.kubernetes.io/instance=jenkins" -o jsonpath="{.items[0].metadata.name}")
echo http://127.0.0.1:8080
kubectl --namespace jenkins port-forward $POD_NAME 8080:8080
```

# gitlab

```
helm repo add gitlab https://charts.gitlab.io/
helm repo update
kubectl create namespace gitlab
helm install gitlab -n gitlab gitlab/gitlab --set global.hosts.domain=DOMAIN --set certmanager-issuer.email=EMAIL
# get ip
kubectl get ingress -n gitlab -lrelease=gitlab
# get all
kubectl get all -n gitlab
```


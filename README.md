## sample-addons-k8

This project is used to deploy k8 addons using helm charts and App-of-Apps pattern of ArgoCD

### Local Helm commands
```shell
helm repo add argo https://argoproj.github.io/argo-helm
helm repo update
kunectl create ns argocd
helm upgrade --install --atomic --version 5.51.6 --namespace argocd my-argocd argo/argo-cd

kubectl patch svc my-argocd-server -n argocd -p '{"spec": {"type": "LoadBalancer"}}'

export ARGOCD_SERVER=`kubectl get svc my-argocd-server -n argocd -o json | jq --raw-output '.status.loadBalancer.ingress[0].hostname'`

export ARGOCD_PWD=$(kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo)

brew install argocd
argocd login $ARGOCD_SERVER --username admin --password $ARGOCD_PWD --insecure

kubectl apply -n argocd -f app-of-apps.yaml

argocd app list

argocd app sync add-ons
```


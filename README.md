# minikube + Istio + Argo CD GitOps

## Run locally
```bash
minikube start --cpus=2 --memory=6g
istioctl install --set profile=demo -y
kubectl create ns argocd
kubectl apply -n argocd --server-side -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
# edit repoURL in argocd/*.yaml first, push repo to GitHub, then:
kubectl apply -f argocd/
minikube tunnel   # separate terminal
INGRESS=$(kubectl -n istio-system get svc istio-ingressgateway -o jsonpath='{.status.loadBalancer.ingress[0].ip}')
curl -H "Host: nginx.local" http://$INGRESS/
curl -H "Host: bookinfo.local" http://$INGRESS/productpage
```

## Run with GitHub Actions
Push to a **public** GitHub repo (Argo CD pulls from it) on branch `main`; the workflow in `.github/workflows` runs automatically or via *Run workflow*.

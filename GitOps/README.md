In this exercise, you will instanciate an ArgoCD and get familiar with basics of GitOps.

## Install ArgoCD

Create an `argocd` namespace.

Then deploy ArgoCD:

```sh
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```

## Connect on the ArgoCD server

Get the password of the `admin` user:

```sh
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d; echo
```

Then run port-forward to reach the service of `argocd server` on the bastion instance on  port 8080.

```sh
kubectl port-forward --address 0.0.0.0 -n argocd svc/SERVICE_NAME LOCALPORT:SERVICE_PORT
```

Open a browser on https://<BASTION_URL>:8080, then login.

What is the default cluster URL?

## Create your first application

Inside the default project, create a new application named `ui-gitops-chart`.

* Enable `Auto create namespace`.
* For the repository, indicate: `https://github.com/wescale/k8s-advanced-training.git`
* Path: `GitOps-helm/sample-demo`
* Destination: the default cluster
* namespace: `ui-gitops`

Then click on **Create**.

You should see your application.

**Sync** it and verify it has been correctly configured.

## Declarative manifests

Look at the resources your kubernetes cluster can manage (`kubectl api-resources`).
What are the resources provided by **argo**?

Edit the given `application.yaml` to declare an ArgoCD application named `declarative-gitops-chart` to deploy the same Helm chart as above.

This chart must be in the namespace `declarative-gitops`.

Then apply the manifest: `kubectl apply -f application.yaml`

Look at the ArgoCD UI to view the state of your deployment.

## Clean

> Clean only if you don't play next lab on gitops-helm

```sh
kubectl delete -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
kubectl delete ns argocd
kubectl delete ns ui-gitops
kubectl delete ns declarative-gitops
```
# davli-IOT-app

Application config repository for the **Inception-of-things** project, deployed continuously by **ArgoCD**.

## Purpose

ArgoCD watches the `config/` folder on the `app` branch of this repo and keeps a Kubernetes cluster in sync with whatever manifests it finds there (`Deployment` + `Service`). Push a change to that folder, ArgoCD applies it.

## Switching between v1 and v2

Pre-made manifests live in the `files/` folder:

- `files/Deployment_v1.yml` → image `wil42/playground:v1`
- `files/Deployment_v2.yml` → image `wil42/playground:v2`

To switch versions, on the `app` branch, replace `config/Deployment_v1.yml` with the desired version:

```bash
git checkout app

# switch to v2
cp files/Deployment_v2.yml config/Deployment_v1.yml

# or switch back to v1
cp files/Deployment_v1.yml config/Deployment_v1.yml

git add config/ && git commit -m "switch version" && git push
```

ArgoCD will detect the change and roll out the new version automatically.

## Layout

```
config/   # watched by ArgoCD (deployed manifests)
files/    # version templates (v1 / v2)
```

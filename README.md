# oidc-lab-app

Sample application for **Lab 9** of
[Use-the-Azure-Login-action-with-OpenID-Connect](https://github.com/morakot-inta/Use-the-Azure-Login-action-with-OpenID-Connect/blob/main/docs/09-app-repo.md).

The pipeline builds a container image, pushes it to Azure Container Registry and deploys it to AKS.
It uses **no stored passwords**: every Azure, ACR and Kubernetes login comes from the GitHub OIDC token.

```
push to main
  └─ build   (subject <PREFIX>:ref:refs/heads/main)          → AcrPush on the registry
       └─ deploy (subject <PREFIX>:environment:production)    → AKS Cluster User + RBAC Writer on namespace oidc-app
```

## Layout

| Path | What |
|------|------|
| `app/` | Static web page + `Dockerfile` (non-root nginx on port 8080) |
| `k8s/app.yaml` | Deployment + Service. The deploy job fills in `IMAGE_PLACEHOLDER`. |
| `.github/workflows/build-deploy.yml` | Build → push → deploy |

## Configuration (set up in Lab 9)

| Kind | Name | Value |
|------|------|-------|
| Secret | `AZURE_CLIENT_ID` | Client ID of managed identity `id-gh-oidc-app` |
| Secret | `AZURE_TENANT_ID`, `AZURE_SUBSCRIPTION_ID` | Your tenant and subscription |
| Variable | `ACR_NAME` | Registry name (without `.azurecr.io`) |
| Variable | `AZURE_RESOURCE_GROUP` | `rg-oidc-lab` |
| Variable | `AKS_CLUSTER_NAME` | `aks-oidc-lab` |
| Environment | `production` | Used by the deploy job |

The build job is skipped until `ACR_NAME` is set.

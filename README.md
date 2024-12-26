[![python-pipeline](https://github.com/amrendra01/python-pipeline/actions/workflows/pipeline.yml/badge.svg)](https://github.com/amrendra01/python-pipeline/actions/workflows/pipeline.yml)




## This project uses github workflow to build & push python webapp docker image to ACR and then deploys on AKS Cluster

## Steps to create Infrastructure on Azure Cloud.
### Login to Azure cli
```
az login
```

### Create Resource Group and ACR
```
az group create --name githubActions --location centralindia
az acr create --resource-group githubActions --name <AcrName> --sku Basic
```

### Create contributor role for GithubActions to access AzureResources.
```
az ad sp create-for-rbac --name "GitHub-Actions" --role contributor --scopes /subscriptions/<subscriptionId> --sdk-auth
```

### Create AcrPush role.
```
az role assignment create --assignee <ClientId> --scope /subscriptions/<subscriptionId>/resourceGroups/githubActions/providers/Microsoft.ContainerRegistry/registries/<ACRegistryId> --role AcrPush
```

### Create AKS cluster.
```
az aks create --resource-group githubActions --name myaksCluster --node-count 1 --generate-ssh-keys
```
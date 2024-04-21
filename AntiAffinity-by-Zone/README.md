
# Anti Affinity by Zone

### Create AKS Cluster Multi-Zone
``` shell
LOCATION=eastus
RESOURCE_GROUP=azr-rg-aks
CLUSTER_NAME=azr-aks-lab-n

az group create --name $RESOURCE_GROUP --location $LOCATION

az aks create \
    --resource-group $RESOURCE_GROUP \
    --name $CLUSTER_NAME \
    --node-count 3 \
    --zones 1 2 3 \
    --enable-app-routing
```

### Deployment Application with AntiAffinity

``` yaml
...
    spec:
      affinity:
        podAntiAffinity:
          requiredDuringSchedulingIgnoredDuringExecution:
          - labelSelector:
              matchExpressions:
              - key: app
                operator: In
                values:
                - anti-affinity-dummies
            topologyKey: "topology.kubernetes.io/zone"
...
```
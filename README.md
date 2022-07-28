# Azure Container Apps Album Viewer UI

## Container App Creation Code

$RESOURCE_GROUP="containerapps-demo"
$LOCATION="eastus"
$ENVIRONMENT="containerapps-env-demo"
$API_NAME="album-api"
$FRONTEND_NAME="album-ui"
$ACR_NAME="academo"
$API_BASE_URL=$(az containerapp show --resource-group $RESOURCE_GROUP --name $API_NAME --query properties.configuration.ingress.fqdn -o tsv)

az containerapp create `
    --name $FRONTEND_NAME `
    --resource-group $RESOURCE_GROUP `
    --environment $ENVIRONMENT `
    --image $ACR_NAME.azurecr.io/albumapp-ui  `
    --env-vars API_BASE_URL=https://$API_BASE_URL `
    --target-port 3000 `
    --ingress 'external' `
    --registry-server "$ACR_NAME.azurecr.io"  `
    --query configuration.ingress.fqdn

## Build Code

cd into src folder

az acr build --registry $ACR_NAME --image albumapp-ui .
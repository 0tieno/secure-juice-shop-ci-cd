Push the Docker Image to Azure Container Registry (ACR)

az login
az acr create --resource-group <your-resource-group> --name <your-acr-name> --sku Basic
az acr login --name <your-acr-name>
docker images
docker tag <image-name> <your-acr-name>.azurecr.io/juice-shop:v1
docker push <your-acr-name>.azurecr.io/juice-shop:v1


Deploy the Container to Azure Web App

az webapp create --resource-group <your-resource-group> \
  --plan <your-app-service-plan> \
  --name <your-web-app-name> \
  --deployment-container-image-name <your-acr-name>.azurecr.io/juice-shop:v1


Set Up ACR Authentication for Azure Web App

az webapp config container set --name <your-web-app-name> \
  --resource-group <your-resource-group> \
  --docker-custom-image-name <your-acr-name>.azurecr.io/juice-shop:v1 \
  --docker-registry-server-url https://<your-acr-name>.azurecr.io \
  --docker-registry-server-user <your-acr-name> \
  --docker-registry-server-password $(az acr credential show --name <your-acr-name> --query "passwords[0].value" --output tsv)


Restart the Web App to Apply Changes

az webapp restart --name <your-web-app-name> --resource-group <your-resource-group>


Check Deployment

https://<your-web-app-name>.azurewebsites.net

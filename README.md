***
# Deploying Applications in Azure

## Requirments
Download [Azure Cli](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli)

Open cmd/powershell/terminal and Login to Azure with
```
az login
```

# AZURE CONTAINER REGISTRY. THE first step for all methods below.
## Logging in
 1. Use the Azure GUI to create a new Azure Container Registry (CR).
 2. After having been logged in to azure log in to the CR run:
 ``` 
 az acr login --name <acrname>
 ```
 This should update the docker.config file itself so you can use docker command on the registry without logging in to docker for the next 3 hours.

 3. If for whatever reason it doesnt work or you want to remain logged in for long without relogging in to acr, you can login like this:

 ```
 docker login <acrname>.azurecr.io
 ```
 When prompted for username and password use one of this two ways to authenticate

**First way**

  Go to the created resource
  > Settings >
  > Access Keys >
  > Check Admin user box  ✓ 

  User username and password provided there
  
**Second way**
 ``` 
 az acr login --name <acrname> --expose-token
 ```
 Use username: 00000000-0000-0000-0000-000000000000
 
 Password: <accessToken returned created from --expose-token>

## Naming the Images
The images must be properly tagged in order to be recognized by the push commands. 
* For single image do
```
docker tag <your_image> <acrname>.azurecr.io/<your_image>
```
* For a docker compose file change the image property of each container to
```
- image <acrname>.azurecr.io/<your_image>
```
**__FOLLOW THE NAMING CONVENTION, lowercase alphanumerics and hyphen(-) ... DO NOT USE "_" in the container name or it might not run later. IMAGE name is not relevant, only the service name__**

## Push Single Docker Image
```
docker push <acrname>-azurecr.io/<your_image>
```
## Push by Docker Compose
Go to the correct directory where the docker-compose.yaml file is and
```
docker-compose push
```

# The Super Easy way but limited in size of docker compose file
Through the Azure CLI create a Web App under App-Services. Choose Docker-Container for "Veröfentlichen" and under the Docker Tab select Docker Compose, Azure Container Registry and upload the docker-compose.yaml file. If you want to use the cli see [here](https://learn.microsoft.com/en-us/azure/app-service/quickstart-multi-container). Keep in mind that there is a **limit** on the size of the docker-compose file of 4000 characters when converted to Base64. (Aprox 1000 normal characters)

# Container instances.
# through docker compose built in stuff, limitations on ports and stuff, TODO services named by convention above
# Through az craete with yaml file that kind of replaces the docker compose
naame of container follows convention above. port should be the one the container exposes

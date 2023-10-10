# CICD for Deploying Application to the Azure Container Apps 

- here we will be using the `Azure DevOps pipeline` to `create a Azure Container Apps`

- here the `developer` were `Developing the Apps` and `push that to the github repo`

- once that reaches to the `github` then we can `create an Azure Pipeline or GitHuBActions` then
  
  - CICD pipeline will read the `code from the Git Repository`

  - then we will have to build the `docker image` from there , from the `DockerFile` , which will help in `creating the Docker Image`
  
  - then we can push the `image that we build to the container regisrty` which can be `Docker Hub/ Azure Container Registry`
  
  - then we will be `deploying the container image` that we have created  into the `Azure Container Apps`
  
    - for this `we will be using a Task` under a `Task` in order to `connect the Azure DevOps subscription` to the `Azure Container Apps`  using a `service principle`
    
    - then we will `deploy that container image` to the `Azure Container Apps`

- here we are defining the `TAG` variable as `$(Build.BuildId)` which is an `environment variable` `incremental number` for the `BuildID`

- but while pushing to the `Docker Hub` then we need to `configure the username and password `  using the `Service Connection (Docker Registry)` in `azure DevOps subscription`

- for `deploying to the Azure container` we need to set the `service principle` using the the `Service Connection(Azure Service Connection)` in `azure DevOps subscription` and `Azure container Apps` in order to `deploy the image in the Docker-HUB container registry` to the `Azure DevOps Container Apps`
 
-   if we want to deploy the `Docker Image to Azure container Apps` then we can define the `CICD pipeline ` as below 

    ```yaml
        azure-piplines.yaml
        ===================

        variables:
            IMAGE_NAME: psarangi58/custom-nginx
            CONTAINERAPPS_APP: custom-nginx  # ContainerApp Name that we have in the Azure Container App
            CONTAINERAPPS_ENVIRONMENT: aca-environment #containerAppEnvName inside the Azure Container Apps or (AZA)
            RESOURCE_GROUP: rg-containerapps-azure-pipelines
            TAG: '$(Build.BuildId)' # tag for the Image


        stages:
        
        - stage: Build # this stage will Build the Image and Deploy to the Contianer registry as dockerhub
          displayName: Build and Push Image
          jobs:
          - job: Build
            displayName: Build
            pool:
              vmImage: 'ubuntu-latest'

            steps:
            - task: Docker@2 # this will just build and Push the image to the DockerHub Registry
              displayName: Build An Image
              input:
                command: BuildAndPush
                dockerfile: /DockerFile
                repository: $(IMAGE_NAME)
                tags: $(TAG) 


        - stage: Deploy # this stage will Deploy the Image that we build to the ACA or Axure container Apps
          displayName: Deploying to Azure Container Apps
          jobs:
          
          - job: Deploy
            displayName: Deploy
            
            pool:
              vmImage: ubuntu-latest
            
            steps:
            
            - tasks: AzureContainerApps@1 # this will create a service principle to deploy the Apps to Azure container Apps
              displayName: Deploy New Container Version
              inputs:
                azureSubscription: 'azure_connection' # for this service principle we need to create a service connection between the Azure Subscription and Azure container Apps and here the service connection name is `azure_connection` that we have created inside the Azure Subscription Manually
                imageToDeploy: '$(IMAGE_NAME):$(TAGS)' #image to deploy
                containerAppName: '$(CONTAINERAPPS_APP)' # Azure container App Name
                resourceGroup: '$(RESOURCE_GROUP)' # the resource group also we can specify
                containerAppEnvironment:  '$(CONTAINERAPPS_ENVIRONMENT)' # here we can also specify the ACA container App Environment
                targetPort: '3500' # here we are defining which port we want to expose in the container image
                ingress: 'external' # with ingress that decide whether we want to expose the Targeted Endpoint to Public Endpoint

    ```

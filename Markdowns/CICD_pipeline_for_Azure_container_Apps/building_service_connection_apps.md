# creating the Service Connection to the Azure SubScription to Azure Container Apps

- here we can go to the `HoussemDellai/aca-course/10_aca_azuredevops/Readme.md`

- here we need to `go to the Azure Cloud Shell` in order to `login to the Azure Cloud Shell` using the command as `az login`

- there there are `Few Resource Group` and `conrainerApp` and `containerAppEnv` and `Log Analyst` we need to `create in here` 

- then we need to create a `Resource Group` with the command as below 

    ```bash
        # here we have define few variables in here 
        $RESOURCE_GROUP="<any str>"
        $LOCATION="westeurope"
        $CONTAINERAPPS_ENVIRONMENT="<any str>"
        $CONTAINERAPPS_APP="<any str>"
        # step-1:- creating the resource group using the Azude CLI on Azure cloudshell
        az group create --location $LOCATION --resource-group $RESOURCE_GROUP
        # creating the Azure Resource Group in here
        # then we need to register for subscription as below 
        # this will create the resource  group which can be seen over here as below link
        # https://portal.azure.com/ 
        # here  on the below we are creating the Log Analytics workspace
        # we can see that in the Azure DevOps Resouce Group in the portal
        az provider register -n Microsoft.OperationalInsights --wait
        # this will be for registering the apps in here 
        # then we can create the container App Env as below 
        # if we are going inside the reosurce group we can see the Container App Env
        az containerapp env create \   
        --name $CONTAINERAPPS_ENVIRONMENT \ 
        --resource-group $RESOURCE_GROUP \ 
        --location $LOCATION
        # then we can define the container app with a default image as below 
        # if we are going inside the reosurce group we can see the Container App
        # we also able to see the `Log Analytics workspace`
        # here we are using the hello-world program
        az containerapp create \
        --name $CONTAINERAPPS_APP \                 
        --resource-group $RESOURCE_GROUP \ 
        --environment $CONTAINERAPPS_ENVIRONMENT  \ 
        --image mcr.microsoft.com/azuredocs/containerapps-helloworld:latest   \        
        --target-port 80  \
        --ingress 'external'
    
    ```

- we will have to `crete the service connection` from the `Azure Subscription` to the `Docker HuB registry`  as below 
  
  - go to the `project settings` &rarr; `service-connection` &rarr; `Docker Registry` &rarr; `Here we can provide the info about the docker Hub Registry by selecting it`

- here we can `also Build and create the New Container Apps` using the `Azure Container registry` and `pushing to it`

- we also have to create `Azure resource manager` which will connect the `azure subscription` to the `Azure container apps`

- hence we can go to the `project settings` for the `Azure DevOps PipeLine` and select the option as `service connection` &rarr; `Azure Resource Manager` &rarr; `Service Principal (automatic) for learning purpose`

- we can also create the `Service Principal (manual)/Managed Identity` for `Real Time Product Development` 

- we can also create the `Service Principal`  from the `Azure cloud Shell` as below 

    ```bash
        
        RESOURCE_GROUP="rg-containerapps-azure-pipelines" # here we are setting up the resource group
        RESOURCE_GROUP_ID=$(az group show --name $RESOURCE_GROUP --query id -o tsv)
        # fetching the resource Group Id  using the RESOURCE_GROUP_ID by querying the RESOURCE_GROUP
        az ad sp create-for-rbac -n "spn-aca-azure-pipelines" --role Contributor --scope $RESOURCE_GROUP_ID
        # using the Azure cloud shell and RESOURCE_GROUP_ID we are creating the Service Principle in here
        # this will be very helpful for mentioning the Azure Reource Manager whike deploying to Azure Container Apps
        # here the name of the Service Principle is spn-aca-azure-pipelines
        # the role here provided for the Contributor
        # here also we have specified the Resource Group ID which will be deried from the Resource Group
        # here we can provide the Scope as the "Entire Azure Subscriotion"
        # here we can also provide to a particular `RESOURCE_GROUP` only where we have deployed the `Azure Container App`
          
    ```


- then we can define the `aazure-pipelines.yml` as below 

    ```yaml
        ---

        trigger:
        branches:
            include:
            - main
        paths:
            include:
            - azure_pipeline_cotainer.yml
            - backend_api_python
            exclude:
            - azure_pipeline_practise.yml
            - azure-pipeline-test.yml
            - azure-pipelines.yaml


        variables:
        IMAGE_NAME: pratik186/api-fastapi
        CONTAINERAPPS_APP: album-backend-api  # ContainerApp Name that we have in the Azure Container App
        CONTAINERAPPS_ENVIRONMENT: aca-environment #containerAppEnvName inside the Azure Container Apps or (AZA)
        RESOURCE_GROUP: rg-containerapps-azure-pipelines
        TAG: '$(Build.BuildId)' 


        stages:
        - stage: BuildApps
        displayName: Build the Container Apps
        pool:
            vmImage: 'ubuntu-latest'
        jobs:
        - job: Build_Docker_App
            displayName: Build the Docker Image
            steps:
            - task: Docker@2
            inputs:
                repository: $(IMAGE_NAME)
                command: 'buildAndPush'
                Dockerfile: './backend_api_python/Dockerfile'
                containerRegistry: 'Docker_Registry_connection'
                tags: '$(TAG)'


        - stage: DeployApp
        displayName: Deploy the App to ACA Container
        
        pool:
            vmImage: 'ubuntu-latest'
        
        jobs:
        - job: Deploy_ACA_Container
            displayName: Deploy to ACA container
            steps:
            - task: AzureContainerApps@1
            inputs:
                azureSubscription: 'Pay-As-You-Go(f6e00de2-b6f0-457c-99a0-f65ef9f60f18)'
                imageToDeploy: '$(IMAGE_NAME):$(TAG)'
                containerAppName: $(CONTAINERAPPS_APP)
                containerAppEnvironment: $(CONTAINERAPPS_ENVIRONMENT)
                resourceGroup: $(RESOURCE_GROUP)
                targetPort: '3500'
                ingress: 'external'
        ...
    
    ```

- here from the `DockerFile` we are `exposing the port 3500` hence the `hence the target is of 3500`

- But we can also define the `we want the public access hence we can define the ingress=external`

- we can then go to the `Azure Portal Resource Group` &rarr; `azure continer App`  and `in nthe container section we can see the container running`

- we can also see the `ApplicationUrl` where we can see the `webapp` that we have deployed those images in here  

- **Observations**

- here while defining the `pipeline` there are few observations 

- while defining the `trigger` `declarative` we can define the `branches` and `include` specific branches as below 

    ```yaml
        trigger: # defining the trigger params in here
            branches: # out of all the branches 
                include: # including the branch here
                    - main # including the main branch in here as List 
    
    ```

- we can also define the `trigger` with the `path` `which path we want to include` and `which path we want to exclude as below` 

    ```yaml
        trigger:
            branches:
                include:
                    - main # defining the branch as List in here
            paths:
                include:
                    - azure-pipelines.yaml # which file we want to include
                exclude:
                    - Readme.md # which file we want to ignore
    
    ```

- for defining the `repository` we can define in the global group as below 

    ```yaml

        # by using this we can refere the file and directory
        resource:
            repo: self
        # here in azure DevOp Pipelines the Env Variable as $(Build.SourcesDirectory)
    
    ```

- 
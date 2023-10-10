# creating the Service Connection to the Azure SubScription to Azure Container Apps

- here we can go to the `HoussemDellai/aca-course/10_aca_azuredevops/Readme.md`

- here we need to `go to the Azure Cloud Shell` in order to `login to the Azure Cloud Shell` using the command as `az login`

- then we need to create a `Resource Group` with the command as below 

    ```bash
        # here we have define few variables in here 
        $RESOURCE_GROUP="<any str>"
        $LOCATION="westeurope"
        $CONTAINERAPPS_ENVIRONMENT="<any str>"
        $CONTAINERAPPS_APP="<any str>"
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


- we also have to create `Azure resource manager` which will connect the `azure subscription` to the `Azure container apps`

- hence we can go to the `project settings` for the `Azure DevOps PipeLine` and select the option as `service connection` &rarr; `Azure Resource Manager` &rarr; `Service Principal (automatic) for learning purpose`

- we can also create the `Service Principal (manual)/Managed Identity` for `Real Time Product Development` 

- we can also create the `Service Principal`  from the `AAzure cloud Shell` as below 

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


- then we need to `select the option` as ``  

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

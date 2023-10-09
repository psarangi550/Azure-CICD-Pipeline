# Demo Stages,Jobs,Steps on Azure CICD Pipeline 

- `Relation between stages, Jobs and Steps` in Azure Devops CICD YAML

- `Stages` `that` can `consist of` `One or Multiple Stage` `will always run` in `sequence` not on `parallel`

-  we can define the `YAML` configs as below 

    ```yaml
        azure-pipelines.yaml
        ====================
        trigger: # defining the trigger list in here
            - main
        
        pool: #definning the pool dictionary which can containe the `Build Agent Name` or `vmImage` or both
          vmImage : ubuntu-latest # defining the VM on which we want too run the pipeline on which our build agent will work

        stages: # defining all the steges with the stage name which can consist of multiple jobs
        - stage: BuildApp
          displayName:  Build Apps
        - stage: DeployDev
          displayName: Deploy to Dev Environment
        - stage: DeployProd
          displayName: Deploy to the Prod Environment
    
    ```

- we can define `multiple jobs` inside the `each stage define with name and disPlayName`

- we can define that as below 

    ```yaml

        azure-pipelines.yaml
        ====================
        trigger: # defining the trigger list in here
            - main
        
        pool: #definning the pool dictionary which can containe the `Build Agent Name` or `vmImage` or both
          vmImage : ubuntu-latest # defining the VM on which we want too run the pipeline on which our build agent will work

        stages: # defining all the steges with the stage name which can consist of multiple jobs
        - stage: BuildApp
          displayName:  Build Apps
          jobs:
          - job: Build Frontend App
            dispplayName: Building FrontEnd App
          - job: Build Backend App
            dispplayName: Building Backend App

        - stage: DeployDev
          displayName: Deploy to Dev Environment
          jobs:
          - job: Deploy Frontend App to the Dev Env
            dispplayName: Deploying FrontEnd App to the Dev Env
          - job: Deploy Backend App to the Dev Env
            dispplayName: Deploying Backend App to the Dev Env
        
        - stage: DeployProd
          displayName: Deploy to the Prod Environment
          jobs:
          - job: Deploy Frontend App to the Prod Env
            dispplayName: Deploying FrontEnd App to the Prod Env
          - job: Deploy Backend App to the Prod Env
            dispplayName: Deploying Backend App to the Prod Env

    
    ```

- we can have `one or more tasks` inside the `step` which belong to the `job` which ius under the `Job List`

- on the `steps` we can write the `task/script` with their `displayName` in order to `execute your DotNet/Java/Python Program` 

- we can define the `azure-pipelines.yml` pipeline as below  

    ```yaml
        azure-pipelines.yaml
        ====================
        trigger: # defining the trigger list in here
            - main
        
        pool: #definning the pool dictionary which can containe the `Build Agent Name` or `vmImage` or both
          vmImage : ubuntu-latest # defining the VM on which we want too run the pipeline on which our build agent will work

        stages: # defining all the steges with the stage name which can consist of multiple jobs
        - stage: BuildApp
          displayName:  Build Apps
          jobs:  # here the job inside jobs/stage can run in parallel
          - job: Build Frontend App
            dispplayName: Building FrontEnd App
            steps:    # defining the steps which consist of multiple  taks/scripts
              - script: echo Building the Frontend App 
                displayName: Build FrontEnd App
              - script: echo Unit Test for Frontend App
                displayName: Unit Test for Frontend App

          - job: Build Backend App
            dispplayName: Building Backend App
            steps:    # defining the steps which consist of multiple  taks/scripts
              - script: echo Building the Backend App 
                displayName: Build Backend App
              - script: echo Unit Test for Backend App
                displayName: Unit Test for Backend App

        - stage: DeployDev
          displayName: Deploy to Dev Environment
          jobs:   # here the job inside jobs/stage can run in parallel
          - job: Deploy Frontend App to the Dev Env
            dispplayName: Deploying FrontEnd App to the Dev Env
            steps:    # defining the steps which consist of multiple  taks/scripts
              - script: echo Deploying the Frontend App to Dev Env
                displayName: Deploying the Frontend App to Dev Env
          
          - job: Deploy Backend App to the Dev Env
            dispplayName: Deploying Backend App to the Dev Env
            steps:    # defining the steps which consist of multiple  taks/scripts
              - script: echo Deploying the Backend App to Dev Env
                displayName: Deploying the Backend App to Dev Env
        
        - stage: DeployProd
          displayName: Deploy to the Prod Environment
          jobs:   # here the job inside jobs/stage can run in parallel
          - job: Deploy Frontend App to the Prod Env
            dispplayName: Deploying FrontEnd App to the Prod Env
            steps:    # defining the steps which consist of multiple  taks/scripts
              - script: echo Deploying the Frontend App to Prod Env
                displayName: Deploying the Frontend App to Prod Env
             
          
          - job: Deploy Backend App to the Prod Env
            dispplayName: Deploying Backend App to the Prod Env
            steps:    # defining the steps which consist of multiple  taks/scripts
              - script: echo Deploying the Backend App to Prod Env
                displayName: Deploying the Backend App to Prod Env
            
    ```


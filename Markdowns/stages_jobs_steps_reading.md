# Working with Azure DevOps Pipelines Stages, Jobs and Steps

- This workshop will explore some good features of Azure Pipelines for creating complex and complete pipelines.

- In `real world scenarios`, `we might have multiple applications: frontend and backend`, `different environments: dev, test and prod`. 

- That adds more constraints to the pipelines

- For example, we want to build frontend and backend in parallel  

- we want to deploy into dev and test environments

- But right before deploying to production, we want to add a manual approva

-  Using Stages, Jobs and Steps help to implement these strategies easily   

- **Case01**

- In the following pipeline we have a single step. 

- But it has an implicit Stage and Job

- we can write the Pipeline as below 

    ```yaml
        pool:
          vmImage: 'ubuntu-latest'
          steps:
          - bash: echo "Hello world"
    
    ```

- which can also be written as below 

    ```yaml
        pool:
          vmImage: 'ubuntu-latest'
          stages:
          - stage: A
            jobs:
            - job: A1
              steps:
              - bash: echo "Hello world"

    ```

- **Case02**
  
- `Stages` are a at the `top of the tree`

- `Each stage` is `an array of Jobs` 

- `Each job` is a `collection of steps`

- `Stages and Jobs aren’t constrained to a limited use case` 

- `For example, in a pipeline we can use Jobs and Steps only without Stages if not needed` 

- But here are some important facts about each one. Stages will run sequentially unless specified (using the syntax dependsOn: [] as will be shown next).

-  Jobs within the same Stage, by default runs in parallel unless there are dependencies. 

- `Every Job and Stage` `will run in a different hosted build agent`

- `we can specify a different build agent for each Stage or Job` 

- For Example

  - Let’s take a sample scenario where we have a frontend and a backend to be built and deployed to both dev and prod environments

  - Since both apps could be built independently, we’ll build them in parallel to enhance build time
  
  - So, we’ll use 2 jobs for building the apps
  
  - The build will be triggered first, we’ll create a Stage for that by defining first
  
  - After that, we’ll deploy the apps into the dev environment
  
  - Again, we’ll use 2 jobs, one to deploy frontend and one to deploy backend.
  
  - These two jobs should run after the build stage
  
  - So, we’ll put them under a new stage
  
  - Then we’ll duplicate that stage to deploy to production.

  - hence we can write the pipeline as below 

    ```yaml
        azure-pipelines.yaml
        ====================
        trigger:
        - main
        
        pool:
        vmImage: ubuntu-latest
        
        stages:
        - stage: BuildApp
        displayName: Build Apps
        jobs:
        - job: BuildFrontendApp
        displayName: Build Frontend App
        steps:
        - script: echo building frontend app
        displayName: build frontend app
        - script: echo running unit tests for frontend app
        displayName: unit tests frontend
        
        - job: BuildBackendApp
        displayName: Build Backend App
        steps:
        - script: echo building backend app
        displayName: build backend app
        - script: echo running unit tests for backend app
        displayName: unit tests backend
        
        - stage: DeployDev
        displayName: Deploy to DEV environment
        jobs:
        - job: DeployFrontendDev
        displayName: Deploy frontend to DEV
        steps:
        - script: echo deploying frontend app to DEV
        displayName: deploy frontend app to DEV
        
        - job: DeployBackendDev
        displayName: Deploy backend to DEV
        steps:
        - script: echo deploying backend app to DEV
        displayName: deploy backend app to DEV
        
        - stage: DeployProd
        displayName: Deploy to PROD environment
        pool:
        vmImage: 'macOS-10.14'
        jobs:
        - job: DeployFrontendProd
        displayName: Deploy frontend to PROD
        steps:
        - script: echo deploying frontend app to PROD
        displayName: deploy frontend app to PROD
        
        - job: DeployBackendProd
        displayName: Deploy backend to PROD
        pool:
        vmImage: 'vs2017-win2016'
        steps:
        - script: echo deploying backend app to PROD
        displayName: deploy backend app to PROD

    ```

- **Case03**
  
  -  Adding `dependencies` `between stages and jobs`
  
  -  `We said earlier that Stages runs in sequence` `unless` `specified using the syntax dependsOn:[]` 
  
  -  Let’s try it in action in the previous pipeline
  
  - we can define the application as below 

    ```yaml
        stages:
        - stage: BuildApp
        # code removed for brievety…
        - stage: DeployDev
        dependsOn: []
        # code removed for brievety…
        - stage: DeployProd
        dependsOn: []
        # code removed for brievety…
    
    ```

- `When we run this pipeline`,` Azure DevOps will run the 3 Stages in parallel` and `will show them vertically meaning there are no dependencies`.


- **Case04**
  
  - Let’s take another example where we have dependencies between Stages.
  
  - This pattern is the ‘fan-out and fan-in’.
  
  - we can write the `azure-pipelines.yml` as below  

    ```yaml
        
        stages:
        - stage: Test
        
        - stage: DeployUS1
        dependsOn: Test # this stage runs after Test
        
        - stage: DeployUS2
        dependsOn: Test # this stage runs in parallel with DeployUS1, after Test
        
        - stage: DeployEurope
        dependsOn: # this stage runs after DeployUS1 and DeployUS2
        - DeployUS1
        - DeployUS2

    
    ```

- we can define the full exampe as below 

    ```yaml
        azure-pipelines.yml
        ====================
        trigger:
        - main
        
        pool:
        vmImage: ubuntu-latest
        
        stages:
        - stage: Test
        jobs:
        - job: test
        steps:
        - script: echo testing app
        
        - stage: DeployUS1
        dependsOn: Test # this stage runs after Test
        jobs:
        - job: deploy_us1
        steps:
        - script: echo deploying to US1
        
        - stage: DeployUS2
        dependsOn: Test # this stage runs in parallel with DeployUS1, after Test
        jobs:
        - job: deploy_us2
        steps:
        - script: echo deploying to US2
        
        - stage: DeployEurope
        dependsOn: # this stage runs after DeployUS1 and DeployUS2
        - DeployUS1
        - DeployUS2
        jobs:
        - job: deploy_europe
        steps:
        - script: echo deploying to Europe
    
    ```

- **case05**
  
  - `Adding conditions within stages` 
  
  - `We can specify the conditions under which each stage runs` 
  
  - By default, a stage runs if it does not depend on any other stage, or if all the stages that it depends on have completed and succeeded
  
  - We can customize this behavior by forcing a stage to run even if a previous stage fails or by specifying a custom condition.   
  
  - If `we customize the default condition of the preceding steps for a stage`, `we remove the conditions for completion and success`
  
  - if `we use a custom condition, it's common to use and(succeeded(), custom_condition) to check whether the preceding stage ran successfully`
  
  -  `Otherwise, the stage runs regardless of the outcome of the preceding stage.` 
  
  - Example to run a stage based upon the status of running a previous stage:  
  
  - we can define the `azure-pipelines.yml` as below 

    ```yaml
        
        stages:
        - stage: A
        
        # stage B runs if A fails , as it is the immediate stage we can get the response in here 
        - stage: B
            condition: failed()
        
        # stage C runs if B succeeds , as it is the next step we can define the Task we want to put the condition on 
        - stage: C
            dependsOn:
            - A
            - B
            condition: succeeded('B')
            
    
    ```

- **Case06**

- Example of using a custom condition:

- we can define the pipeline as below 

    ```yaml
        stages:
        - stage: A
        
        - stage: B
        condition: and(succeeded(), eq(variables['build.sourceBranch'], 'refs/heads/main'))
    
    ```


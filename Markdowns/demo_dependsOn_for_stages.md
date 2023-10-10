# Showing the Dependencies in  Azure Pipeline

- if we have stages `by default `then `it will going to run` `sequencially`

- but we can add the `dependsOn` argument  as `dependsOn: []` if we want to run the `pipeline stage`  `parallelly`

- we can use the pipeline as below for this 

    ```yaml
        azure-pipelines.yaml
        =====================

        trigger:
            - main
        
        pool:
          vmImage: 'ubuntu-latest'

        stages:
        - stage: Deploying_to_the_Test_Env
          jobs:
          - job: Deploying_to_Test
            steps:
            - script: echo "Deploying_to_Test"

        - stage: Deploying_to_the_US1_Env
          jobs:
          - job: Deploying_to_US1_Env
            steps:
            - script: echo "Deploying_to_US1_Env"

        
        - stage: Deploying_to_the_US2_Env
          jobs:
          - job: Deploying_to_US2_Env
            steps:
            - script: echo "Deploying_to_US2_Env"

        
        - stage: Deploying_to_the_Europe_Env
          jobs:
          - job: Deploying_to_Europe_Env
            steps:
            - script: echo "Deploying_to_Europe_Env"
            
    
    ```

- by default all these `stag` defined inside the `stages` will run `sequencially in the order they have defined`

- but we can use the `dependsOn` paramter to run then `parallely` as well 

- but we need to make sure `one stage` in the pipeline `should not have the stage dependent` 

- we can modify the same application as below 


    ```yaml
       
        azure-pipelines.yaml
        =====================
       
       trigger:
            - main
        
        pool:
          vmImage: 'ubuntu-latest'

        stages:
        - stage: Deploying_to_the_Test_Env
          jobs:
          - job: Deploying_to_Test
            steps:
            - script: echo "Deploying_to_Test"

        - stage: Deploying_to_the_US1_Env
          dependsOn: [] # defininng the dependsOn which means that it will run independently as there is no dependency
          jobs:
          - job: Deploying_to_US1_Env
            steps:
            - script: echo "Deploying_to_US1_Env"

        
        - stage: Deploying_to_the_US2_Env
          dependsOn: [] # defininng the dependsOn which means that it will run independently as there is no dependency
          jobs:
          - job: Deploying_to_US2_Env
            steps:
            - script: echo "Deploying_to_US2_Env"

        
        - stage: Deploying_to_the_Europe_Env
          dependsOn: [] # defininng the dependsOn which means that it will run independently as there is no dependency
          jobs:
          - job: Deploying_to_Europe_Env
            steps:
            - script: echo "Deploying_to_Europe_Env" 
    
    ```

- when running in `parallel` then the `Stages` will show `vertically over here`

- this also `depends` on the `How Many agent Nodes we have` , if we are running on `n no of stages` in parallel then `we must need 5 build agent` in order to make those jobs parallel

- if we are running the `Free Azure Pipeline` then it can provide upto `10 build agent` to run `10 stages in parallel`

- in case of the `Azure DevOps Server` in which we need to `create` the `build agent by yourself`

- so we can create the `as many build agent` based on the `as many stages` that we have 

- we need to make sure we hav the right number of `Build Agent` in order to run  `multiple stages parallely`



- **How to Add Implicit Depdendency in Stages in Azure DevOps Pipeline**
  
  -  we can also `add implecit depdencies` onto the `stages` so that they can run on a particular order as below 


    ```yaml
        azure-pipeline.yml
        ==================
        trigger:
            - main
        
        pool:
          vmImage: 'ubuntu-latest'

        stages:
        - stage: Deploying_to_the_Test_Env
          jobs:
          - job: Deploying_to_Test
            steps:
            - script: echo "Deploying_to_Test"

        - stage: Deploying_to_the_US1_Env
          dependsOn: Deploying_to_the_Test_Env # defininng implicit dependsOn 
          jobs:
          - job: Deploying_to_US1_Env
            steps:
            - script: echo "Deploying_to_US1_Env"

        
        - stage: Deploying_to_the_US2_Env
          dependsOn: Deploying_to_the_Test_Env # defininng implicit dependsOn
          jobs:
          - job: Deploying_to_US2_Env
            steps:
            - script: echo "Deploying_to_US2_Env"

        
        - stage: Deploying_to_the_Europe_Env
          dependsOn:  # here adding the implicit dependencies as the 2 Task in here 
            - Deploying_to_the_US1_Env
            - Deploying_to_the_US2_Env
          jobs:
          - job: Deploying_to_Europe_Env
            steps:
            - script: echo "Deploying_to_Europe_Env" 

    
    ```

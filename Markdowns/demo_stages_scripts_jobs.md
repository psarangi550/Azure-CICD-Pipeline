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
            displayName: Building FrontEnd App
          - job: Build Backend App
            displayName: Building Backend App

        - stage: DeployDev
          displayName: Deploy to Dev Environment
          jobs:
          - job: Deploy Frontend App to the Dev Env
            displayName: Deploying FrontEnd App to the Dev Env
          - job: Deploy Backend App to the Dev Env
            displayName: Deploying Backend App to the Dev Env
        
        - stage: DeployProd
          displayName: Deploy to the Prod Environment
          jobs:
          - job: Deploy Frontend App to the Prod Env
            displayName: Deploying FrontEnd App to the Prod Env
          - job: Deploy Backend App to the Prod Env
            displayName: Deploying Backend App to the Prod Env

    
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

- stages always run in `sequence` but job will always run in `parallel`

- but `we can make one job dependent on other job and hence making it sequencial` by using the `"dependsOn" attribute` in the `job` 

- we can also have the `specific image` for the `stage` which consist of multiple tasks

- we can also have the `specific image` for the `particular job` so all the `steps(consist of tasks/script)` will `run on the machine`

- we can write the `Azure-Pipelines/yaml` file as below 

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
            displayName: Building FrontEnd App
            steps:    # defining the steps which consist of multiple  taks/scripts
              - script: echo Building the Frontend App 
                displayName: Build FrontEnd App
              - script: echo Unit Test for Frontend App
                displayName: Unit Test for Frontend App

          - job: Build Backend App
            displayName: Building Backend App
            steps:    # defining the steps which consist of multiple  taks/scripts
              - script: echo Building the Backend App 
                displayName: Build Backend App
              - script: echo Unit Test for Backend App
                displayName: Unit Test for Backend App

        - stage: DeployDev
          displayName: Deploy to Dev Environment
          jobs:   # here the job inside jobs/stage can run in parallel
          - job: Deploy Frontend App to the Dev Env
            displayName: Deploying FrontEnd App to the Dev Env
            steps:    # defining the steps which consist of multiple  taks/scripts
              - script: echo Deploying the Frontend App to Dev Env
                displayName: Deploying the Frontend App to Dev Env
          
          - job: Deploy Backend App to the Dev Env
            displayName: Deploying Backend App to the Dev Env
            steps:    # defining the steps which consist of multiple  taks/scripts
              - script: echo Deploying the Backend App to Dev Env
                displayName: Deploying the Backend App to Dev Env
        
        - stage: DeployProd
          displayName: Deploy to the Prod Environment
          pool: # here can define the pool over her where the jobs and their step will run
            vmImage: 'macOS-10.14'
          jobs:   # here the job inside jobs/stage can run in parallel
          - job: Deploy Frontend App to the Prod Env
            displayName: Deploying FrontEnd App to the Prod Env
            steps:    # defining the steps which consist of multiple  taks/scripts
              - script: echo Deploying the Frontend App to Prod Env
                displayName: Deploying the Frontend App to Prod Env
             
          
          - job: Deploy Backend App to the Prod Env
            displayName: Deploying Backend App to the Prod Env
            pool:   # inside the jobs also we can define the pool image as well where all the tasks as a part of it will run
              vmImage: 'vs2017-win2016'  
            steps:    # defining the steps which consist of multiple  taks/scripts
              - script: echo Deploying the Backend App to Prod Env
                displayName: Deploying the Backend App to Prod Env
    
    
    ```

- the main idea of `Stages/Jobs/Steps` is to `provide logical separation` while building the `CICD Pipeline Task`

- we can create `seprate stage` for `performing infrastructure Test on the Windows Env` and also we can create the `Seprate Task to run the infrastructure Test on Linux Env`

-  we can create the `stage to deploy the Application into one Azure Region` adnd we can also define `another stage to deploy to the another Azure Region`

- we can also find this repo in `HoussemDellai/stages-jobs-tasks` and in that we have the `azure-pipelines.yml`

- we can also see the `Azure DevOps Documentation` as below in here
  
  - [Microsoft official doc for script/Tasl/Stages](https://learn.microsoft.com/en-us/azure/devops/pipelines/process/stages?view=azure-devops&tabs=yaml) 


- **Notes**

- The full syntax to specify a `stage` is:

    ```yaml
        stages:
        - stage: string  # name of the stage, A-Z, a-z, 0-9, and underscore
            displayName: string  # friendly name to display in the UI
            dependsOn: string | [ string ]
            condition: string
            pool: string | pool
            variables: { string: string } | [ variable | variableReference ] 
            jobs: [ job | templateReference]
    ```

- **dependsOn**

- When you `define multiple stages in a pipeline`,` by default, they run sequentially` `in the order in which you define them in the YAML file`. 

- The `exception to this is when you add dependencies`. `With dependencies`, `stages run in the order of` the `dependsOn` `requirements` 

- `Pipelines must contain at least one stage with no dependencies.`

- The `syntax for defining multiple stages and their dependencies` is:  
  
    ```yaml
        
        stages:
        - stage: string
            dependsOn: string
            condition: string
    
    ```

- we can define in an example as below 

    ```yaml
        stages:
        - stage: FunctionalTest
            jobs:
            - job:
            ...

        - stage: AcceptanceTest
            dependsOn: []    # this removes the implicit dependency on previous stage and causes this to run in parallel
            jobs:
            - job:
            ...
    
    ```

- we can also define below 

    ```yaml
        stages: # there will always be a stage without having a dependency
        - stage: Test

        - stage: DeployUS1
            dependsOn: Test    # this stage runs after Test

        - stage: DeployUS2
            dependsOn: Test    # this stage runs in parallel with DeployUS1, after Test

        - stage: DeployEurope
            dependsOn:         # this stage runs after DeployUS1 and DeployUS2
            - DeployUS1
            - DeployUS2
    
    ```


- **Conditions**
  
  - You can specify the `"conditions under which each stage runs" "with expressions"`. 
  
  - `By default`, a `stage runs` if it `doesn't depend on` `any other stage`, or `if all of the stages that it depends on have completed and succeeded.`
  
  - `You can customize this behavior` by `forcing a stage to run` `even if a previous stage fails or by specifying a custom condition.`
  
  - `If you customize` the `default condition of the preceding steps for a stage`, `you remove the conditions for completion and success`. 
  
  - `So, if you use a custom condition, it's common to use and(succeeded(),custom_condition) to check whether the preceding stage ran successfully` 
  
  - `Otherwise, the stage runs regardless of the outcome of the preceding stage.` 
  
  - we can use the Azure DevOps pipeline YAML i.e `azure-pipelines.yml` as below 

    ```yaml
        
        stages:
        - stage: A

        # stage B runs if A fails
        - stage: B
        condition: failed()

        # stage C runs if B succeeds
        - stage: C
        dependsOn:
        - A
        - B
        condition: succeeded('B')

    ```

- we can define `succeeded()/failed()` if we which map to the `preveious steps`

- we can also define the `azure-pipelines.yml` as below

    ```yaml
        stages:
        - stage: A

        - stage: B
            condition: and(succeeded(), eq(variables['build.sourceBranch'], 'refs/heads/main'))
            # here we are using the condition as eq which will be used as equality operator
    
    ```

- we can fetch the `Azure Pipelines` about the `job` by using the command as below 

  - [](https://docs.microsoft.com/en-us/azure/devops/pipelines/process/phases?view=azure-devops&tabs=yaml)


**Azure Devbops Pipeline Jobs**

- You can `organize` your `pipeline` `into` `jobs`. `Every pipeline has at least one job`

- `A job is a series of steps that run sequentially as a unit` 

- `In other words, a job is the smallest unit of work that can be scheduled to run.` 

- **Define a single job**

  - In the simplest case, a pipeline has a single job. In that case, you do not have to explicitly use the job keyword unless you are using a template. You can directly specify the steps in your YAML file.

      ```yaml
          pool:
          vmImage: 'ubuntu-latest'
          steps:
          - bash: echo "Hello world"
      
      ```

- You may want to `specify additional properties on that job`. `In that case`, `you can use the job keyword` as below 

    ```yaml
        jobs:  # definning the list of job in here
        - job: myJob # defining the job with the name
            timeoutInMinutes: 10  # defining th timeOutInMinutes time to become timed out
            pool:  # defining the pool image in here 
            vmImage: 'ubuntu-latest'
            steps:  # defining the steps to be executed over here
            - bash: echo "Hello world"

    ```

- The full syntax to specify a job is:

    ```yaml
    
    - job: string  # name of the job, A-Z, a-z, 0-9, and underscore
        displayName: string  # friendly name to display in the UI
        dependsOn: string | [ string ] #defining the dependOn condition out in here
        condition: string # define the condition based on the expression or status of other jobs
        strategy:
            parallel: # parallel strategy
            matrix: # matrix strategy
            maxParallel: number # maximum number simultaneous matrix legs to run
            # note: `parallel` and `matrix` are mutually exclusive
            # you may specify one or the other; including both is an error
            # `maxParallel` is only valid with `matrix`
        continueOnError: boolean  # 'true' if future jobs should run even if this job fails; defaults to 'false'
        pool: pool # agent pool
        workspace:
            clean: outputs | resources | all # what to clean up before the job runs
        container: containerReference # container to run this job inside
        timeoutInMinutes: number # how long to run the job before automatically cancelling
        cancelTimeoutInMinutes: number # how much time to give 'run always even if cancelled tasks' before killing them
        variables: { string: string } | [ variable | variableReference ] 
        steps: [ script | bash | pwsh | powershell | checkout | task | templateReference ]
        services: { string: string | container } # container resources to run as a service container
        uses: # Any resources (repos or pools) required by this job that are not already referenced
            repositories: [ string ] # Repository references to Azure Git repositories
            pools: [ string ] # Pool names, typically when using a matrix strategy for the job

    ```

- If the `primary intent of your job is to deploy your app` (`as opposed to build or test your app`), then you can `use` a `special type of job` called `deployment job`.

- Although `you can add steps for deployment tasks in a job`, we `recommend that you instead use a deployment job`

- A deployment job has a few benefits. For example, you can deploy to an environment

- `which includes benefits such as being able to see the history of what you've deployed`.


- which can be written as below 

    ```yaml
    
    - deployment: string        # instead of job keyword, use deployment keyword
        pool:
            name: string
            demands: string | [ string ]  # inside the pool we can mention demand but for stages/group we can use the `dependsOn`
        environment: string   # you can deploy to an environment
        strategy:
            runOnce:
            deploy:
                steps:
                - script: echo Hi!
    

    ```


- `Jobs can be of different types, depending on where they run`.
  
  - `Agent pool jobs run on an agent in an agent pool`.
  
  - `Server jobs run on the Azure DevOps Server.`
  
  - `Container jobs run in a container on an agent in an agent pool.` 

- **Agent Pool Job**

  - `These are the most common type of jobs` and `they run on an agent in an agent pool`
  
  - ` When using Microsoft-hosted agents`, `each job in a pipeline gets a fresh agent`. 
  
  - `Use` `demands` `with self-hosted agents` to `specify` `what capabilities an agent must have to run your job` 
  
  - `You may get the same agent for consecutive jobs`,depending on `whether there is more than one agent in your agent pool` that `matches your pipeline's demands`.
  
  - `If there is only one agent in your pool that matches the pipeline's demands, the pipeline will wait until this agent is available.`

  - we can write the pipeline for the same as below 

    ```yaml
        
        pool:
        name: myPrivateAgents    # your job runs on an agent in this pool
        demands: agent.os -equals Windows_NT    # the agent must have this capability to run the job
        steps:
        - script: echo hello world
    
    ```

    or 

    ```yaml
        
        pool:
        name: myPrivateAgents
        demands:
        - agent.os -equals Darwin
        - anotherCapability -equals somethingElse
        steps:
        - script: echo hello world
    
    ```

- **Server jobs**
  
  - `Tasks` `in a server job` `are orchestrated by` and `executed on the server ` `(Azure Pipelines or TFS) `
  
  - A server job does not require an agent or any target computers
  
  - Only a few tasks are supported in a server job at present
  
  - The maximum time for a server job is 30 days.

  - Currently, only the following tasks are supported out of the box for agentless jobs:

    - Delay task
    
    - Invoke Azure Function task
    
    - Invoke REST API task
    
    - Manual Validation task
    
    - Publish To Azure Service Bus task
    
    - Query Azure Monitor Alerts task
    
    - Query Work Items task

- we can write the `Server Jobs` as below 

    ```yaml
        
        jobs:
        - job: string
            timeoutInMinutes: number
            cancelTimeoutInMinutes: number
            strategy:
            maxParallel: number
            matrix: { string: { string: string } }

            pool: server # note: the value 'server' is a reserved keyword which indicates this is an agentless job
    
    ```

- **Dependencies**
  
  -  `When you define multiple jobs in a single stage, you can specify dependencies between them`
  
  - Pipelines must contain at least one job with no dependencies
  
  - By default Azure DevOps YAML pipeline jobs will run in parallel unless the dependsOn value is set.
  
  - The syntax for defining multiple jobs and their dependencies is: 

  - we can define the pipeline as below 

    ```yaml
        jobs:
        - job: string
        dependsOn: string
        condition: string
    
    ```

- Example jobs that build sequentially:

    ```yaml
        jobs:
        - job: Debug
            steps:
            - script: echo hello from the Debug build
        - job: Release
            dependsOn: Debug
            steps:
            - script: echo hello from the Release build
    
    ```

- Example jobs that build in parallel (no dependencies):

    ```yaml
        jobs:
        - job: Windows
            pool:
            vmImage: 'windows-latest'
            steps:
            - script: echo hello from Windows
        - job: macOS
            pool:
            vmImage: 'macOS-latest'
            steps:
            - script: echo hello from macOS
        - job: Linux
            pool:
            vmImage: 'ubuntu-latest'
            steps:
            - script: echo hello from Linux

    ```
- Or we can write as below 

    ```yaml
        jobs:
        - job: InitialA
            steps:
            - script: echo hello from initial A
        - job: InitialB
            steps:
            - script: echo hello from initial B
        - job: Subsequent
            dependsOn:
            - InitialA
            - InitialB
            steps:
            - script: echo hello from subsequent
    
    ```

- **Condition**

    - You can specify the conditions under which each job runs
    
    - By default, a job runs if it does not depend on any other job, or if all of the jobs that it depends on have completed and succeeded
    
    - You can customize this behavior by forcing a job to run even if a previous job fails or by specifying a custom condition.

    - hence we can define the `azure-pipelines.yml` as below

        ```yaml
            jobs:
            - job: A
                steps:
                - script: exit 1

            - job: B
                dependsOn: A
                condition: failed()
                steps:
                - script: echo this will run when A fails

            - job: C
                dependsOn:
                - A
                - B
                condition: succeeded('B')
                steps:
                - script: echo this will run when B runs and succeeds
        
        ```


    - Example of using a custom condition:

      ```yaml
        jobs:
        - job: A
            steps:
            - script: echo hello

        - job: B
            dependsOn: A
            condition: and(succeeded(), eq(variables['build.sourceBranch'], 'refs/heads/main'))  # here when both the condition satisfied then it will be going to be get executed
            steps:
            - script: echo this only runs for master
      
      ``` 
 
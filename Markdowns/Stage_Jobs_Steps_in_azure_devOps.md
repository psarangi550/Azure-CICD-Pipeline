# Azure DevOps Stages/Job/Steps

- when creating the `CICD pipeline` we can use the `step to perform some task`

- but we can use the `Stages` and `Jobs` while defining the `Azure CICD pipeline`

-  **Jobs**
   
   - A `Job` is a `collection of multiple tasks` 

- **stage**   
   
   - A `Stage` is a `colection of multiple JObs`
   
- **Steps** 
  
  - a `Steps` is a `collection of multiple tasks`    


- one of the `usecase being` when we have the `frontend and backend application` then we can define the `Build Frontend App` and `Build Backend App` as `jobs` and able to run bith the `jobs` `parallely`

- here we can define the `jobs` as `2 different job` where we can `create one job for Build Frontend App` and other can be of `create another job for Build Backend App`

- we have the `build stage` inside that we can  have `multiple jobs with creating fronend and backend application`

- then we can have `another stage`which is for the `release Pipeline or CD` for `deploying the application` with `multiple jobs residing inside the stage`

- we can then `deploy the Frontend App that we build to the Dev Env` and `deploy the Backend App that we build to the Dev Env` and we can have another stage to `deploy the entire app into the production`

- but when we are using the `Azure DevOps Vsual designer GUI` then `we can only create `a `single Pipeline stage` but we can have `multiple jobs` in that , which can run `parallely`

-  we can here in the `build stage` can `Build the Frontend and Backend App with 2 different JOb`

- then we can also create `another job` with repect to `databae` in order to create the `DB Script` using the `MS Build` inside the `same build stage`

- then we can have the `another job` to run the `UAT Testing with selenium` inside the `build stage`

- then we will have the `Azure ARM Template` which will help in `building the infrastructure env` and `Pester Test to check the infrastructure env`

- if we want to see the  `YAML pipeline` of the `Visual Designer` then we can see the details of `each task inside the specific job` which are `running parallely`

<br/><br/>

- with the help of the `GUI Visual Designer` we have `segregate the build pipeline to the release pipeline `  

- we can have `multiple stages` as `deploying to different different env as different different stage` having common `multiple jobs running in them`

-  in the `release pipeline or CD` here we are trying to  `deploy the application into the infrastructure env which is the dev env` before putting into `production` using the `Azure Resource Management` i.e `ARM template`

- the `ARM Template Task` the create the `infrastructure env and Application deployed to the environment such as the WebUi and Databases `

- then `in the dev env ` we can have the `Task to perform th Selenium Test for UI Testing`

- we also have the `Test Env` which is `similar to the dev Env` but have `additional Performance QA Test Script`

- then we have the `Production Env` where we will be `deploying webapp and SQLDB to the productiuon env as the tasks`      

-  we can have the `another stage as pre-prod` which will check for the `Db Migration Test`

- Also we can have the `another stage` in order to manage the `Azure DevOps Subscription and Resources `

-  we have `defined the Build and Release Pipeline` using the `GUI option`, but we can define `All the stages` as below in the `YAML config` as well 
   
   - `Build Stage`
   
   - `Deploy to Dev Env`
   
   - `Deploy to the Test Env`
   
   - `Deploy to the Prod Env`        

- we can go to `HoussemDellai/WebAppWithDatabaseDemo` github repo followed by `axure-pipelines-ci-cd.yml`

- here we have the `multiple stages defined with repective Jobs` as discussed above 


- **Observations**
  
  - here we can define `trigger` as the `branch in which we want to make the commits`
  
  - here the `pool` means running the `Microsoft Hosted Build Agent or Self Hosted Build Agent` running over a `vmImage`
  
  - then we can heve to  define the `stages` declarative directives  where we have multiple `stage` with the `displayName parameter`
  
  - then we also have `multiple JObs` can `reside inside a single stage` , we can also create the `Multiple Jobs` inside the `Same Stage` in the `release pipeline` which is not possible  in `build pipeline`
  
  - we can have `multiple tasks running inside the Job of a Stage` in the `release pipeline`
  
  - we can select the `add Job Agent` in the `releae stage` so that we can add `multiple jobs having multiple tasks`  
  
  - we can also define the `pool` inside the `stage` as well where we can define the `vmImage` for `particular Stage to run on a particular vm agenmt by the build agent`
  
  - like `stages` , there will  be `Jobs` which consist of `multiple jobs`

  - for the `job`we can also provide the `displayName` attribute  
  
  - where each `job` consist of `steps of multiple task/script`
  
  - for each `task/script` inside the `steps` can also have the `display Name`
  
  - we can also define the `pool` inside the `job` as well where we can define the `vmImage` for `particular Stage to run on a particular vm agenmt by the build agent` for the `Job`
  
  - when we have `multiple Jobs` running under the `same stage` they can `run in parallel`
  
  -   


  
   
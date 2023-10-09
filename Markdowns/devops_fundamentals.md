# Introduction to DevOps 

- `DevOps` is a `setup practices` that `combine` the `software development` and `operations` `which is aim to reduce the system development life cycle` and `provide the continious integration and continious deployments` `pipelines` for `deploying software` to `production`

- `DevOps` `approach` on is to `getting closer` to the `developer ` to the `IT Operations`  

- `continious and improved collaberation being needed` betweeen `business` , `IT-Development` and `IT-Operations`

- `"process improvement"` `play` a `tremendous role`  in `achiving continious Integration and continious Delivery/Deployment` by using `Iterative Development Approach` such as `Agile SDLC`

- `New Technology always be required for automation of deployment` 

- `Organisational changes will always be critical for the success of implementing Devops Transformation`  

- `DevOps` require the `transformational learadership Approach`, `understanding` the `impact on the people and system and processes`


# DevOps Main Objective:- 

- the objective of the `implementing devops practises` is to `achieve full automation from code to production without human intervention`

- `continious intgration` is a `practise` in which `focus on developer integrating the code into a shared environment`

- `continious deployment` on the other hand `deploying changes automatically after passing all the stages of a production pipeline` 


# Devops Process 

- on the `general consensus/agreement in the IT communinity` that exists `DevOps Process start with below process`
  
  - `plan`

      - `plan` ususally starts with `business and planning requirements`
      
      - then we will have `implementing the plan`
      
      - then ` create the solution architecture` and `design` of the `system`
  
  - `build`
        
      - starts when we are `implementing the code changes or configuration changes`
      
      - then we will have to test `implementation of code and configuration changes`
      
      - then we will push the code to the `local` and followed by the `pushing to the centralized repo`
      
      - which will `trigger the continous integration pipeline` 

  
  - `continous integration`
    
    - combine `all different part of the project component` and make it `together` as  `one integrated build` and `executingall automated Test`
    
    - execute the `code quality check` and `security vulnerablity check`
    
    - also can perform the `Unit Testing` and `UAT Testing with selenium` as a `part of the pipeline`   
  
  - `deploy`
    
    - we have `multiple things for the deployment`
      
      - `configuration management`
        
        - here we will confgure the `servers` and `services` that we need to `create on the cloud`
        
        - `security setting that we need to set`
        
        - other `configuration settings` before the `Deployment environment`
      
      - `artifact managenment`
        
        - `artifact management` used to store the `configuration script`
        
        - we can save `build script` or `deployment script` or `system executable` as a part of the `artifact management`  which can be shared between the `different Teams`
      
      - `Deployment Environment`
        
        - we also need to `configure for the deployment environment`
        
        - we can use the  `IaaC` i.e `infrastructure as code`        
      
      - `Package/Deploy to the Environment and perform smoke test`  
        
        - here we can use the `IaaC` i.e `infrastructure as code` to `build and package and deploy to a specific Deployment environment`   
        
        - we can perform the `Smoke Test` just to validate the `deployment done successfully or not`
   
  
  - `operate`
    
    - it is mandetory that to  `when the software is running or operating` we need to monitor  `the application for possible failure` thatcan occure in the `Deployment environment`
    
    - we can perform below operations using monitoring the `running or operating application`
      
      - `security intrusion check`
      
      - `performance check`
      
      - `server failure`
      
      - `application exception` 


  - `continious feedback`  
    
    -  this is basically `distrubuting reports` from the `monitoring the application on the Deployment environment`
    
    -  we can `report failure or error` or `any issue on the Deployment Env System` as `distributed report` to all the `teams`
    
    -  it can also help in `managing the fixes` as it goes back to the `planning stage again`  for `next iteration`

# Our CICD Pipeline will look as below 

- we will be looking into the `coding and check-in` &rarr; `build and package` &rarr; `Testing` &rarr; `Ready to publish to a continious deployment pipeline` &rarr; `continious deployment package the code into and env` &rarr; `configuration management` &rarr; `deploy the package containing the app with respect to the env` &rarr; &rarr;`somke test to checking the deployment been working fine or not i.e code working fine in the deployment env` &rarr; `production`


- `continious deployment will then package the appication code into an environment`

- `it can be a docker image` which need to `store to a container registry`

-  in the `configuration management we can do some configuration changes before deploying the code onto the deployment env`

- once the `configuration been done` then we can `deploy the app code onto deployment env using the deploy option`

- we have the `deployment enviroment` can be as `staging env or UAT env`   before going to the `production` 


# Skills for DevOps

- `Basic Linux and Unix scripting,powershell,bash,python`

- `continous integration /Continious Delivery using Azure DevOps`

- `Performance Testing using JMeter and Gtling`

- `Operation Readinessans:--->Automated Testing,Smoke Testing Sonarqube,selenium for UAT`

- `knownledge of container as Docker and Kubernetes`

- `Query Monitoring tools`

- `Cloud Services such as AWS and Azure`

- `Basic Networking Knowledge`       
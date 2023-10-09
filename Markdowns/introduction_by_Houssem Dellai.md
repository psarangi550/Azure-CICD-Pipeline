
- here we will be discussing about the `Continious Integration and Continious Deployment`

- we will talk about those `pipeline` when we `want to automate` and `create process where build and release of application will be fully automated`

- **For example**

- lets suppose a `bunch of developer` been developing the `web application that connect to a DB`

- we wil go through `from developing the source code` till the `deployment`

- the `developer` in order to `share their application` then `using a source control management such as git/gitlab/azure repo/github/bitbucket`   

- the `code will live into the source control management` 

- this is `essential` as we are `creating CI Pipeline or Built`  

- then we will create the `build for the application` , during the `build or CI Pipeline` the `goal here should be to compile the app and generate the application packages`

-  here we can run the `command` such as `npm package install` or `dotnet build` to `generate the package in here`

-  from the `Build or CI Pipeline` we will generate the `application package` which can be a `dll file` or `jar file` 

- the   output of the `build process or CI ` is to `create the application package`


- then `we have the release pipeline or CD`  the `goal` is to `deploy the application` 

-  we have  `deploy the app` `means` to `put the apps into a web server` 

- but in our case we have a `production env to where we want to deploy the application`

- this is the `basic Deveops implementation of CICD pipeline` but if we want to `go in depth`

-  we have to make sure the `package i.e application package we are building in the CI or build process` should not have bug as we are resusing the `same package on the CD or release pipeline`

- hence it will be `great to run unit test on the CI or Build pipeline which ensure the application package we are building during the CI process does not have any bugs or anamoles `

- if we want to validate the `package being ready and ready to be deployed` then we can also use `static code analyzer such as sonarqube` into the `Build or CI pipeline`

-  and also one the `package being deployed to the release pipeline or CD` we want to run `make some more additional test` before `deploying to production`  

- hence we can use `some other environment test env(UAT/QA/etc)` 

- before moving the `tests into the test env (UAT/QA/etc) which known as the beta tester`  we want to make sure the `deploy p[ackage running ok` from the `PO and developer prospect` 

- once `PO and developer validated that the deployable package running ok` then we can `push the application package to the test env (UAT/QA/Etc)`

- for testing prior to the `test env(UAT/QA/etc)` we can create a `Dev env` where the `Developer and PO` can `test the deployable application`

- the difference between the `Dev Env for developer and PO` and `test env (UAT/QA/etc) for the beta tester` that  `dev` is more `restricted` than the `Test env`

- in the `Dev Env` we have lets suppose `10 resources` ffor testing the `deployable package`

- but in `test env` we can have `thousand of Beta Tester testing the application package`

- once validated by the `beta tester in the Test Env` then we can ddeploy the app into the `production Env`

- in the `release pipeline along with deploying the apps into different env` we can also have `Integration Test`

- along with `Integation Test` we can run the `UI/UAT/QA Test as well in Test env` when we have the `releae CD pipeline`

-  here we have made sure that 
   
   - we have a `build stage which generate an application package`
   
   - which the be tested in the `nultiple stages such as build and release stage with different deployed env` before pushing to the `production`
   

<br/><br/>

- But if we go back to the `developer` , the main goal of `DevOps` is to bring together the `Developer and DBA and IT Team` together

- hence we can add  the `DBA` to the `developer zone` as well 

- we can put the `SQL Script developed by the DBA into the SCM repo`

- the `DBA might not generate the script right away rather he will generate the SQL Script while compiling the code in the Build Process`

-  hence we can say `compling and generate SQL script from the database project in SSMS(SQL server management studio)` to the `Build Pipeline or CI process`

- we can `validate` the `SQL Script` which been `generated` are with the `correct SQL Syntax` by running some `test` on the `compiled SQL Scripts`     

- then we can also move the `correct SQL Syntax script from the Build to Release pipeline` by `including them in the application package artifacts`

- then we can `deploy the SQL Script to the Database whilst we are deploying the web application into the web server in the release pipeline`

-  hence we can add one more stp to `deploy the sytax correct SQL script to the Databases from the release pipeline`

-  we can also run the `unit test`  for the `SQL Sever` which can be written in `tsqlt language` over the `database script` in the `release pipeline beofre deployingto the Databsaes`

-  the `UI/QA test` and `Integration Test` will `validate the application and the schema of the Database`

-  we have the different instance of  `webapp+db` for `production env` and `test env` and `dev env`

-  we have to make sdure the `production env` and `test env` and `dev env` are `isolated from each other`


<br/><br/>

- then we can also add the `IT Pro/operations Team` guys where `developer and DBA` exists

- the `IT Pro/operations Team` wiill check the `infrastructure on which application been running on`     
 
  - `IT Pro/operations Team` wiill care about the `webapp and DB on different different env`   
 
  -   if we are using the `load balancer or virtual machine inside the env` then `IT Pro/operations Team` can  look into that
  
  -  the operations teams will worry about the `provisioning the infrastructure and making sure app fits well in the created infrastructure env`

- the `IT Pro/operation` will have their `own source code` 

- lets suppose the `IT pro/operation team` using the `Terraform(Iaac)` in order to `provision the infrasture env` which is a `different script`

- or they can use the `ARM template which stands for Azure Resource Manager` to `provision of the resources on the Azure` 

- we can also use the `AWS Cloud Formation template` to `provision resources on AWS`   

- each cloud have their `own separate service` to `provision the infrastructure and resources`

-  `IT Pro/operation` can also  push their `source code` into the `SCM Repo`

-  in the `Build or CI Pipeline  stage` we are `validating the terraform script which we created for the infrastructure provisioning`

-  we need to run command as below inorder to `provision the terraform script`
   
   -  `teraform init`
   
   - `terraform plan`

- which will output the `changes it will made` while `check before provisioning/deploy the resource` 

- we can put a task as `check Infra config` as a `stage` to the `build or CI pipeline`

- here we wikk `just validate` the `infra config script` for `syntax checking` before `provisioning/deploying the infrastructure`

- we can send the `infra config in the application packages` from the `build or CI stage` into the `deploy or CD Stage ` 

- then we will be using the `deployed infra config files` , `if we using the cloud then this infra script will deploy the infrastructure env onto the cloud`

-  we can also `provision multiple rsources from the cloud which are needed to run the application in different different environment ` 

<br/><br/>

- we can define `different different stratergy` how to `trigger the Build or CI pipeline`

- when we are `triggering` the `Build or CI pipeline` we can `configure` so that `Build ot CI Pipeline` will be `created or run if there is a new commit push`

- but we can also create so that `we can trigger CI pipeline or Build when there is a pull request (PR) to my master/dev branch` `    

- here `we don't want to commit to the master branch` `rather` we will be `creating` the `development branch`

- each `developer` can take the copy from `master or development branch` and create their own `feature-X` branch for the `particular User Story`

- then once the `feature development complete` then developer will be raising a `pull request`  from the `feature beanch` to the `development branch`

- as soon as we are creating a `PULL Request` then the `Build or CI pipeline will be going to Get Triggered`  

<br/><br/>

- with the `build pipeline` and `release pipeline` we alsso need to `monitor the deployed app` for `potential error`

- once we deployed the application in to the `dev/test env` we can also `monitor` the `application in here` for `improvement`

-  we will be mnitoring the `application been behaving as expected or not`

<br/><br/>

- this `complete loop can be developed in the CICD pipeline`

- few of the `CICD pipelines tools` are 

  - `jenkins`
  
  - `Azure Pipeline` developed by `Microsoft`
  
  - `GitLab CICD`
  
  - `Circle CI`
  
  - `GoCD`

- we can make sure all are `implementation` of `DevOps Loop` will be `automated` , we will be more `confident`

- we are not dependent on the `developer machine` to `deploy the application`
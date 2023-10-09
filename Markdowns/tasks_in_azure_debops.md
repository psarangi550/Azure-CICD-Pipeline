# Whats a Task in Azure DevOps

- `CI and CD` is nothing but the `group of different tasks` that perform `different operation`

- the `task in Azxure DevOps` can be 
  
  - `Running a CMD command` 
  
  - `Running a Powershell command` 
  
  - `copying file and publish then`

  - `we can also have the styling commands as well`
  
- we can `go to task` and `click on the + symbol` to create task from `templates`

- we can see the `integrated application to displat as task` by going to the (visual studio market place )[https://marketplace.visualstudio.com/] &rarr; then selecting the `Azure Devops` then we can see the `List of application that can be used as the Task`

- when we select the `Apps to install` then that will be `installed on the Azzure DevOps Account`

- so we can use the `AWS Tasks` from the `AWS application extension ` from   (visual studio market place )[https://marketplace.visualstudio.com/search?target=AzureDevOps]

- we can integrate the 
  
  - `Jenkins Pipeline as Tasks`
  
  - `AWS application as Task` 
  
  - `google play`
  
  - `cordova build` for `mobile development`
  
  - `Terraform` 
  
  - `Kubernetes extension` 
  
  - `Redgate Readyroll` for `database integration`

- we can go to the [Azure Task Github Repo](https://github.com/microsoft/azure-pipelines-tasks) in order to check `what task microsoft have created from their own`

- we can go to the [Azure Task Github Repo Task Dir](https://github.com/microsoft/azure-pipelines-tasks/tree/master/Tasks) to see the `Task which are automatically created by Microsoft`

-  if we go to the [Azure Task Github Repo DotNetCoreCLIV2 Dir](https://github.com/microsoft/azure-pipelines-tasks/tree/master/Tasks/DorNetCoreCLIv2) directory then we can see the `different ts file which stands for the type script  i.e developed by nodejs`

- hence if we have the `Tasks are developed using the nodejs application` where we can see the `implementation for that task`

- we have the `method` as `execute` in order to `execute the Task` i,e `DotNetCoreCLIV2`

- we can use the `build` / `publish` / `run` / `custom` / `test` /`restore` / `pack` / `push` which are the option available inside the `execute command of the dotenetcore.ts file`  in order to `run the Dotnet packages`  

- for `each command option inside the execute method` will have a `corresponding action` associated with the `command under the execute method of the task dotnetcore.ts scipt`

- we can also `create our own extension and publish to the Azure Marketplace which can be used as the Task inside the Azure Devops Pipeline` 
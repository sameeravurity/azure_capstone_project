
# Task 2: Configure Agent Pool, Implement CI, and Deploy Backend

## Create a New Agent Pool and Self-Hosted Agent in Azure DevOps (VM)

To configure agent pools and agents, follow the official Microsoft learning document:  
🔗 [Configure Agent Pools and Understand Pipeline Styles](https://microsoftlearning.github.io/AZ400-DesigningandImplementingMicrosoftDevOpsSolutions/Instructions/Labs/AZ400_M02_L03_Configure_Agent_Pools_and_Understand_Pipeline_Styles.html)

Once the agent has been configured and is online, edit the pipeline to use the **self-hosted agent** for deployments.

---

## Implementing Continuous Integration for Backend

Open a starter pipeline and use the following YAML configuration:

```yaml
# Starter pipeline
# Start with a minimal pipeline that you can customize to build and deploy your code.
# Add steps that build, run tests, deploy, and more:
# https://aka.ms/yaml

trigger:
  branches:
    include:
    - master

variables:
- name: solution
  value: '**/*.sln'
- name: buildPlatform
  value: 'Any CPU'
- name: buildConfiguration
  value: 'Release'

stages:
- stage: Build
  jobs:
  - job: Build
    pool:
      name: 'azurecpsam'  # Self-hosted agent pool
    steps:
    - task: NuGetToolInstaller@1
    - task: NuGetCommand@2
      inputs:
        restoreSolution: '$(solution)'
    - task: VSBuild@1
      inputs:
        solution: '$(solution)'
        msbuildArgs: >
          /p:DeployOnBuild=true 
          /p:WebPublishMethod=Package 
          /p:PackageAsSingleFile=true 
          /p:SkipInvalidConfigurations=true 
          /p:DesktopBuildPackageLocation="$(build.artifactStagingDirectory)\WebApp.zip" 
          /p:DeployIisAppPath="Default Web Site"
        platform: '$(buildPlatform)'
        configuration: '$(buildConfiguration)'
    - task: PublishBuildArtifacts@1
      inputs:
        PathtoPublish: '$(Build.ArtifactStagingDirectory)'
        ArtifactName: 'drop'
        publishLocation: 'Container'
```

---

## Create a Web App

1. Go to the [Azure Portal](https://portal.azure.com).
2. Create a **Web App** for hosting the backend.

---

## Add Deployment Stage to YAML (Microsoft Hosted Agent)

Modify the existing pipeline YAML to include the **Deploy** stage:

```yaml
- stage: Deploy
  jobs:
  - job: Deploy
    pool:
      vmImage: 'windows-latest'
    steps:
    - task: DownloadBuildArtifacts@1
      inputs:
        buildType: 'current'
        downloadType: 'single'
        artifactName: 'drop'
        downloadPath: '$(System.ArtifactsDirectory)'

    - task: AzureRmWebAppDeployment@5
      inputs:
        ConnectionType: 'AzureRM'
        azureSubscription: 'azuresubs2'
        appType: 'webApp'
        WebAppName: 'quickartbackend'
        packageForLinux: '$(System.ArtifactsDirectory)/drop/WebApp.zip'
```

---

## Test the Deployment

✅ Once deployed, test the backend API by visiting:  
[https://quickartbackend-bxaecjgzgsdfcpcx.eastus2-01.azurewebsites.net/api/home/getproducts](https://quickartbackend-bxaecjgzgsdfcpcx.eastus2-01.azurewebsites.net/api/home/getproducts)

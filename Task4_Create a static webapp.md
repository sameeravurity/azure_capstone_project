# Task: Create a Static Web App to Deploy Frontend Application

## Steps:

- Import the frontend repo to the existing project.
- Create a static web app in the Azure Portal.
- Create a new pipeline to deploy the frontend application to the static web app.

## Azure Pipeline Configuration

```yaml
name: Azure Static Web Apps CI/CD 
 
pr: 
  branches: 
    include: 
      - master 
trigger: 
  branches: 
    include: 
      - master 
 
jobs: - job: build_and_deploy_job 
  displayName: Build and Deploy Job 
  condition: or(eq(variables['Build.Reason'], 
'Manual'),or(eq(variables['Build.Reason'], 
'PullRequest'),eq(variables['Build.Reason'], 'IndividualCI'))) 
  pool: 
    vmImage: ubuntu-latest 
  variables: 
  - group: 
Azure-Static-Web-Apps-witty-water-04b62000f-variable-group 
  steps: 
  - checkout: self 
    submodules: true 
  - task: AzureStaticWebApp@0 
    inputs: 
      azure_static_web_apps_api_token: 
$(AZURE_STATIC_WEB_APPS_API_TOKEN_WITTY_WATER_04B62000F) 
###### Repository/Build Configurations - These values can be 
configured to match your app requirements. ###### 
# For more information regarding Static Web App workflow 
configurations, please visit: https://aka.ms/swaworkflowconfig 
      app_location: "/" # App source code path 
      api_location: "" # Api source code path - optional 
      output_location: "dist/quick-kart-front-end" # Built app 
content directory - optional 
###### End of Repository/Build Configurations ######

##Make sure to add the correct output_location in the pipeline code. Also add the static web app 
api token which refers to the web app created in the azure portal.
##Run the pipeline and check the build and deploy job to get succeeded.
##Open the url of the static web app and check if the application is up and running.



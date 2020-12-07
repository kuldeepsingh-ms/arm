# Deploy infrastructure and application using Azure DevOps Pipeline

1. Create a new Azure Devops project

2. Create a new repository named `AzureARMRepo`

3. Clone the repository using Visual studio.

4. From the `Team Explorer` in visual studio click on `New` under `Solutions`.

5. Create a visual studio project using `Azure Resource Group` project Template. Select Empty template for Resource Manager.

6. Add below mentioned resources to `azuredeploy.json` file.
  - App Service plan
  - Web App
  - SQL Server
  - SQL Database
  
7. Add the names of above resources to `azuredeploy.parameters.json` file.
  - Hint:
  ```
  "parameters": {
    "parametername": {
      "value": "parametervalue"
    }
  ```

8. Update `azuredeploy.json` file to refer resource names from `azuredeploy.parameters.json` file.
  - Hint:
  ```
  "name": "[parameters('parametername')]"
  ```

9. Set up a classic build pipeline on `AzureARMRepo`

10. Run the pipeline and verify the published artifacts.

11. Create a resource group named `rg-demo-07dec2020` from Azure portal.

12. Create a service connection for resource group `rg-demo-07dec2020`.

13. Set up a classic release pipeline to deploy atrifacts from build pipeline
  - Hint: Start with an empty template
  - Hint: Use `ARM Template Deployment` Task
  
14. Run the deployment and verify resources from the Azure portal.

15. Create a multi-stage YAML pipeline to build and deploy infrastructure.
  - Hints: use below structure for build stage
  ```
  - Stages:
    - Stage:Build
      - Job: BuildJob
        - steps:
          - Task: Visual studio build
          - Task: Copy File (Copy files to Artifacts directory)
          - Publish artifacts  
  ```
  - Use below structure for Release stage
  ```
  - stage: DeployARM
  displayName: Deploy Infrastructure (ARM)
  jobs:
  - deployment: DeployARM
    environment: DevDeploy
    variables:
      resourceGroupName: ''
    strategy:
      runOnce:
        deploy:
          steps:
          - task: <AzureResourceManagerTemplateDeployment> Add 'ARM Template Deployment' task

  ```

16. (Optional) Extend pipeline to deploy web application along with ARM template.

17. (Optional) Extend pipeline to deploy SQL database along with web app and ARM template.

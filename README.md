# Deploying Azure DevOps Agents on Azure Container Apps

## Introduction

This project provides an starter solution to modernize your AzDevOps pipeline infrastructure by migrating from IaaS to PaaS using Azure Container Apps.
Optimize costs by reducing the need for VMs. Additionally, it uses Kubernetes Event Driven Autoscaling (KEDA) to auto-scale based on the jobs queue length.

The Container Apps will be deployed in a Container App Environment injected into a vNet, which sets the foundation to stablish private connectivity with your vnet aware resources.

The project uses Azure DevOps pipelines and Bicep to deploy the required Azure resources. Before running the pipeline, you need to create the Azure DevOps Agent pool and an Azure DevOps library with some variables used to configure to agents. These 2 steps are executed manually following the instructions under the Configuration section.

## Prerequisites

- An Azure account
- An Azure DevOps project and Service Connection to your Azure account

## Configuration

1. Create an Azure DevOps Agent Pool:
   - Log in to your Azure DevOps account and go to your project
   - Go to your project settings
   - Go to the "Agent Pools" section in the left-side menu
   - Click on the "Add Pool" button
   - Select "new", type as "Self-hosted" and give the agent pool a name, for example, "azure-container-apps-pool".
   - Click on "Create"
   - Open the Pool and get the PoolId from the URL, you will need it next.

2. Create an Azure DevOps Library:
   - Log in to your Azure DevOps account and go to your project
   - Go to the "Library" section in the left-side menu
   - Click on the "+ Variable Group" button
   - Give the library a name, for example, "azdevops-agent-aca"
   - Fill in the following variables:
     - `azpToken` (pat token - make sure you make this variable a secret)
     - `azpUrl` (url to your az devops project)
     - `azpPool` (pool name given when creating the az devops agent pool)
     - `azpPoolId` (pool id extracted earlier)

3. Set pipeline variables:
    - Fork this repo
    - Edit the file `pipelines\azdevops-agent.yml` and set the right values for:
     - `group`
     - `azureServiceConnection`
     - `resourceGroupName`
     - `location`

## Deployment

To deploy the Azure DevOps agents on Azure Container Apps, create a pipeline based on the .yaml file in this Github repository.

Here are the instructions to create a pipeline:

1. Go to your Azure DevOps organization's Pipelines.
2. Click on New pipeline.
3. Select the source control that contains the pipelines\azdevops-agent.yml.
4. Follow the steps to authorize Azure DevOps to access your Github repository.
5. Select the repository that contains the .yaml file.
6. Select the pipelines\azdevops-agent.yml file.
7. Click on Run.

Wait until the pipeline finishes and that's it! Your Azure DevOps agents are now deployed on Azure Container Apps.

You can follow the steps above again to create a dummy pipeline based on the file pipelines\dummy.yml and see the agent processing jobs.
Key multiple jobs to how KEDA 

Resources deployed:

![image](https://user-images.githubusercontent.com/12474226/216915593-39044b3b-aeb0-454d-a86e-0584e142bce9.png)

Multiple agents created by KEDA after queuing multiple jobs:

![image](https://user-images.githubusercontent.com/12474226/216915815-7f0df19c-7cc8-4fb0-869f-892b9ea0b2f3.png)

## Docker Container

I created my own Docker Container to bootstrap the agent based on Microsoft doc: [Running self-hosted agents in Docker](https://learn.microsoft.com/en-us/azure/devops/pipelines/agents/docker?view=azure-devops).

You might want to add extra layers to the container image to bake in the tools that you need.

## References

- [KEDA Azure Pipelines Scaler](https://keda.sh/docs/2.8/scalers/azure-pipelines/#authentication-parameters)
- [Running self-hosted agents in Docker](https://learn.microsoft.com/en-us/azure/devops/pipelines/agents/docker?view=azure-devops)

## Contributions

Contributions are welcome! If you have any suggestions or bug reports, please open an issue or submit a pull request.

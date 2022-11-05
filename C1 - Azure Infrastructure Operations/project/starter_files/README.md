# Azure Infrastructure Operations Project: Deploying a scalable IaaS web server in Azure

### Introduction

For this project, you will write a Packer template and a Terraform template to deploy a customizable, scalable web server in Azure.

### Getting Started

1. Clone this repository

2. Create your infrastructure as code

3. Update this README to reflect how someone would use your code.

### Dependencies

1. Create an [Azure Account](https://portal.azure.com) 

2. Install the [Azure command line interface](https://docs.microsoft.com/en-us/cli/azure/install-azure-cli?view=azure-cli-latest)

3. Install [Packer](https://www.packer.io/downloads)

4. Install [Terraform](https://www.terraform.io/downloads.html)

### Instructions

1.Create file policyRules.json to write policy rules

2.Login Azure with following CLI command: az login 

3.Create policy definition with following CLI command:
    az policy definition create --name tagging-policy --display-name "Require a tag on resources" --params "{ "tagName1": { "type": "String", "metadata": { "displayName": "Tag Name", "description": "Name of the tag, such as 'requiretag'" },"defaultValue": "tags01"} }" --subscription "2a65313f-b8d3-4216-a0eb-3a05e912d520" --mode Indexed --rules policyRules.json

4.Apply policy to resources with following CLI command: az policy assignment create --policy tagging-policy --params "{ "tagName1": { "value": "tags01" } }"

5.View list of policies applied with following CLI command: az policy assignment list

6.Create a resource group with following CLI command : az group create --name hinhnh-project1-devops-rg --location centralus 

7.Create new server.json file to write image information

8.Get the information of subscription with following CLI command: az account show --query "{ subscription_id: id }"

9.Create a service principal and configure its access to Azure resources and show info about client_id, client_secret, tenant_id following CLI command:   
  az ad sp create-for-rbac --role Contributor --name sp-packer --scopes /subscriptions/36f2e639-6060-4093-bcff-adb55f8da06a --query "{ client_id: appId, client_secret: password, tenant_id: tenant }"  

10.Update the values (subscription_id, client_id, client_secret, tenant_id) that outputed at step 9 for the variables section of the server.json file 

# Using Packer to build image from server.json file:

11.Create image with following CLI command: "packer build server.json". A image will be created in the resource group that was created in step 6 with "managed_image_name" was declared at server.json file.

12.Create new main.tf file to declare the resources that we want to create. Note please add requiretag tag to resource.

13.Create new variables.tf file to define variables. 

# Using Terraform to create resources in the resource group created.

14.Run following CLI command to init: "terraform init". This command performs several different initialization steps in order to prepare the current working directory for use with Terraform.

15.Run following CLI command to create an execution plan, which lets you preview the changes that Terraform plans to make to your infrastructure: terraform plan -out solution.plan

16.Run following CLI command to execute the actions proposed in a Terraform plan:  terraform apply 

17.Run following CLI command to delete generated resources: terraform destroy

### Output

 The resources will be generated in Azure with informations are declared at files: policyRules.json, server.json, main.tf, variables.tf, solution.plan


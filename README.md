# Terraform-Code
create storage account with private end points using terraform in azure
Create Storage Account with Private Endpoints using Terraform in Azure

This guide outlines how to use terraform to provision an azure storage account and securely connect it using private endpoint.
We need to follow the steps below to create storage account with private endpoints using terraform in azure platform.
Required steps to follow.
1.	Define Resource Group – Create a resource group to contain your resources.
2.	Create Storage Account – Define the storage account with necessary configurations.
3.	Set Up Virtual Network (VNet) and Subnet – Configure the VNet and subnet where the private endpoint will reside.
4.	Create Private DNS Zone – Set up a private DNS zone for resolution of the private endpoint.
5.	Deploy Private Endpoint – Link the storage account to a private endpoint.
After completing or creating the TERRAFORM file we need to deploy using terraform deployment as per the steps below.

Initialize Terraform: 
1.	terraform init – it will initialize the code with latest version 
2.	terraform validate – it will check and validate the code.
3.	terraform plan – it will check all the code plan and show if any error in the code before execution of the created code.
4.	terraform apply -- auto-approve – terraform code will be executed and if code is correct, it will be deployed in the azure platform and created required resources successfully.
Please find the terraform Code in my GIT- Repository.

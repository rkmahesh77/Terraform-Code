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
Please find the terraform Code here and in my GIT- Repository as well using the link below.
Provider.tf
# provider block
provider "azurerm" {
  features {}
  # Configuration option
  client_id       = "1f7f6abb-90d4-45d8-b8a3-d90ad6352ddc"
  client_secret   = "mcg8Q~5Ha3SxleDSBYYGPAbnztNkFtXMnjx4.ab3"
  tenant_id       = "38972487-80cb-475f-aad9-6dc1d624d9ab"
  subscription_id = "ef3de26c-d86d-4e3c-8683-bf29150628a2"
}
Resource Group 
rg.tf
resource "azurerm_resource_group" "rg" {
  name     = var.RG-Name
  location = var.loc-name
}
Virtual network
Vnet.tf
resource "azurerm_virtual_network" "vnet" {
  name                = var.network-01
  location            = azurerm_resource_group.rg.location
  resource_group_name = azurerm_resource_group.rg.name
  address_space       = ["10.0.0.0/16"]
}

resource "azurerm_subnet" "subnet" {
  name                 = var.sub-name
  resource_group_name  = azurerm_resource_group.rg.name
  virtual_network_name = azurerm_virtual_network.vnet.name
  address_prefixes     = ["10.0.0.0/24"]
}
resource "azurerm_private_dns_zone" "dnszone" {
  name                = "privatelink.blob.core.windows.net"
  resource_group_name = azurerm_resource_group.rg.name
}
Storage Account
Storage.tf
resource "azurerm_storage_account" "storage" {
  name                     = var.store-acc
  resource_group_name      = azurerm_resource_group.rg.name
  location                 = azurerm_resource_group.rg.location
  account_tier             = "Standard"
  account_replication_type = "LRS"
}
PRIVATE ENDPOINT
private-endpoint.tf
resource "azurerm_private_endpoint" "private_endpoint" {
  name                = var.private-endpoint
  location            = azurerm_resource_group.rg.location
  resource_group_name = azurerm_resource_group.rg.name
  subnet_id           = azurerm_subnet.subnet.id

  private_service_connection {
    name                           = "storage-private-link"
    is_manual_connection           = false
    private_connection_resource_id = azurerm_storage_account.storage.id
    subresource_names              = ["blob"]
  }
}

resource "azurerm_private_dns_zone_virtual_network_link" "dns_link" {
  name                  = "dns-vnet-link"
  resource_group_name   = azurerm_resource_group.rg.name
  private_dns_zone_name = azurerm_private_dns_zone.dnszone.name
  virtual_network_id    = azurerm_virtual_network.vnet.id
}
Variables Declaration
variables.tf
variable "RG-Name" {}
variable "loc-name" {}

variable "network-01" {}
variable "sub-name" {}

variable "private-endpoint" {}
variable "store-acc" {}
Terraform Varfiles
terraform.tfvars
RG-Name  = "STORE-01"
loc-name = "eastus"

network-01 = "VNET"
sub-name   = "Subnet"

private-endpoint = "My-Privte-ep"

store-acc = "maheshrk654321"

# Azure--WS--Create-Azure-Container-Registry-using-Terraform
Create Azure Container Registry (ACR) using terraform
Introduction
Azure Container Registry (ACR) is a managed Docker registry service provided by Microsoft Azure. It allows you to store and manage container images for your applications in a secure and private environment.

In this lab, I will guide you through the process of creating an Azure Container Registry using Terraform. Furthermore, I will demonstrate how to verify its successful deployment within the Azure portal and provide insights on how to utilize it effectively post-creation.

**ACR provides a number of benefits, including:**

Private repository: ACR provides a private Docker registry, which means that you can store your Docker images securely and privately, and only authorized users or services can access them.

High availability: ACR is built on Azure, so it benefits from Azure's global network and high availability features. This means that your container images are always available, and you can easily replicate them across regions for disaster recovery.

Integration with Azure services: ACR integrates seamlessly with other Azure services, such as Azure Kubernetes Service (AKS), Azure Web Apps, and Azure DevOps, making it easy to incorporate ACR into your existing workflows.

Security and compliance: ACR provides built-in security features, such as role-based access control, network security, and encryption at rest, to help you meet your security and compliance requirements.

Geo-replication: ACR allows you to replicate your container images across multiple regions for improved performance and disaster recovery.

To get started with ACR, we are going to use terraform to create a new Azure Container Registry. Once you have a registry, you can push your Docker images to it using the Docker CLI, Azure CLI, or other tools, and manage your images using the Azure portal or a variety of third-party tools.


Technical Scenario
As a Cloud Engineer, you have been asked to store and manage organization application development private container images and helm charts in secure way in cloud so that these are not directly accessible outside of the company network. also, make sure that azure Container Registry should provide organization users with direct control of their container content, with integrated authentication.

Objective
In this exercise we will accomplish & learn how to implement following:

Task-1: Create ACR resource group

Task-2: Configure variables for ACR

Task-3: Create ACR user assigned identity

Task-4: Create Azure Container Registry (ACR) using terraform

Task-5: Create Diagnostics Settings for ACR

Task-6: Lock ACR resource group

Task-7: Validate ACR resource

Task-7.1: Log in to registry

Task-7.2: Push image to registry

Task-7.3: Pull image from registry

Task-7.4: List container images

Task-8: Restrict Access Using Private Endpoint

Task-8.1: Configure the Private DNS Zone

Task-8.2: Create a Virtual Network Link Association

Task-8.3: Create a Private Endpoint Using Terraform

Task-8.4: Validate private link connection using nslookup or dig

Through these tasks, you will gain practical experience on Azure Container Registry.

**Architecture diagram**
Here is the reference architecture diagram of Azure container registry.


![image](https://github.com/Blass2000/Azure--WS--Create-Azure-Container-Registry-using-Terraform/assets/89789502/abdf69c0-3ac5-4d39-b718-594dd7eb631b)

**Prerequisites**
Download & Install Terraform

Download & Install Azure CLI

Azure subscription

Visual studio code

Azure DevOps Project & repo

Terraform Foundation

Log Analytics workspace - for configuring diagnostic settings.

Virtual Network with subnet - for configuring a private endpoint.

Basic knowledge of terraform and azure concepts.

**Implementation details**
Open the terraform project folder in Visual Studio code and creating new file named acr.tf for Azure container registry specific azure resources;

login to Azure

Verify that you are logged into the right Azure subscription before start anything in visual studio code

![image](https://github.com/Blass2000/Azure--WS--Create-Azure-Container-Registry-using-Terraform/assets/89789502/61d2b538-d33c-435e-8e8b-95be6d803660)

*Task-1: Define and declare ACR variables*
This section covers list of variables used to create Azure container registry with detailed description and purpose of each variable with default values.

![image](https://github.com/Blass2000/Azure--WS--Create-Azure-Container-Registry-using-Terraform/assets/89789502/c5f946c7-f0b2-4e96-a923-f866ef8c0d67)

**Variables Prefixed**

Here is the list of new prefixes used in this lab

variable "acr_prefix" {
  type        = string
  default     = "acr"
  description = "Prefix of the Azure Container Registry (ACR) name that's combined with name of the ACR"
}

**Declare Variables**

Here is the list of new variables used in this lab
// ========================== Azure Container Registry (ACR) ==========================

variable "acr_name" {
  description = "(Required) Specifies the name of the Container Registry. Changing this forces a new resource to be created."
  type        = string
}

variable "acr_rg_name" {
  description = "(Required) The name of the resource group in which to create the Container Registry. Changing this forces a new resource to be created."
  type        = string
}

variable "acr_location" {
  description = "Location in which to deploy the Container Registry"
  type        = string
  default     = "East US"
}

variable "acr_admin_enabled" {
  description = "(Optional) Specifies whether the admin user is enabled. Defaults to false."
  type        = string
  default     = false
}

variable "acr_sku" {
  description = "(Optional) The SKU name of the container registry. Possible values are Basic, Standard and Premium. Defaults to Basic"
  type        = string
  default     = "Basic"

  validation {
    condition     = contains(["Basic", "Standard", "Premium"], var.acr_sku)
    error_message = "The container registry sku is invalid."
  }
}
variable "acr_georeplication_locations" {
  description = "(Optional) A list of Azure locations where the container registry should be geo-replicated."
  type        = list(string)
  default     = ["Central US", "East US"]
}

variable "acr_log_analytics_retention_days" {
  description = "Specifies the number of days of the retention policy"
  type        = number
  default     = 7
}
variable "acr_tags" {
  description = "(Optional) Specifies the tags of the ACR"
  type        = map(any)
  default     = {}
}
variable "data_endpoint_enabled" {
  description = "(Optional) Whether to enable dedicated data endpoints for this Container Registry? Defaults to false. This is only supported on resources with the Premium SKU."
  default     = true
  type        = bool
}
variable "pe_acr_subresource_names" {
  description = "(Optional) Specifies a subresource names which the Private Endpoint is able to connect to ACR."
  type        = list(string)
  default     = ["registry"]
}

**Define variables**
Here is the list of new variables used in this lab

dev-variables.tfvar - update this existing file for ACR values for development environment.
![image](https://github.com/Blass2000/Azure--WS--Create-Azure-Container-Registry-using-Terraform/assets/89789502/a843cd9b-2081-4f5f-8d13-eb2dfecc183f)

**output variables**

Here is the list of output variables used in this lab
![image](https://github.com/Blass2000/Azure--WS--Create-Azure-Container-Registry-using-Terraform/assets/89789502/da070578-4961-427a-bd9a-b99ea99be285)


will update this soon - 










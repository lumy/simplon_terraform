# Terraform: Infra As Code


https://registry.terraform.io/browse/providers

To download the binary https://www.terraform.io/downloads.html

Terraform is an open-source infrastructure as code software tool that provides a consistent CLI workflow to manage
hundreds of cloud services. Terraform codifies cloud APIs into declarative configuration files.

The version you are going to use as to be higher than 0.14.8:
```
Terraform v0.14.8
+ provider registry.terraform.io/hashicorp/azurerm v2.46.0

Your version of Terraform is out of date! The latest version
is 0.15.5. You can update by downloading from https://www.terraform.io/downloads.html
```

still new; the teams has not released a 1.0 version and updates are made frequently; but already
famous and present in a lot of stacks.

During this whole subject I advise you to check the help of each command, `terraform --help`, `terraform init --help`...

## Describe your Environment

### Configuration file / Authentification

For terraform to be able to interact with your provider you'll have to use an authentication method.
For scaleway you use a pair of (api_id, api_key), for Azure many methods are available. 

You will use azure client and our subscription id to authenticate
https://registry.terraform.io/providers/hashicorp/azurerm/latest/docs/guides/azure_cli

You can find your subscription id on azure portal or azure_cli
our configuration file should look like this (replace with your subscription id):
```bash
$ cat config.tf
# We strongly recommend using the required_providers block to set the
# Azure Provider source and version being used
terraform {
  required_providers {
    azurerm = {
      source  = "hashicorp/azurerm"
      version = "=2.46.0"
    }
  }
}

# Configure the Microsoft Azure Provider
provider "azurerm" {
  features {}
 
  subscription_id = "00000-0000-00-0000-000"
}
```
Then you should be able to use the command `terraform init` to initialize terraform (which downloads dependencies).

## tfstate

Terraform maintain in a `terraform.tfstate` file the state of your infrastructure.
That is how it work:

  - Terraform read the state form file.
  - Terraform get current state from provider.
  - Terraform compare State and show you what change will be made if needed.

### Example of server file

`.tf` files describes an infrastucture, depending of the provider you'll describe more or less your infra.
Let's see scaleway provider:
```
# some_file.tf
resource "scaleway_server" "git" {
  name  = "git"
  image = "${data.scaleway_image.debian.id}"
   type  = "VC1S"
  tags  = ["prod", "core", "git"]
  public_ip = "51.15.215.4"
  security_group = "85cee0a4-ed39-4125-a16b-03b71e6fc596" 
}
```

## Import Existing

once you described your infrastructure you can map it on an existing one (if there is one)
`terraform import scaleway_server.git fr-par1/00000000-000-00-00-00000`

This will update local state to reflect the configuration on server. It will no update your `.tf` files.

```bash
$ cat test_import.tf
resource "scaleway_server" "git" {
}
$ terraform import scaleway_server.git fr-par1/00000000-000-00-00-00000
$ cat test_import.tf
resource "scaleway_server" "git" {
}
```

## Deploy

Before deploying you can verify what will happens with `terraform plan`.

You can deploy and destroy your infrastructure with: `terraform apply`, `terraform destroy`

Terraform will talk with your provider to create the required configuration/servers.


## Git

We usualy include terraform.tfstate and terraform.tfstate.backup into git repository *!! BUT IT HAS TO BE PRIVATE OR 
ENCRYPTED !!*

And it is the same with your terraform configuration file,
if it is included into the repository it has to be private or encrypted.

Any information that might be sensible need to be encrypted or in a private repo.

Tips: See `git-crypt`

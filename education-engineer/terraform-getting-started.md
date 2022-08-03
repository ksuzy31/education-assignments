# Getting Started with Terraform

Terraform is an open source infrastructure as code (IaC) tool written in HCL (HashiCorp Configuration Language) that allows you to manage, monitor, and provision resources using configuration files.

## Installating Terraform 
To install Terraform, visit [Terraform.io](https://www.terraform.io/downloads.html) and select the appropriate installation method for your operating system. Refer to [this](https://learn.hashicorp.com/tutorials/terraform/install-cli#install-terraform) page to learn more about installing Terraform.

## Verifying the Installation
To verify the installation of Terraform was successful, open a new terminal session and use the `help` command to list the available subcommands.
```shell
$ terraform -help
Usage: terraform [-version] [-help] <command> [args]

The available commands for execution are listed below.
The primary workflow commands are given first, followed by
less common or more advanced commands.
```
## Troubleshooting the Installation
Refer to [this](https://learn.hashicorp.com/tutorials/terraform/install-cli#troubleshoot) page to learn more on troubleshooting the Terraform installation. 


# Terraform Demo using Docker
In this demo, you will use Terraform to provision and destroy an NGINX instance. 

## Prerequisites

- [Docker](https://www.docker.com/products/docker-desktop/)

## Quick Start Steps
Step 1. Start Docker Desktop
```shell
$ open -a Docker
```

Step 2. Create a directory called terraform-demo
```shell
$ mkdir terraform-demo
$ cd terraform-demo
```

Step 3. Create a file for your Terraform configuration code.

```shell
$ touch main.tf
```

Step 4. Enter the following code snippet in main.tf. This will contain the configuration for your module. To learn more about Terraform configuration, refer to this [page](https://learn.hashicorp.com/tutorials/terraform/docker-build?in=terraform/docker-get-started)

```hcl
terraform {
  required_providers {
    docker = {
      source = "kreuzwerker/docker"
    }
  }
}
provider "docker" {
    host = "unix:///var/run/docker.sock"
}
resource "docker_container" "nginx" {
  image = docker_image.nginx.latest
  name  = "training"
  ports {
    internal = 80
    external = 80
  }
}
resource "docker_image" "nginx" {
  name = "nginx:latest"
}
```

Step 5. Initialize Terraform with the `init` command which prepares the working directory for Terraform to run the configuration. To learn more about the `init` command, refer to [this](https://learn.hashicorp.com/tutorials/terraform/docker-build?in=terraform/docker-get-started#initialize-the-directory) page

```shell
$ terraform init
```

Step 6. Apply the configuration to provision the NGINX resource using the `apply` command. Type `yes` when prompted and hit `Enter` to continue. To learn more about the `apply` command, refer to [this](https://learn.hashicorp.com/tutorials/terraform/docker-build?in=terraform/docker-get-started#create-infrastructure) page

```shell
$ terraform apply
```

Step 7. Confirm the NGINX instance is running by either running ```docker ps``` and verifying that the NGINX container is running or by opening ```localhost:80``` in your browser. 


Step 8. To stop the container, run the following command. Type `yes` when prompted and hit `Enter` to continue. To learn more about the `destroy` command, refer to [this](https://learn.hashicorp.com/tutorials/terraform/docker-destroy?in=terraform/docker-get-started#destroy) page

```shell
$ terraform destroy
```

Step 9. Look for a message at the bottom of the output stating `Destroy complete! Resources: destroyed.` to confirm your NGINX instance has been destroyed.

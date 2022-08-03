# Getting Started with Terraform

Terraform is the most popular langauge for defining and provisioning infrastructure as code (IaC).

## Prerequisites 

## MacOS
-[Brew (MacOS Package Manager )](https://brew.sh) or Terraform **version 1.2.6** binary (https://www.terraform.io/downloads)

## Windows
- Terraform **version 1.2.6** binary (https://www.terraform.io/downloads)

## Linux

### CentOS Distributions
- yum-config-manager (installed via yum-utils)
- Terraform CLI (https://www.terraform.io/downloads)

### Debian Distributions
- gpg (installed via gnupg)
- Terraform CLI (https://www.terraform.io/downloads)

### Other Linux Distributions
- Terraform **version 1.2.6** binary (https://www.terraform.io/downloads)

## Installating Terraform 
To install Terraform, visit [Terraform.io](https://www.terraform.io/downloads.html) and use the appropriate method correlating to your operating system.

## Verify the installation
To verify the installation of Terraform was successful, open a new terminal session and list Terraform's available subcommands.
```shell
$ terraform -help
Usage: terraform [-version] [-help] <command> [args]

The available commands for execution are listed below.
The primary workflow commands are given first, followed by
less common or more advanced commands.
```

To learn more about specific commands add it to `terraform --help`

# Terraform Demo using Docker
In this demo, you will use Terraform to provision an Nginx instance using Docker Desktop. 

## Additional Prerequisites

- Docker (https://www.docker.com/products/docker-desktop/)

## Quick Start Steps
Step 1. Start Docker Desktop
``` open -a Docker
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

Step 4. Enter the following code snippet in main.tf. This will contain the main set of configuration for your module. 

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

Step 5. Initialize Terraform with the `init` command which prepares the working directory so Terraform can run the configuration

```shell
$ terraform init
```

Step 6. Provision the NGINX resource with the `apply` command. Type `yes` when prompted and hit `Enter` to continue. 

```shell
$ terraform apply
```

Step 7. Confirm the NGINX instance is running by either running ```docker ps ``` and verifying that the NGINX container is running or by opening localhost:80 in your browser. 


Step 8. To stop the container, run the following command. Type `yes` when prompted and hit `Enter` to continue.

```shell
$ terraform destroy
```

Step 9. Look for a message at the bottom of the output stating `Destroy complete! Resources: destroyed.` for confirmation

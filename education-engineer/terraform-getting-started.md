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

The output should display the following:
```shell
$ terraform init

Initializing the backend...

Initializing provider plugins...
- Reusing previous version of kreuzwerker/docker from the dependency lock file
- Using previously-installed kreuzwerker/docker v2.20.0

Terraform has been successfully initialized!

You may now begin working with Terraform. Try running "terraform plan" to see
any changes that are required for your infrastructure. All Terraform commands
should now work.

If you ever set or change modules or backend configuration for Terraform,
rerun this command to reinitialize your working directory. If you forget, other
commands will detect it and remind you to do so if necessary.
```

Step 6. Apply the configuration to provision the NGINX resource using the `apply` command. Type `yes` when prompted and hit `Enter` to continue. To learn more about the `apply` command, refer to [this](https://learn.hashicorp.com/tutorials/terraform/docker-build?in=terraform/docker-get-started#create-infrastructure) page

```shell
$ terraform apply
```

The output should display the following:
```shell
$ terraform apply

Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
  + create

Terraform will perform the following actions:

  # docker_container.nginx will be created
  + resource "docker_container" "nginx" {
      + attach           = false
      + bridge           = (known after apply)
      + command          = (known after apply)
      + container_logs   = (known after apply)
      + entrypoint       = (known after apply)
      + env              = (known after apply)
      + exit_code        = (known after apply)
      + gateway          = (known after apply)
      + hostname         = (known after apply)
      + id               = (known after apply)
      + image            = (known after apply)
      + init             = (known after apply)
      + ip_address       = (known after apply)
      + ip_prefix_length = (known after apply)
      + ipc_mode         = (known after apply)
      + log_driver       = (known after apply)
      + logs             = false
      + must_run         = true
      + name             = "training"
      + network_data     = (known after apply)
      + read_only        = false
      + remove_volumes   = true
      + restart          = "no"
      + rm               = false
      + runtime          = (known after apply)
      + security_opts    = (known after apply)
      + shm_size         = (known after apply)
      + start            = true
      + stdin_open       = false
      + stop_signal      = (known after apply)
      + stop_timeout     = (known after apply)
      + tty              = false

      + healthcheck {
          + interval     = (known after apply)
          + retries      = (known after apply)
          + start_period = (known after apply)
          + test         = (known after apply)
          + timeout      = (known after apply)
        }

      + labels {
          + label = (known after apply)
          + value = (known after apply)
        }

      + ports {
          + external = 80
          + internal = 80
          + ip       = "0.0.0.0"
          + protocol = "tcp"
        }
    }

  # docker_image.nginx will be created
  + resource "docker_image" "nginx" {
      + id          = (known after apply)
      + latest      = (known after apply)
      + name        = "nginx:latest"
      + output      = (known after apply)
      + repo_digest = (known after apply)
    }

Plan: 2 to add, 0 to change, 0 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

    Enter a value: yes

docker_image.nginx: Creating...
docker_image.nginx: Creation complete after 9s [id=sha256:b692a91e4e1582db97076184dae0b2f4a7a86b68c4fe6f91affa50ae06369bf5nginx:latest]
docker_container.nginx: Creating...
docker_container.nginx: Creation complete after 1s [id=56f48a12b3194c6f014bae95f4e46dc56689ea0824dea38790a43f6673a519ce]

Apply complete! Resources: 2 added, 0 changed, 0 destroyed.
```


Step 7. Confirm the NGINX instance is running by either running ```docker ps``` and verifying that the NGINX container is running or by opening ```localhost:80``` in your browser. 

The page should display the following:  


<img width="477" alt="Screen Shot 2022-08-03 at 5 18 11 PM" src="https://user-images.githubusercontent.com/6539578/182714156-82c612ec-12ee-44b3-9b07-d0455ebdd09e.png">


Step 8. To stop the container, run the following command. Type `yes` when prompted and hit `Enter` to continue. To learn more about the `destroy` command, refer to [this](https://learn.hashicorp.com/tutorials/terraform/docker-destroy?in=terraform/docker-get-started#destroy) page

```shell
$ terraform destroy
```

The output should display the following:
``` shell 
$ terraform destroy
docker_image.nginx: Refreshing state... [id=sha256:b692a91e4e1582db97076184dae0b2f4a7a86b68c4fe6f91affa50ae06369bf5nginx:latest]
docker_container.nginx: Refreshing state... [id=56f48a12b3194c6f014bae95f4e46dc56689ea0824dea38790a43f6673a519ce]

Terraform used the selected providers to generate the following execution plan. Resource actions are indicated with the following symbols:
  - destroy

Terraform will perform the following actions:

  # docker_container.nginx will be destroyed
  - resource "docker_container" "nginx" {
      - attach            = false -> null
      - command           = [
          - "nginx",
          - "-g",
          - "daemon off;",
        ] -> null
      - cpu_shares        = 0 -> null
      - dns               = [] -> null
      - dns_opts          = [] -> null
      - dns_search        = [] -> null
      - entrypoint        = [
          - "/docker-entrypoint.sh",
        ] -> null
      - env               = [] -> null
      - gateway           = "172.17.0.1" -> null
      - group_add         = [] -> null
      - hostname          = "56f48a12b319" -> null
      - id                = "56f48a12b3194c6f014bae95f4e46dc56689ea0824dea38790a43f6673a519ce" -> null
      - image             = "sha256:b692a91e4e1582db97076184dae0b2f4a7a86b68c4fe6f91affa50ae06369bf5" -> null
      - init              = false -> null
      - ip_address        = "172.17.0.4" -> null
      - ip_prefix_length  = 16 -> null
      - ipc_mode          = "private" -> null
      - links             = [] -> null
      - log_driver        = "json-file" -> null
      - log_opts          = {} -> null
      - logs              = false -> null
      - max_retry_count   = 0 -> null
      - memory            = 0 -> null
      - memory_swap       = 0 -> null
      - must_run          = true -> null
      - name              = "training" -> null
      - network_data      = [
          - {
              - gateway                   = "172.17.0.1"
              - global_ipv6_address       = ""
              - global_ipv6_prefix_length = 0
              - ip_address                = "172.17.0.4"
              - ip_prefix_length          = 16
              - ipv6_gateway              = ""
              - network_name              = "bridge"
            },
        ] -> null
      - network_mode      = "default" -> null
      - privileged        = false -> null
      - publish_all_ports = false -> null
      - read_only         = false -> null
      - remove_volumes    = true -> null
      - restart           = "no" -> null
      - rm                = false -> null
      - runtime           = "runc" -> null
      - security_opts     = [] -> null
      - shm_size          = 64 -> null
      - start             = true -> null
      - stdin_open        = false -> null
      - stop_signal       = "SIGQUIT" -> null
      - stop_timeout      = 0 -> null
      - storage_opts      = {} -> null
      - sysctls           = {} -> null
      - tmpfs             = {} -> null
      - tty               = false -> null

      - ports {
          - external = 80 -> null
          - internal = 80 -> null
          - ip       = "0.0.0.0" -> null
          - protocol = "tcp" -> null
        }
    }

  # docker_image.nginx will be destroyed
  - resource "docker_image" "nginx" {
      - id          = "sha256:b692a91e4e1582db97076184dae0b2f4a7a86b68c4fe6f91affa50ae06369bf5nginx:latest" -> null
      - latest      = "sha256:b692a91e4e1582db97076184dae0b2f4a7a86b68c4fe6f91affa50ae06369bf5" -> null
      - name        = "nginx:latest" -> null
      - repo_digest = "nginx@sha256:ecc068890de55a75f1a32cc8063e79f90f0b043d70c5fcf28f1713395a4b3d49" -> null
    }

Plan: 0 to add, 0 to change, 2 to destroy.

Do you really want to destroy all resources?
  Terraform will destroy all your managed infrastructure, as shown above.
  There is no undo. Only 'yes' will be accepted to confirm.

  Enter a value: yes

docker_container.nginx: Destroying... [id=56f48a12b3194c6f014bae95f4e46dc56689ea0824dea38790a43f6673a519ce]
docker_container.nginx: Destruction complete after 0s
docker_image.nginx: Destroying... [id=sha256:b692a91e4e1582db97076184dae0b2f4a7a86b68c4fe6f91affa50ae06369bf5nginx:latest]
docker_image.nginx: Destruction complete after 0s

Destroy complete! Resources: 2 destroyed.
```

Step 9. Look for a message at the bottom of the output stating `Destroy complete! Resources: destroyed.` to confirm your NGINX instance has been destroyed.

## Next Steps

Terraform Orchestator Proxmox
============
This repository serves as a agregator and orcestrator for other proxmox modules.

## 🪜 Repository Structure
> [utilizable/github-actions-semver-tagging](https://github.com/utilizable/github-actions-semver-tagging)
>> [utilizable/terraform-module](https://github.com/utilizable/terraform-module)
>>> [utilizable/terraform-module-proxmox](https://github.com/utilizable/terraform-module-proxmox)
>>>> [utilizable/terraform-orchestator-proxmox](https://github.com/utilizable/terraform-orchestator-proxmox)

## Table of Contents
- [Requirements](#-requirements)
- [Quick start](#%EF%B8%8F-quick-start)
- [Module options](#-module-options)
- [Configuration](#%EF%B8%8F-configuration)
- [Makefile stages](#-make-stages)
- [Repository structure](#-repository-structure-1)
- [Versioning model](#-versioning-model)

## 🧰 Requirements
<sup>[(Back to top)](#table-of-contents)</sup>

Make sure you have installed both - latest docker and gnu make!

  - `Docker` - https://docs.docker.com/desktop/install/
  - `Make` - https://ftp.gnu.org/gnu/make/

## ⚡️ Quick start
<sup>[(Back to top)](#table-of-contents)</sup>

  1. Clone repository
  2. adjust [.env](./build/.env)
  3. execute `make init`

## 📔 Modules
<sup>[(Back to top)](#table-of-contents)</sup>

List of modules that are included:

- [utilizable/terraform-module-proxmox-vm](https://github.com/utilizable/terraform-module-proxmox-vm)
- [utilizable/terraform-module-proxmox-vlan](https://github.com/utilizable/terraform-module-proxmox-vlan)


#### Environment Requirements
```ini
# ./build/.env

TF_VAR_provider_proxmox_username="username"
TF_VAR_provider_proxmox_password="userpass"
__provider_proxmox_scheme="https"
__provider_proxmox_host="pve"
__provider_proxmox_port="8006"
TF_VAR_provider_proxmox_endpoint="${__provider_proxmox_scheme}://${__provider_proxmox_host}:${__provider_proxmox_port}"
```
## ⚙️ Configuration
<sup>[(Back to top)](#table-of-contents)</sup>

You can overload default `.env` configuration file using ENV_FILE variable. 

#### Command Line:
```sh
ENV_FILE=.env.override make apply
```
```sh
export ENV_FILE=.env.override
make apply
```
#### Github Actions:
```sh
- name: Terraform apply
  run: |
    echo "${{ secrets.ENV_PRODUCTION }}" > .env.override
    ENV_FILE=.env.override make apply
```

## 📒 Make stages
<sup>[(Back to top)](#table-of-contents)</sup>

Stages definied in makefile.

- `make prune` - Wipe all docker-related resources associated with the current project,
- `make show` - Show executed containers (dokcer-compose ps -a),
- `make backend` - Setup [MinIO](https://min.io/) S3 backend with pre-definied bucket (`backend_bucket` variable based),
- `make init` - Execute `terraform init` for modules located in `./terraform` inside docker container,
- `make plan` - Execute `terraform plan` for modules located in `./terraform` inside docker container,
- `make apply` - Execute `terraform apply` for modules located in `./terraform` inside docker container,
- `make destroy` - Execute `terraform destroy` for modules located in `./terraform` inside docker container.

## 🗄 Repository structure
<sup>[(Back to top)](#table-of-contents)</sup>

- `./terraform` terraform related resources, workdir for compose-containers,
- `./build/compose.yml` each step contains its own docker container,
- `./Makefile` entrypoint,
- `./build/.env` default configurations.

## 🔖 Versioning model
<sup>[(Back to top)](#table-of-contents)</sup>

Versions have the format `<MAJOR>.<MINOR>(.<PATCH>)?` where:

- `<MAJOR>` Triggered manualy from default branch,
- `<MINOR>` Triggered automaticly after each push from default branch,
- `<PATCH>` Triggered automaticly after each push from fix/[0-9].[0-9].x branch.

# postgres-infrastructure-automation
This repository automates the deployment and configuration of a PostgreSQL cluster on AWS using Terraform and Ansible for seamless scaling and replication management.


# PostgreSQL Infrastructure Automation

This project automates the deployment of PostgreSQL infrastructure on AWS, utilizing Docker, Terraform, and Ansible. It generates and applies configurations for primary and replica PostgreSQL servers, and it provides a REST API for interacting with the infrastructure.

## API Documentation

### 1. `/generate`
**Method:** `POST`  
**Description:** This API generates the Terraform configuration files for the PostgreSQL infrastructure. It takes user input for the instance type and the number of replica instances. It writes the generated configurations to the `terraform_configs` directory.

#### Parameters:
- `instance_type`: (optional) The type of EC2 instance for PostgreSQL servers. Default is `t3.medium`.
- `num_replicas`: (optional) The number of replica PostgreSQL instances. Default is `2`.

#### Example Request Body:
```json
{
  "instance_type": "t3.large",
  "num_replicas": 3
}
```


### 2. `/apply`
**Method:** `POST`
**Description:** This API applies the generated Terraform configurations. It runs the terraform init and terraform apply commands to provision the infrastructure on AWS. This step creates the EC2 instances for the PostgreSQL primary and replica servers, along with associated resources.

#### Parameters:
{}

### 3. `/apply_ansible_configuration`
**Method:** `POST`
**Description:** 
This API generates the Ansible inventory file and playbook based on the Terraform outputs (the IP addresses of the primary and replica PostgreSQL instances). It also modifies the docker-compose.yml and postgresql.conf files to configure PostgreSQL containers based on the input parameters.

This step automates the configuration of PostgreSQL for replication and scaling. The API ensures that both the primary and replica PostgreSQL servers are correctly configured for Docker deployment with the appropriate environment settings.

- Ansible inventory file is generated using the Terraform output (primary and replica instance IP addresses).
- The docker-compose.yml and postgresql.conf files are updated with user-defined values for the Docker image, maximum connections, and shared buffers.

#### Parameters:
- `image_tag`: (optional)  The Docker image tag for PostgreSQL. Default is `postgres:14-alpine`.
- `shared_buffers`: (optional) The shared buffers size for PostgreSQL. Default is `128MB`.
- `max_connection`: (optional)  The maximum number of PostgreSQL connections.Default is `200`.

#### Example Request Body:
```json
{
  "image_tag": "latest",
  "max_connection": "200",
  "shared_buffers": "256MB"
}
```
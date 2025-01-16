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


### 2. `/apply`
**Method:** `POST`
**Description:** This API applies the generated Terraform configurations. It runs the terraform init and terraform apply commands to provision the infrastructure on AWS. This step creates the EC2 instances for the PostgreSQL primary and replica servers, along with associated resources.

#### Parameters:
{}


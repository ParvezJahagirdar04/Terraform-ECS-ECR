# Terraform-ECS-ECR

# Terraform VPC, ECR, ALB, ECS with Auto-Scaling Configuration

## Overview

This repository contains a set of Terraform modules that are used to create a fully functional AWS infrastructure for a production environment. The infrastructure includes a Virtual Private Cloud (VPC), Elastic Container Registry (ECR), Application Load Balancer (ALB), Elastic Container Service (ECS) with EC2 instances, and Auto-Scaling for ECS tasks.

## Modules

### VPC Module
The VPC module creates a Virtual Private Cloud (VPC) with the specified CIDR block. It sets up an isolated network environment where other resources such as subnets, NAT gateways, and internet gateways can be deployed.

- **CIDR Block**: The IP address range for the VPC.
- **Name**: The name assigned to the VPC.

### Subnet Module
The Subnet module creates public and private subnets within the VPC. Public subnets are used for resources that need to be accessible from the internet, while private subnets are used for internal resources.

- **Public Subnet**: 
  - CIDR Block
  - Availability Zone
  - Public IP Mapping
  - Name

- **Private Subnet**: 
  - CIDR Block
  - Availability Zone
  - Name

### Internet Gateway Module
The Internet Gateway (IGW) module creates an internet gateway and attaches it to the VPC. This allows resources in public subnets to connect to the internet.

- **Name**: The name assigned to the internet gateway.
- **VPC ID**: The ID of the VPC to attach the internet gateway to.

### NAT Gateway Module
The NAT Gateway module creates a NAT gateway in a public subnet, allowing resources in private subnets to access the internet while remaining isolated from inbound traffic.

- **Subnet ID**: The ID of the subnet where the NAT gateway is deployed.
- **Name**: The name assigned to the NAT gateway.

### Route Table Module
The Route Table module creates route tables and associates them with subnets. It defines routes for internet traffic through the internet gateway for public subnets and through the NAT gateway for private subnets.

- **Public Route Table**: 
  - VPC ID
  - Subnet ID
  - Name

- **Private Route Table**: 
  - VPC ID
  - Subnet ID
  - Name

### Security Group Module
The Security Group module creates security groups that control inbound and outbound traffic for resources within the VPC.

- **Public Security Group**: 
  - Ingress Rules (Ports, Protocols, CIDR Blocks)
  - Egress Rules (Ports, Protocols, CIDR Blocks)
  - Name

- **Private Security Group**: 
  - Ingress Rules (Ports, Protocols, CIDR Blocks)
  - Egress Rules (Ports, Protocols, CIDR Blocks)
  - Name

### ECR Module
The Elastic Container Registry (ECR) module creates a private Docker registry where Docker images can be stored and managed.

- **Name**: The name of the ECR repository.
- **Tag Mutability**: Determines whether image tags can be overwritten.
- **Scan on Push**: Enables image scanning on push for vulnerabilities.
- **Tags**: A set of tags to apply to the ECR repository.

### IAM Module
The IAM module creates IAM roles and policies required for ECS task execution and task roles.

- **Execution Role**: The role assumed by ECS tasks for pulling images and writing logs.
- **Task Role**: The role assigned to the ECS tasks for accessing AWS resources.
- **Tags**: A set of tags to apply to the IAM roles.

### ALB Module
The Application Load Balancer (ALB) module creates an ALB and configures listeners and target groups. It distributes incoming traffic across multiple ECS tasks.

- **Name**: The name of the ALB.
- **Security Groups**: The security groups associated with the ALB.
- **Subnets**: The subnets to associate with the ALB.
- **Deletion Protection**: Whether to enable deletion protection for the ALB.
- **Idle Timeout**: The idle timeout for the ALB.
- **Target Group**: The target group for routing traffic to ECS tasks.
- **Health Check**: Configuration for health checks on the target group.
- **Listener**: Configuration for listeners that handle incoming traffic.

### ECS Module
The Elastic Container Service (ECS) module creates an ECS cluster and service using EC2 instances. It defines the task definitions, container configurations, and service settings.

- **Cluster Name**: The name of the ECS cluster.
- **Task Definition**: The definition of the ECS task, including CPU and memory requirements.
- **Execution Role ARN**: The ARN of the IAM execution role for ECS tasks.
- **Task Role ARN**: The ARN of the IAM task role.
- **Container Configuration**: The configuration for the container, including image, CPU, memory, ports, and log configuration.
- **Service Configuration**: The configuration for the ECS service, including desired count, network configuration, and load balancer integration.

### Auto-Scaling Module
The Auto-Scaling module configures auto-scaling policies for the ECS service. It adjusts the number of running tasks based on specified metrics and thresholds.

- **Cluster Name**: The name of the ECS cluster.
- **Service Name**: The name of the ECS service.
- **Min Capacity**: The minimum number of tasks.
- **Max Capacity**: The maximum number of tasks.
- **Tags**: A set of tags to apply to the auto-scaling resources.

## Usage

1. Clone this repository.
2. Navigate to the root directory.
3. Customize the `terraform.tfvars` file with your specific values.
4. Initialize the Terraform configuration:
    ```sh
    terraform init
    ```
5. Apply the Terraform configuration:
    ```sh
    terraform apply
    ```

## Tags

All resources created by these modules can be tagged with user-defined tags. This allows for easy identification and management of resources.

## Dependencies

This configuration assumes that you have AWS credentials configured in your environment. Ensure that you have the necessary permissions to create the resources defined in these modules.

## Author

This Terraform configuration was created to provide a robust, production-ready AWS infrastructure using modular and reusable components.

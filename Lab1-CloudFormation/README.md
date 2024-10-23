
# AWS CloudFormation Project: VPC, EC2 Instances, NAT Gateway, and Security Groups

## Table of Contents

- [Introduction](#introduction)
- [Architecture](#architecture)
- [Directory Structure](#directory-structure)
- [Parameters](#parameters)
- [Deployment Instructions](#deployment-instructions)
  - [Prerequisites](#prerequisites)
  - [Deployment](#deployment)
  - [Updating the Stack](#updating-the-stack)
- [Deleting Resources](#deleting-resources)
- [Notes](#notes)

## Introduction

This project creates a secure and scalable network infrastructure on AWS using CloudFormation. The resources deployed include a VPC with public and private subnets, an Internet Gateway, Route Tables, a NAT Gateway, EC2 instances in both subnets, and Security Groups to manage access.

## Architecture

The following resources will be created:

- **VPC**: A Virtual Private Cloud with both Public and Private Subnets.
  - **Public Subnet**: Allows external access through the Internet Gateway.
  - **Private Subnet**: Uses a NAT Gateway to securely access the internet.
- **Internet Gateway**: Provides internet access for resources in the Public Subnet.
- **Route Tables**:
  - **Public Route Table**: Routes traffic to the Internet Gateway.
  - **Private Route Table**: Routes traffic to the NAT Gateway.
- **NAT Gateway**: Allows resources in the Private Subnet to access the internet securely.
- **EC2 Instances**:
  - Public EC2 instance, accessible via the internet.
  - Private EC2 instance, accessible only from the Public instance via SSH.
- **Security Groups**:
  - Public EC2 Security Group: Allows SSH access from a specific IP.
  - Private EC2 Security Group: Allows access from the Public EC2 instance.

## Directory Structure

- `networking.yaml`: CloudFormation template that defines the AWS network infrastructure.
 - `compute.yaml`: CloudFormation template that defines the AWS EC3 and related components
-   `security.yaml`: CloudFormation template that defines the AWS security rules for our  infrastructure.
-  `main_stack.yaml`: CloudFormation template that reuse the others stack with Nested stack format.

## Parameters

The following parameters are used in the template:

| Parameter            | Description                                                     | Type   | Default Value    |
|----------------------|-----------------------------------------------------------------|--------|------------------|
| `VpcCidr`            | CIDR block for the VPC                                          | String | 10.0.0.0/16      |
| `PublicSubnetCidr`   | CIDR block for the Public Subnet                                | String | 10.0.1.0/24      |
| `PrivateSubnetCidr`  | CIDR block for the Private Subnet                               | String | 10.0.2.0/24      |
| `KeyPairName`        | SSH key pair to access the EC2 instances                        | String | -                |
| `AllowedIp`          | The IP address allowed to SSH into the Public EC2 instance      | String | -                |

## Deployment Instructions

### Prerequisites

- AWS CLI installed and configured.
- An SSH key pair created in your AWS account.
- Appropriate IAM permissions to deploy the necessary resources.

### Deployment

1. **Validate the template** (optional): Validate the CloudFormation template before deployment:

    ```bash
    aws cloudformation validate-template --template-body file://template.yaml
    ```

2. **Create the stack**: Deploy the CloudFormation stack by specifying the stack name and parameters:

    ```bash
    aws cloudformation create-stack \
        --stack-name VPCStack \
        --template-body .yaml \
        --parameters file://parameters.json \
        --capabilities CAPABILITY_NAMED_IAM
    ```

3. **Monitor stack creation**: Use the following command to track the progress of the stack creation:

    ```bash
    aws cloudformation describe-stacks --stack-name VPCStack
    ```

### Updating the Stack

If you need to update the stack to modify or add resources:

```bash
aws cloudformation update-stack \
    --stack-name VPCStack \
    --template-body file://template.yaml \
    --parameters file://parameters.json \
    --capabilities CAPABILITY_NAMED_IAM

                              DynamoDB and Aurora Project with Terraform

# Project Overview
This project demonstrates the use of Terraform to set up and manage AWS resources, specifically DynamoDB and Aurora, for efficient data handling and storage. The goal is to showcase the infrastructure as code approach for cloud resource management.

+ Prerequisites
- An AWS account
- Terraform installed on your local machine
- AWS CLI configured with your credentials

++ Project Structure

# Configuration Details

## AWS Provider
First, we define the AWS provider to specify the region and provider version:
```hcl
provider "aws" {
  region = "us-east-1"
}

terraform {
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 4.0"
    }
  }
}
resource "aws_dynamodb_table" "example" {
  name           = "awsome-jon-table"
  billing_mode   = "PAY_PER_REQUEST"
  hash_key       = "ID"

  attribute {
    name = "ID"
    type = "S"
  }

  tags = {
    Name = "awsome-jon-table"
  }
}
resource "aws_rds_cluster" "aurora" {
  cluster_identifier      = "aurora-cluster"
  engine                  = "aurora-mysql"
  master_username         = "dbMasterUser"
  master_password         = "your-password"
  skip_final_snapshot     = true

  tags = {
    Name = "aurora-cluster"
  }
}

resource "aws_rds_cluster_instance" "aurora_instances" {
  count              = 2
  identifier         = "aurora-cluster-${count.index}"
  cluster_identifier = aws_rds_cluster.aurora.id
  instance_class     = "db.r5.large"
  engine             = aws_rds_cluster.aurora.engine
}


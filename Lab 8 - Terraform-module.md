# Lab: Create an EC2 using Terraform Modules

### Task 1: Create the Project Files
```
mkdir terraform-ec2-lab && cd terraform-ec2-lab
```
```
touch main.tf variables.tf 
```

### Task 2: Define main.tf (EC2 using Terraform AWS Module)
```
nano main.tf
```
Paste the following code in **main.tf**
```
provider "aws" {
  region = var.aws_region
}

# -------------------------------
# EC2 INSTANCE MODULE
# -------------------------------
module "ec2_instance" {
  source  = "terraform-aws-modules/ec2-instance/aws"
  version = "5.0.0"

  name          = "yournam-ec2-instance"
  ami           = var.ami_id
  instance_type = var.instance_type
  vpc_security_group_ids = [var.security_group_id]
  subnet_id = var.subnet_id

  tags = {
    Environment = "dev"
  }
}
```
To save the file Press **CTRL + O** Click Enter and Press **CTRL + X**

### Task 3: Define variables.tf (Input Variables)
```
nano variables.tf
```
Paste the following code in **variables.tf**
```
variable "aws_region" {
  description = "AWS region"
  type        = string
  default     = "us-east-2"
}

variable "ami_id" {
  description = "Amazon Machine Image ID for EC2"
  type        = string
  default     = "ami-0cb91c7de36eed2cb" #Change as per your region
}

variable "instance_type" {
  description = "EC2 instance type"
  type        = string
  default     = "t2.medium"
}

variable "subnet_id" {
  description = "Subnet ID where the EC2 will be deployed"
  type        = string
  default     = ""  # Provide a subnet ID or leave it empty for default VPC
}

variable "security_group_id" {
  description = "Security Group ID for EC2"
  type        = string
  default     = ""  # Provide a security group ID # Use your CICD machine's SG ID
}
```
To save the file Press **CTRL + O** Click Enter and Press **CTRL + X**

### Taks 4: Deploy the EC2 Instance
Run the following commands:
```
terraform init       
```
```
terraform apply 
```
Verify the EC2 creation on AWS.
### Lab 5: Destroy the EC2 Instance 
```
terraform destroy -auto-approve
```

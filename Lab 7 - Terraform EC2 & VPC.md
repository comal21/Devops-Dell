
## Launching VPC and EC2 Instance 

### Task-1: Launching VPC and creating subnets
```
cd /home/ubuntu/
```
```
wget https://s3.ap-south-1.amazonaws.com/files.cloudthat.training/devops/terraform-essentials/vpc_lab.zip
```
```
unzip vpc_lab.zip
```
```
cd vpc_lab/
ls
```
Now, Open the files one by one and replace your regions (**Ex:** ap-south-1) and Availability Zones (**Ex:** ap-south-1a)
```
vi vpc.tf
```
```
cat nat.tf
```
No Updated need in `nat.tf`
```
vi vars.tf
```
In `vars.tf` file Update your Region.
```
terraform init
```
```
terraform plan
```
**Note:** In `terraform plan` if it shows any warning just ignore it. But, if it shows any error we need to correct it.

```
terraform apply -auto-approve
```
Once applied, In the Console check that the resources are getting created like `VPC,` `Subnets,` `Internet Gateway` etc....

### Task-2: Launching an EC2 Instance 

Create a `new File` for EC2 Instance
```
vi instance.tf 
```
Add the given lines, by pressing "INSERT" and replace `Yourname-Lab5-ec2` with your name.
```
resource "aws_instance" "example" {
  ami           = var.AMIS[var.AWS_REGION]
  instance_type = "t2.micro"
  # the VPC subnet
  subnet_id = aws_subnet.main-public-1.id
  # the security group
  vpc_security_group_ids = [aws_security_group.allow-ssh.id]
  # the public SSH key
  key_name = aws_key_pair.mykeypair.key_name
  tags = {
    Name = "Yourname-Lab5-ec2"
  }
}
```
save the file using "ESCAPE + :wq!"

Also, Create a new File for EC2 SecurityGroup
```
vi securitygroup.tf
```
Add the given lines, by pressing "INSERT" and replacing your name at `YourName-allow-ssh`
```
resource "aws_security_group" "allow-ssh" {
  vpc_id      = aws_vpc.main.id
  name        = "yourname-allow-ssh"
  description = "security group that allows ssh and all egress traffic"
  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }
  ingress {
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
  tags = {
    Name = "YourName-allow-ssh"
  }
}
```
Save the file using "ESCAPE + :wq!"

Now, Create a new keypair that we use for our EC2 Instance.
```
ssh-keygen -f mykey -t rsa
```
```
ls
```
```
vi key.tf
```
Add the given lines, by pressing "INSERT" and replace with `your name`.
```
resource "aws_key_pair" "mykeypair" {
  key_name   = "YourName-KeyPair"
  public_key = file(var.PATH_TO_PUBLIC_KEY)
}
```
Save the file using "ESCAPE + :wq!"
```
vi vars.tf
```
Add the given lines, by pressing "INSERT" and replace your `region` and add your `Region's AMI` to the list.

##### The first 3 lines are already present. you can add the remaining lines.
```
variable "PATH_TO_PUBLIC_KEY" {
  default = "mykey.pub"
}

variable "AMIS" {
  type = map(string)
  default = {
    us-east-2 = "ami-059d836af932792c3"
    us-west-2 = "ami-0a7d051a1c4b54f65"
    eu-west-1 = "ami-04c58523038d79132"
  }
}
```
Save the file using "ESCAPE + :wq!"
```
terraform fmt
```
```
terraform validate
```
```
terraform plan
```
```
terraform apply -auto-approve
```
Once applied, verify that the `EC2 Instance` is created in the `Custom VPC` that we created in Task-1.

To `SSH` into newly launched EC2, run below command.
```
ssh -i mykey -l ubuntu <New EC2's Public IP>
```
Once verified, `Exit` from Instance.

Once Done, Use the `terraform destroy` command to clean the infrastructure used in this lab.
```
terraform destroy -auto-approve
```
**Note:** Ensure to crosscheck that all the resources are destroyed or else it will charge you. 

Remove the directory and Zip file using `rm -rf`
```
cd ..
rm -rf vpc_lab
```
```
rm -rf vpc_lab.zip
```


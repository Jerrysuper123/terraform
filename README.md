# terraform
there are servers, db, network and storages on the cloud resources

we used CLI and console to create these resources/or infrastructure across different regions and cloud providers. but manually manage them is diffcult, a minor misconfig would cause huge problem. Also manual does not scale. that is why we need terraform.

We use terraform as infrastructure as a code to create, update or delete resources on cloud.
- terraform is cloud provider agonistic
- can work with google, azure and aws or oracle

key concepts
- state file - keep track all resources managed
- providers - allow u to communicate with different cloud providers
- Terraform core - execute plan (code), run Terraform apply

HCL
- define code in hashicorp config language
- terraform init to download provider file
- run terraform plan to see changes to be made
- run terraform apply to apply all the changes. will update the state file to reflect changed resources
- run terraform show to see the updated changes

2 important block
- resource
define specific infrastructure resource to create
- data
query existing infrastructure to get data

best practices:
use pre built community template
store your code in a remote repo

```
# Define the provider, in this case AWS
provider "aws" {
  region = "us-west-2"  # Specify the AWS region
}

# Define a resource (EC2 instance) to be provisioned
resource "aws_instance" "example" {
  ami           = "ami-0c55b159cbfafe1f0"   # The Amazon Machine Image (AMI) ID for the instance
  instance_type = "t2.micro"                # Instance type (e.g., t2.micro is a small instance)
  
  # Optionally, define the security group for the EC2 instance
  security_groups = ["default"]             # Use the default security group

  # Optional: You can add tags for better organization of the resources
  tags = {
    Name = "ExampleInstance"                # Tag name for this instance
  }

  # Enable SSH access to the instance
  key_name = "my-key-pair"                  # Name of the existing key pair for SSH access
  
  # User Data (optional): You can run scripts when the instance starts
  user_data = <<-EOF
                #!/bin/bash
                echo "Hello, Terraform!" > /var/www/html/index.html
                EOF
}

# Output the public IP of the instance after provisioning
output "instance_public_ip" {
  value = aws_instance.example.public_ip  # Return the public IP address of the EC2 instance
}

```




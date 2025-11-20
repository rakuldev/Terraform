# Terraform
Terraform is fundamentally an IaC tool used to automate the process of building cloud infrastructure and do SRE processes like scaling, performance tuning by replacing instance types, and many other tasks without manual intervention.  It solves a lot of problems, not just 
automation of infrastructure creation without manual intervention, but a lot of problems like replacing resources according to the needs, tuning them in accordance to the usage, and a lot more processes which tends to be useful as a team. Sharing of terraform files becomes 
easier, we just can use VCS systems to share code across the team and hence, there won't be confusions among Devops Engineers to provision systems, so the main file approved will always carry the state of how the system should look like.


Overall, there are various overheads which are being solved by terraform in short:
- Lack of transparency of the system's design
- System reliability automation
- Slower provisioning
- Manual destruction of resources to accomodate change

### Installation on Windows
Windows installation of terraform is pretty straightforward. 

Just head to: https://developer.hashicorp.com/terraform/install#windows which will provide 2 different options according to your system's architecture for terraform installation. Also, we can leverage the
package manager <b><i>chocolatey</i></b>, a free open-source tool to perform command-line installation just as we do for Linux. 

A quick tutorial or AI prompt will guide you installing chocolatey. The command to install terraform using chocolatey:
```
choco install terraform
```
and verify the installation using:
```
terraform -v
```

### AWS Connection
As we progress, we are going to work with a cloud provider. AWS is the one in our projects, so there are few steps to be followed inorder to get access to AWS service account that you are going to use to create infrastructure. 
- Go to AWS Management console (make sure to login using an administrator account / root account)
- Click of IAM service
- Choose users and hit on create user
- Choose <i>"Attach Policies"</i> option and add <b>AdministratorAccess</b> and <b>AmazonEC2FullAccess</b> to start.
- Download the <i>Access key</i> & <i>Secret Access key</i> and store it safe.

Download the AWS CLI, and then configure the Secret key and Access key.
```
aws configure
```

## Providers
To start, a provider plugin is an important one as this determines which environment we are going to choose to work with. For instance, 
```
# Can be used in main.tf itself or a seperate file can be created

provider "aws" {
  region = "us-east-1"
}
```
<b>Bonus Tip:</b>
You can leverage: https://registry.terraform.io/providers/hashicorp/aws/latest/docs to write terraform plugins

## Variables
Variables is just like the variables that we use in programming languages, helps to store different types of data and provides the previlages to re-use stuff. Variables can be overridden in multiple ways. The structure of variables in variables.tf would be like:
```
# Often a seperate file creation is suggested.

variable "instance_type" {
  type = string
  defaut = "t2.micro"  
}

# On main.tf
resource "aws_instance" "web" {
  ami_id = "ami-0c7114fa3eac14de1"
  instance_type = var.instance_type
}
```

There are other few ways to override data. 
- The **terraform.tfvars** will be the one carrying variable values. So clearly, variables.tf (definition & declaration sometimes), terraform.tfvars - declaration. Also we can leverage multiple vars file for tests. Like: dev.tfvars, 
*.tfvars but at the end, terraform.tfvars will override the others when given with <i>apply</i> command. 
- Also on run time, while supplying the apply command, we can use: <i>terraform apply --var-file dev.tfvars</i> or any other variable file as per the requirement.

NOTE: We can use the **terraform console** to see the variable values (to determine priority and change as needed)

<b>Bonus Tip:</b>
You can use this website to get the ubuntu ami-id according to the region, architecture and instance type.

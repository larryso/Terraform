# Terraform Tutorial — Part 3 — Terraform Configuration File
<p align="center">
  <img src="../images/terraform_logo.png" />
</p>
As we discussed in the previous part, Terraform will read the configuration file (.tf) and execute the actions according to that. This Configuration file is written in Terraform’s own language called HashiCorp Configuration Language (HCL) which is easily readable for Human and Machine as well and hence it is called Declarative Language. So, first, we will see the basic elements and structure of the Terraform Configuration Language.
## Syntax
First of all, we will discuss the syntax and arguments that are going to be used in the Terraform Configuration file. The following are the basic arguments and syntax that we are going to use in HCL.

* Arguments – It assigns value to the respective name.
* Blocks – All the content inside “{ ... }” which is also called a container.
* Identifiers – All argument names, resource names, variable names, are called Identifiers.
* Comments – Just like any other programming language, # or // or /* ... */ is the string used to comment the lines in HCL.

```
# Output variables - this is a commented linecomment

output "ec2_instance_id" { # "ec2_instance_id" is Identifier
  value = instance.ec2.id # "value" is Argument name and "instance.ec2.id" is Argument Value.
}

resource "aws_instance" "proxy" {
  ami = "ami-c3b64ac23"
  instance_type = "t2.nano"
}
```
## Resources
Just like any other configuration management platform, Basic element of the terraform is an Resource. It will define the infrastructure object like AWS, AZURE or Kubernetes.
```
resource "aws_instance" "proxy" {
  ami = "ami-c3b64ac23"
  instance_type = "t2.nano"
}
```
In the above snippet, “resource” is the keyword that defines the resource type as “aws_instance” and then “proxy” is the local name of the resource so that we can call this resource block as “proxy” on the overall configuration file. Then, in between braces “{ ... }“, we call configuration arguments. like, “ami” declaring the Amazon Machine Image ID and “instance_type” is the type of the AWS instance type.
## Modules
Modules are collection of one or more resources that will help performing a complete cycle of operations. Basically, Modules can have child modules and that can call each other. So, in a single configuration file, we can call many modules with multiple resources to provisioning or automating the configuration part.
```
module "app-server" {
  source = "./app-slaves"
  servers = 5
}
```
The snippet defined above is the “module” block and has “app-server” as the name of the module. and within the braces “{ ... }“, we have “source” that will point the path of the configuration file of the resources. This may either be the local path or remote path from terraform library. “servers” is the argument for the number of instances that are going to be engaged for the operation.
## Providers
As we discussed, Terraform is an Infrastructure Provisional tool. Hence we have to let the Terraform know who is going to provide whichever Resources to build your infrastructure. So, “Provider” is a block name that defines which infrastructure provides and some parameters of the same.
```
provider "aws" {
  version = "~> 2.0"
  region  = "us-east-1"
}
```
In this above snippet, we have defined provider as “aws” and inside the braces “{ ... }“, we define “version” which defines which version of API that can be used by the Terraform. Then, “region” is the keyword that will define which region the terraform can perform operations.
## Variables
Just like any other configuration management system, Terraform is in need of variables that can set some values at anytime of the terraform operation. we have three different variables in the terraform. they are:
* Input Variable – These are used to set values to configure the Infrastructure with or without the type name and default values.
* Output Variable – These are the Variables that are returned after some execution of modules are resources.
* Local variable – Local variables set values can be used within the modules.

Input Variables are mentioned as “variables” and followed by braces “{ ... }“.
```
#Input variables:

variable "vm_ports" {
  type = list(object({
    internal = number
    external = number
    protocol = string
  }))
  default = [
    {
      internal = 80
      external = 443
      protocol = "http"
    }
  ]
}
```

In the above snippet, “vm_ports” is the variable name and it is declared as list type and hence it is mentioned as “list(object({}))“. Then it is also having the default values in “default” section.

```
Output Variables are like returned variables from after execution of the module. Output variable can be declared as “output“.
```
So, the above snippet will have the block called “output” and the name of the variable is “ec2_instance_id” and this will get the value of executed module. For this case, after the EC2instance creation, it will return the ID of the created EC2 instance and that can be used further to configure the particular created EC2 instance.

Local Variables are basically the variables that are made available within the modules and these are mentioned as “locals“

```
# Local Variables

locals {
  operation_name = "app-server"
  instance_id = ec2_instance_id
}
```
So, the above snippet shows the list of local variables called “operation_name” and the “instance_id“. Just note closely that we have connected the output variable name “ec2_instance_id” in the “instance_id” and hence it is going to be a dynamic local variable.

## Data Source
the data source can be used to feed the Terraform configuration with the set of data from outside the Terraform or from other Terraform Configuration files. These data source block can be defined as “data” and followed by braces “{ ... }“.
```
# Data source.

data "aws_ami" "server_ami" {
  most_recent = true
  filter {
    name   = "name"
    values = ["server-*"]
  }
}

#resorce that is using data source.

resource "aws_instance" "server" {
  ami           = "${data.aws_ami.server_ami.id}"
  instance_type = "t2.nano"
}
```
So the snippet shows the data block. which will get AWS AMI from the repository or somewhere where the AMIs are listed and prefixed with “server-“. Then the same data source can be used by the resource mentioned in the snippet as “data.aws_ami.server_ami.id“.

## Terraform Locals
Terraform Locals are named values which can be assigned and used in your code. It mainly serves the purpose of reducing duplication within the Terraform code. When you use Locals in the code, since you are reducing duplication of the same value, you also increase the readability of the code. But how does it differ from a Terraform variable, you may ask. There are a few differences which can be pointed out.  

1. The first difference can be pointed towards the scope. 
    A Local is only accessible within the local module vs a Terraform variable which can be scoped globally. 
2. Another thing to note is that a local in Terraform doesn’t change its value once assigned. 
    A variable value can be manipulated via expressions. This makes it easier to assign expression outputs to locals and use that throughout the code instead of using the expression itself at multiple places. 
#### How to Use Terraform Locals?
* First, declare the local along and assign a value
* Then, use the local name anywhere in the code where that value is needed

``` tf
locals {
  bucket_name = "mytest"
  env         = "dev"
}
```
Here we are assigning two local values. There can be many locals defined within the locals section. These locals are assigned the values to the right. Now, the locals’ name can be used across the code

The values assigned to the locals are not limited to just strings or constants. Expression outputs can also be assigned:

```tf
locals {
  instance_ids = concat(aws_instance.ec1.*.id, aws_instance.ec3.*.id)
}
```

Terraform is not just another open source tool to manage infrastructure. One can define whole infrastructure components and implement the same using a specific coding language called Hashicorp Code Language (HCL). It has a full coding structure within itself, along with its own syntax and terminologies.

For this post, I will focus on a specific, important part of the language. All coding languages have a fundamental concept of ‘variables’ and ‘scopes’. Terraform also has its own version of this called Terraform Locals. There is also a concept of variables on Terraform which can be used to assign dynamic values, but we won’t be covering that in this post. I will instead explain locals and why (and how) they should be used in your Terraform scripts. 

What Are Terraform Locals?
Terraform Locals are named values which can be assigned and used in your code. It mainly serves the purpose of reducing duplication within the Terraform code. When you use Locals in the code, since you are reducing duplication of the same value, you also increase the readability of the code. But how does it differ from a Terraform variable, you may ask. There are a few differences which can be pointed out.  

The first difference can be pointed towards the scope. A Local is only accessible within the local module vs a Terraform variable which can be scoped globally. Another thing to note is that a local in Terraform doesn’t change its value once assigned. A variable value can be manipulated via expressions. This makes it easier to assign expression outputs to locals and use that throughout the code instead of using the expression itself at multiple places. 

If you want to compare Terraform Local to a general programming language construct, it will be equivalent to a local temporary variable declared within a function. Here is an example of local declaration in a Terraform script:
```
locals {
  bucket_name = "${var.text1}-${var.text2}"
}
```
How to Use Terraform Locals? 
Now, let’s move on to see how to use a Terraform local. When you use a Terraform local in the code, there are two parts to it:  

First, declare the local along and assign a value
Then, use the local name anywhere in the code where that value is needed
Let’s look at an example for assigning a local:  
```
locals {
  bucket_name = "mytest"
  env         = "dev"
}
```
Here we are assigning two local values. There can be many locals defined within the locals section. These locals are assigned the values to the right. Now, the locals’ name can be used across the code.   

The values assigned to the locals are not limited to just strings or constants. Expression outputs can also be assigned:

```
locals {
  instance_ids = concat(aws_instance.ec1.*.id, aws_instance.ec3.*.id)
}
```
Locals can be assigned maps as values. Maps are nothing but a list of key value pairs. Here is an example of assigning a map to the local:
```
locals {
  env_tags = {
    envname = "dev"
    envteam = "devteam"
  }
}
```
One thing to note here is that the local name defined has to be unique across the code, because this is the name with which it will be referenced in various places.

Now that we have seen how to declare a local, let’s see how to use one. Since the local has been defined with a specific name, the local can be referenced anywhere with the expression 

local.<declared_name>
```
resource "aws_s3_bucket" "my_test_bucket" {
  bucket = local.bucket_name
  acl    = "private"
 
  tags = {
    Name        = local.bucket_name
    Environment = local.env
  }
}
```

## Built-in Functions
One of the fantastic feature of terraform is built-in functions that perform some logical calculation or some operations.
* Numeric Function – to perform Numeric operations like max, min, floor, abs, and more.
* Collection Function – Set of values functions like list, index, key, concat and more.
* String Function – String related operation like, Join, indent, format, replace, split and more.
* Encoding Function – Encode related functions like URLencode, jsonencode, base64encode and more.
* Filesystem Function – Importantly, filesystem functions like abspath, dirname, basename, filename and more.
* Date & Time Function – Time based functions like timestamp, formatedate and timeadd.
* Hash and Crypto Functions – Encryption/decryption and hashing functions like filemd5, bcrypt,
* Networking Functions – Another important, Network related functions like, cidrhost, cidrnetmask, and cidrsubnet.
* Type Conversion Functions – Casting the types to function like, tomap,tonumber, tostring, tobool and more.
## Terraform Settings
This is basically configuring the terraform itself. Like, mentioning the version of providers’ version or backend. The settings of the Terraform can also be declared as block and the name of the block is “terraform“.
```
terraform {
    backend "akamai" {
        # (akamai backend settings...)
    }
    required_providers {
        aws = ">= 2.2.0"
    }
}
```
Basically, the Terraform block will have the some arguments that are applied for the entire Terraform configuration. So the above snippet have back-end setting for “akamai” and “required_providers” will have the minimum needed version of API that is going to be used in the terraform configuration. In this example, aws >= 2.2.0.
## Overriding the Terraform Files
When you define the same variable in the different Terraform files, we have to override it in order to avoid the Terraform Error. We just have to create an override file called _override.tf or override.tf. Assume we have two files called main.tf and override.tf. and main.tf will have the following code.
```
# main.tf

resource "aws_instance" "server" {
  ami = "ami-ba3c24af"
  instance_type = "t2.nano"
}
```
Then the other file override.tf will have the following snippet.
```
# override.tf

resource "aws_instance" "server" {
  ami = "ami-44aacc3322"
}
```
In the both files, we have ami argument but with different value. But the terraform will take the value from the override file and consolidate the file with following snippet.
```
# Terrafom Consolidated file

resource "aws_instance" "server" {
  ami = "ami-44aacc3322"
  instance_type = "t2.nano"
}
```
his merging will happen in following blocks.

* Resource and Data Block
* Variables Block
* Terraform Block
All togather, have a look at the following terraform file, which is having almost all the component and syntax mentioned above.
```
# Specify the provider and access details
provider "aws" {
  region = "${var.aws_region}"
}

# Create a VPC to launch our instances into
resource "aws_vpc" "default" {
  cidr_block = "10.0.0.0/18"
}

# Output variable
output "address" {
  value = "${aws_elb.web.dns_name}"
}

# Example variable
variable "key_name" {
  description = "Desired name of AWS key pair"
}

variable "aws_region" {
  description = "AWS region to launch servers."
  default     = "us-west-1"
}

# Ubuntu Precise 12.04 LTS (x64)
variable "aws_amis" {
  default = {
    eu-west-1 = "ami-cb03e0f1e"
    us-east-1 = "ami-1d4969a6"
    us-west-1 = "ami-b1f61d4"
    us-west-2 = "ami-d4969880"
  }
}

# Built-in function
resource "aws_key_pair" "auth" {
  key_name   = "${var.key_name}"
  public_key = "${file(var.public_key_path)}"
}
```
The above code snippet covers almost all the concepts mentioned above. But don’t expect that it will work as it is. This is just written to give an example.

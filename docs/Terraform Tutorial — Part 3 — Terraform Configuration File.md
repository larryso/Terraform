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


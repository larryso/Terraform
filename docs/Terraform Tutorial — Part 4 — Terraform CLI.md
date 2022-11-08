# Terraform Tutorial — Part 4 — Terraform CLI
<p align="center">
  <img src="../images/terraform_logo.png" />
</p>

## Terraform Commands
### Terraform Init
### Terraform Plan
As we are creating the Infrastructure as Code, we need to make sure of our action before we execute anything. So, terraform CLI provides an option to check out plan of execution. To see the plan of execution, just run terraform plan followed by some arguments. This will show the plan of action like bellow.

```
$ terraform plan
...
+ aws_instance.digitalvarys
    ami:                         "ami-5a1f6d6c6w752"
    ...

Plan: 1 to add, 0 to change, 0 to destroy.
```

### Terraform apply
The terraform apply command is to apply the state of configuration made in the configuration file (.tf) or actions created by the previous command (terraform plan) to the respective providers. All you need to do is pass the command terraform apply.
### Terraform Destroy
If you want to delete the infrastructure you created by the terraform, you can pass the Terraform CLI command terraform destroy. For more arguments with the terraform destroy, run -help argument.

### Terraform Taint
Terraform Taint command will mark the particular resource as tainted so that it will be destroyed when we run the next terraform apply. This will only modify the state file but not the actual infrastructure. This command is used as terraform taint [address-of-resource]. Along with this command, we have many options available. To see those, pass -help argument with the command and you will get the following output

### Terraform untaint
To undo the terraform taint command, we have a command called terraform untaint [address-of-resource]. So, the additional options of this command are exactly the same as the terraform taint command.

https://digitalvarys.com/complete-terraform-tutorial-part-3-terraform-cli/

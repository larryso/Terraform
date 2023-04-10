# Terraform Tutorial — Part 5 — Terraform State
<p align="center">
  <img src="../images/terraform_logo.png" />
</p>

## Importance of Terraform State
For the fundamental basic usage of Terraform, state is not needed. But for the following important factors, we need the Terraform State

* Resource Mapping
* Metadata
* Remote State with Locking
* Cache mechanism.

### Resource mapping
As we know the Terraform configuration file can create real-world infrastructure. So, we need a kind of mapping between real-world infrastructure and Terraform configuration. If you have a provider that is not having a proper tag name, a terraform state will be very useful as it has its own naming conventions to map the resources of real-world providers.
### Metadata       
When you work with multiple resources, you will have to record all the dependency on the proper order. For example, when you create a subnet and after that servers, you should not delete the servers before the subnet. So, it is very important to know the dependency order. Hence Terraform State will store the metadata of dependency and order of the dependent resource.
### Remote State with Locking


https://digitalvarys.com/complete-terraform-tutorial-part-4-terraform-state/
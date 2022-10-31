# Terraform Tutorial — Part 2 — Providers and Resources

<p align="center">
  <img src="../images/terraform_logo.png" />
</p>

## What is the Provider, and what does it do?
Providers are an important part of Terraform. From one side, they get what they should do from the Terraform, and on the other side, they communicate with real infrastructures, software, etc., to do real actions. They know everything on the real-world side of the Terraform lifecycle. For example, when you write a Terraform code to create an EC2 Instance in AWS, the AWS provider knows how to connect to the AWS API, authenticate to it, which endpoint(s) should be called to create an EC2 Instance, and it knows the AWS API responses and etc. So, if you want to integrate the Terraform with a cloud environment, infrastructure, software, etc., you need its provider to able the Terraform to communicate with it. Terraform comes with a vast of providers, and they are separated into three types. Official Providers are developed, maintained and provided by the HashiCorp team officially. Verified Providers are developed, maintained and distributed by the official team of the software (For example, the Cloudflare provider is provided by the Cloudflare company). Community Providers are developed, maintained and distributed by third parties like me, you or everyone.
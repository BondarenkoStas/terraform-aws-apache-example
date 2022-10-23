Terrafrom Module to provision an EC2 Instance that is running Apache.

Not intended for production use. Just showcasing how to create a custom public module on Terraform Registry

```hcl
terraform {

}

provider "aws" {
  region = "us-east-1"
}

data "http" "myip" {
  url = "http://ipv4.icanhazip.com"
}

module "apache" {
  source          = ".//terraform-aws-apache-example"
  my_ip_with_cidr = "${chomp(data.http.myip.body)}/32"
  public_key      = file(pathexpand("~/.ssh/terraform.pub"))
  instance_type   = "t2.micro"
  server_name     = "Apache Example Server"
}

output "public_ip" {
  value = module.apache.public_ip
}
```
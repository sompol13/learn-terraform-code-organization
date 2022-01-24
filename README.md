## Refactor Monolithic Terraform Configuration
These tutorials are for Terraform users who need to restructure Terraform configurations as they grow. In this tutorial, you will provision two instances of a web application hosted in an S3 bucket that represent production and development environments. The configuration you use to deploy the application will start in as a monolith. You will modify it to step through the common phases of evolution for a Terraform project, until each environment has its own independent configuration and state.

### Separate configuration
Defining multiple environments in the same main.tf file may become hard to manage as you add more resources. The HashiCorp Configuration Language (HCL), which is the language used to write Terraform configurations, is meant to be human-readable and supports using multiple configuration files to help organize your infrastructure.
- `cp main.tf dev.tf`
- `cp main.tf dev.tf`
- `mv main.tf prod.tf`
- Open dev.tf and remove any references to the production environment by deleting the resource blocks with the prod ID. Repeat the process for prod.tf by removing any resource blocks with the dev ID.

### Simulate a hidden dependency
- In `dev.tf`, update your `random_pet` resource's length attribute to `4`.
- You might think you are only updating the development environment because you only changed `dev.tf`, but remember, this value is referenced by both `prod` and `dev` resources.
- `terraform apply`
- `terraform destroy`

### Separate states (Workspaces)
The previous operation destroyed both the development and production environment resources. 
- `terraform workspace list`
- `terraform init`

*Create a dev workspace*
- `terraform workspace new dev`
- `terraform apply -var-file=dev.tfvars`

### Reference
https://learn.hashicorp.com/tutorials/terraform/organize-configuration#create-a-dev-workspace

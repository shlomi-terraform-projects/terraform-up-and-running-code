# Instructions on running this project
This project is divided to 3 sub-projects under:
* global
* stage
  * data-stores
  * services

You need to run terraform for each one of these projects.\
The order matters. first provision the global module, then the data store and then the webserver-cluster.


## Global
Need to pass values to the variable defined in variables.tf file.\
I use local `.tfvars` which I will not push to the repo, to avoid exposing sensitive information.\
Generally, if this is the variables.file:
```hcl
variable "bucket_name" {
  description = "The name of the S3 bucket. Must be globally unique."
  type        = string
}

variable "table_name" {
  description = "The name of the DynamoDB table. Must be unique in this AWS account."
  type        = string
}
```
This will be the `global.tfvars` file:
```hcl
bucket_name = "unique-bucket-name"
table_name = "unique-table-name"
```

### terraform plan
```shell
terraform plan -var-file="global.tfvars"
```

### terraform apply
```shell
terraform apply -var-file="global.tfvars"
```

## Data Store
Create the database

`terraform init -backend-config=backend.conf`

`terraform plan -var-file="data-stores.tfvars"`

`terraform apply -var-file="data-stores.tfvars"`

## Webserver Cluster
Create the webserver cluster

`terraform init -backend-config=backend.conf`

`terraform plan -var-file="webserver-cluster.tfvars"`

`terraform apply -var-file="webserver-cluster.tfvars"`


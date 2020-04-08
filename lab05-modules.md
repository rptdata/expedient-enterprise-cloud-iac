# Lab 4: Modules

Duration: 20 minutes

Configuration files can be separated out into modules to better organize your configuration. This makes your code easier to read and reusable across your organization. You can also use the Public Module Registry to find pre-configured modules.

- Task 1: Refactor your existing code into a local module
- Task 2: Explore the Pubic Module Registry and install a module
- Task 3: Refresh and rerun your Terraform configuration

## Task 1: Refactor your existing code into a local module

A Terraform module is just a set of configuration. For this lab, we'll refactor
your existing configuration so that the webserver configuration is inside a
module.


### Step 5.1.1

We have refactored the Terraform code we have been working on into a module to build out the infrastructure for a LAMP deployment. First run a `Terraform Destroy` of your existing environment.


### Step 5.1.2

Erase the files `main.tf` and `terraform.tfvars` under `terraform_lab`. 

**Do not modify `terraform.tfstate`, `terraform.tfstate.backup` or the `.terraform folder`**

### Step 5.1.3

Copy the `main.tf`, `variables.tf`, `terraform.tfvars`, and `outputs.tf` from the `examples/terraform_ansible` folder to your `terraform_lab` folder. Overwriting the existing files while maintaining state. 

**Copy the individual files**

Uncomment and update your `terraform.tfvars` with your username, password, and vapp name.

```hcl
# vcd_user = "<USERNAME>"
# vcd_pass = "<PASSWORD>"
vcd_org               = "VMWare-Demo-Channel"
vcd_vdc               = "VMWare-Demo-Channel_DUB"
vcd_url               = "https://expedient.cloud/api"
vcd_max_retry_timeout = 240

# #vapp

# vapp_name                = "<MY-VAPP>"
```

### Step 5.1.4

Now run `terraform get` or `terraform init` to install the module. Since we're
just adding a module and no providers, `get` is sufficient, and `init` can
be used too. Even local modules need to be installed before they can be used.

Once you've done that, you can run `terraform apply` again. Notice that the the
instance will be recreated, and its id changed, but everything else should
remain the same.

```shell
terraform apply
```

```text

# module.eec_lamp.vcd_vapp.rptvapp will be created
  + resource "vcd_vapp" "rptvapp" {
...

# module.eec_lamp.vcd_vapp_vm.database will be created
  + resource "vcd_vapp_vm" "database" {
...


# module.eec_lamp.vcd_vapp_vm.load_balancer_server will be created
  + resource "vcd_vapp_vm" "load_balancer_server" {

...

# module.eec_lamp.vcd_vapp_vm.web[0] will be created
  + resource "vcd_vapp_vm" "web" {
...

# module.eec_lamp.vcd_vapp_vm.web[0] will be created
  + resource "vcd_vapp_vm" "web" {
...

# module.eec_lamp.vcd_vapp_vm.web[1] will be created
  + resource "vcd_vapp_vm" "web" {
...

Plan: 5 to add, 0 to change, 0 to destroy.

Do you want to perform these actions?
  Terraform will perform the actions described above.
  Only 'yes' will be accepted to approve.

  Enter a value: yes

Apply complete! Resources: 5 added, 0 changed, 0 destroyed.

```

## Task 2: Explore the Public Module Registry

### Step 5.2.1

HashiCorp hosts a public module registry at: https://registry.terraform.io/

The registry contains a large set of community-contributed modules that you can
use in your own configurations. Explore the registry to see what is available to
you.


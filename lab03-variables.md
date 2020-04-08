# Lab 3: Variables

Duration: 15 minutes

We don't want to hard code all of our values in the main.tf file. We can create a variable file for easier use.

- Task 1: Create variables in a configuration block
- Task 2: Interpolate those variables
- Task 3: Create a terraform.tfvars file

## Task 1: Create a new configuration block for variables

### Step 3.1.1

Add two variables at the top of your configuration file:

```hcl
variable "vcd_user" {}
variable "vcd_pass" {}
```

## Task 2: Interpolate those variables into your existing code

### Step 3.2.1

Update the _provider_ block to replace the hard-coded values with the new
variables.

```hcl
provider "vcd" {
  user              = var.vcd_user
  password          = var.vcd_pass
  org               = "VMWare-Demo-Channel"
  vdc               = "VMWare-Demo-Channel_DUB"
  url               = "https://expedient.cloud/api"
  max_retry_timeout = 240
}
```

### Step 3.2.2

Rerun `terraform plan` for Terraform to pick up the new variables. Notice that you will be prompted for input now. Enter your credentials and run the plan. 

```shell
terraform plan
```

```text
var.vcd_pass
  Enter a value:

var.vcd_user
  Enter a value:


```



## Task 3: Edit your terraform.tfvars file

Variables only defined in the configuration file will be prompted for input. We
can avoid this by creating a variables file. You'll find a file called
`terraform.tfvars` in your Terraform directory with several commented lines.

### Step 3.3.1

Edit the `terraform.tfvars` file by uncommenting the first two key-value pairs:

```
vcd_user = "YOUR_USER"
vcd_pass = "YOUR_PASSWORD"

...
```

Rerun `terraform plan` and notice that this no longer prompts you for input.

### Step 4.3.2

You'll see that there are several other values in `terraform.tfvars`. Uncomment them and make sure to replace the value for...

```hcl
vapp_name = "YOUR_VAPP"
```

 Then add the following matching variables to your `main.tf` file:

```hcl

variable "vcd_org" {}
variable "vcd_vdc" {}
variable "vcd_url" {}
variable "vcd_max_retry_timeout" {}
variable "vapp_name" {}
variable "web_server_name" {}
variable "web_server_catalog_name" {}
variable "web_server_template_name" {}
variable "web_server_memory" {}
variable "web_server_cpus" {}
variable "web_server_cpu_cores" {}
variable "web_server_power_on" {}
variable "web_server_network_name" {}
variable "web_server_network_type" {}
variable "web_server_network_ip_allocation" {}
variable "web_server_network_ip_is_primary" {}
```

### Step 4.3.3

Edit your resource block with these changes as well.

```hcl
provider "vcd" {
  user              = var.vcd_user
  password          = var.vcd_pass
  org               = var.vcd_org
  vdc               = var.vcd_vdc
  url               = var.vcd_url
  max_retry_timeout = var.vcd_max_retry_timeout
}

resource "vcd_vapp" "rptvapp" {
  name     = var.vapp_name
  power_on = false
}


resource "vcd_vapp_vm" "web" {
  vapp_name     = vcd_vapp.rptvapp.name
  name          = var.web_server_name
  catalog_name  = var.web_server_catalog_name
  template_name = var.web_server_template_name
  memory        = var.web_server_memory
  cpus          = var.web_server_cpus    
  cpu_cores     = var.web_server_cpu_cores 
  power_on      = var.web_server_power_on

  network {
    type               = var.web_server_network_type
    name               = var.web_server_network_name
    ip_allocation_mode = var.web_server_network_ip_allocation
    is_primary         = var.web_server_network_ip_is_primary
  }

  depends_on = [vcd_vapp.rptvapp]
}
```

After making these changes, rerun `terraform plan`. You should see that there
are no changes to apply, which is correct, since the variables contain the same
values we had previously hard-coded:

```text
...

No changes. Infrastructure is up-to-date.

This means that Terraform did not detect any differences between your
configuration and real physical resources that exist. As a result, no
actions need to be performed.
```

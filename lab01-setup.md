# Lab 1: Lab Setup

Duration: 20 minutes

- Task 1: Connect to the Student Workstation
- Task 2: Verify Terraform
- Task 3: Generate your first Terraform Configuration
- Task 4: Use the Terraform CLI to Get Help
- Task 5: Apply and Update your Configuration

## Task 1: Connect to the Student Workstation

In the previous lab, you learned how to connect to your workstation.

One you've connected, make sure you've navigated to the `/home/iac_expedient/workspace/terraform_lab` directory. This is where we'll do all of our work for this training.

## Task 2: Verify Terraform installation

### Step 1.2.1

Terraform is already installed, but here is a link to [Download Terraform](https://www.terraform.io/downloads.html)

Run the following command to check the Terraform version:

```shell
terraform -version
```

You should see:

```text
Terraform v0.12.21
```

## Task 3: Generate your first Terraform Configuration

### Step 1.3.1

In the `/home/iac_expedient/workspace/terraform_lab` directory, create a file titled `main.tf` to create an EEC instance.


Your final `main.tf` file should look similar to this with different values:

Replace MY_USERNAME, MY_PASSWORD, and MY_VAPP with your own values.

```hcl
provider "vcd" {
  user              = "MY_USERNAME"
  password          = "MY_PASSWORD"
  org               = "VMWare-Demo-Channel"
  vdc               = "VMWare-Demo-Channel_DUB"
  url               = "https://expedient.cloud/api"
  max_retry_timeout = 240
}

resource "vcd_vapp" "rptvapp" {
  name     = "MY-VAPP"
  power_on = false
}


resource "vcd_vapp_vm" "web" {
  vapp_name     = vcd_vapp.rptvapp.name
  name          = "Web Server"
  catalog_name  = "VMs-and-Templates-Catalog-DUB"
  template_name = "CentOS-7-Vault-Enterprise"
  memory        = 2048
  cpus          = 1
  cpu_cores     = 1
  power_on      = true

  network {
    type               = "org"
    name               = "Demo05-10.253.5.0"
    ip_allocation_mode = "DHCP"
    is_primary         = true
  }

  depends_on = [vcd_vapp.rptvapp]
}
```

Don't forget to save the file before moving on!

## Task 4: Use the Terraform CLI to Get Help

### Step 1.4.1

Execute the following command to display available commands:

```shell
terraform -help
```

```text
Usage: terraform [-version] [-help] <command> [args]

The available commands for execution are listed below.
The most common, useful commands are shown first, followed by
less common or more advanced commands. If you're just getting
started with Terraform, stick with the common commands. For the
other commands, please read the help and docs before usage.

Common commands:
    apply              Builds or changes infrastructure
    console            Interactive console for Terraform interpolations
    destroy            Destroy Terraform-managed infrastructure
    env                Workspace management
    fmt                Rewrites config files to canonical format
    get                Download and install modules for the configuration
    graph              Create a visual graph of Terraform resources
    import             Import existing infrastructure into Terraform
    init               Initialize a Terraform working directory
    output             Read an output from a state file
    plan               Generate and show an execution plan

    ...
```
* (full output truncated for sake of brevity in this guide)


Or, you can use short-hand:

```shell
terraform -h
```

### Step 1.4.2

Navigate to the Terraform directory and initialize Terraform
```shell
cd /home/iac_expedient/workspace/terraform_lab
```

```shell
terraform init
```

```text
Initializing provider plugins...
...

Terraform has been successfully initialized!
```

### Step 1.4.3

Get help on the `plan` command and then run it:

```shell
terraform -h plan
```

```shell
terraform plan
```

## Task 5: Apply and Update your Configuration

### Step 1.5.1

Run the `terraform apply` command to generate real resources in AWS

```shell
terraform apply
```

You will be prompted to confirm the changes before they're applied. Respond with
`yes`.

### Step 1.5.2

Use the `terraform show` command to view the resources created.

### Step 1.5.3

Terraform can perform in-place updates on your instances after changes are made to the `main.tf` configuration file.

Change the CPU and Memory of the servers:

```hcl

resource "vcd_vapp_vm" "web" {
  vapp_name     = vcd_vapp.rptvapp.name
  name          = "Web Server"
  catalog_name  = "VMs-and-Templates-Catalog-DUB"
  template_name = "CentOS-7-Vault-Enterprise"
  memory        = 2048
  cpus          = 1 # update this value 1 > 2
  cpu_cores     = 1 # update this value 1 > 2
  power_on      = true

  network {
    type               = "org"
    name               = "Demo05-10.253.5.0"
    ip_allocation_mode = "DHCP"
    is_primary         = true
  }

  depends_on = [vcd_vapp.rptvapp]
}
```

### Step 1.5.4

Plan and apply the changes you just made and note the output differences for additions, deletions, and in-place changes.

```shell
terraform apply
```

You should see output indicating that vcd_vapp.rptvapp.name will be modified:

```text
...

# vcd_vapp_vm.web will be updated in-place
  ~ resource "vcd_vapp_vm" "web" {
        accept_all_eulas               = true
        catalog_name                   = "VMs-and-Templates-Catalog-DUB"
        computer_name                  = "Vault-001"
      ~ cpu_cores                      = 1 -> 2
      ~ cpus                           = 1 -> 2
...
```

When prompted to apply the changes, respond with `yes`.

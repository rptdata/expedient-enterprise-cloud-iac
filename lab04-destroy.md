# Lab 04: Destroy

Duration: 5 minutes

You've now learned about all of the key features of Terraform, and how to use it
to manage your infrastructure. The next command we'll use is `terraform
destroy`. As you might guess from the name, this will destroy all of the
infrastructure managed by this configuration.

- Task 1: Destroy your infrastructure

## Task 1: Destroy your infrastructure

### Step 1.1.1

Run the command `terraform destroy`:

```shell
terraform destroy
```

```text
# ...


An execution plan has been generated and is shown below.
Resource actions are indicated with the following symbols:
 - destroy

Terraform will perform the following actions:

  # vcd_vapp.rptvapp will be destroyed
  - resource "vcd_vapp" "rptvapp" {
   ...

  # vcd_vapp_vm.web will be destroyed
  - resource "vcd_vapp_vm" "web" {
   ...

Plan: 0 to add, 0 to change, 2 to destroy.

Do you really want to destroy all resources?
  Terraform will destroy all your managed infrastructure, as shown above.
  There is no undo. Only 'yes' will be accepted to confirm.


 Enter a value: yes

vcd_vapp_vm.web: Destroying... [id=urn:vcloud:vm:b0be1f3b-7eea-4efd-993c-a358fac33647]
vcd_vapp_vm.web: Still destroying... [id=urn:vcloud:vm:b0be1f3b-7eea-4efd-993c-a358fac33647, 10s elapsed]
vcd_vapp_vm.web: Still destroying... [id=urn:vcloud:vm:b0be1f3b-7eea-4efd-993c-a358fac33647, 20s elapsed]
vcd_vapp_vm.web: Destruction complete after 25s
vcd_vapp.rptvapp: Destroying... [id=urn:vcloud:vapp:8c7fdf68-26ee-4ce6-b54c-16d21b3cc1e2]
vcd_vapp.rptvapp: Destruction complete after 9s

Destroy complete! Resources: 2 destroyed.

```

You'll need to confirm the action by responding with `yes`. You could achieve
the same effect by removing all of your configuration and running `terraform
apply`, but you often will want to keep the configuration, but not the
infrastructure created by the configuration.

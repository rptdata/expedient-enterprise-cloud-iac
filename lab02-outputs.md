# Lab 2: Outputs

Duration: 10 minutes

Outputs allow us to query for specific values rather than parse metadata in `terraform show`.

- Task 1: Create output variables in your configuration file
- Task 2: Use the output command to find specific variables

## Task 1: Create output variables in your configuration file 

### Step 2.1.1

Create two new output variables in your `main.tf` file named "web_server_name" and "web_server_network_ip" to output the instance's web_server_name and web_server_network_ip attributes

```hcl

output "web_server_name" {
  value = vcd_vapp_vm.web.name
}

output "web_server_network_ip" {
  value = vcd_vapp_vm.web.network[0].ip
}
```

### Step 2.1.2

Run the refresh command to pick up the new output

```shell
terraform refresh
```

## Task 2: Use the output command to find specific variables

### Step 2.2.1 Try the terraform output command with no specifications

```shell
terraform output
```

### Step 2.2.2 Query specifically for the web_server_name

```shell
terraform output web_server_name
```

### Step 2.2.3 Wrap an output query to ping the IP

```shell
ping $(terraform output web_server_network_ip)
```

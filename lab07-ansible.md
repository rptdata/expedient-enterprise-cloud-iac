# Lab 7: Configuration Managment with Ansible

Duration: 20 minutes

- Task 1: Connect to the Student Workstation
- Task 2: Get Ansible IP information from Terraform State
- Task 3: Run Ansible playbook to configure Web Server, Load Balancer and Database Server

## Task 1: Connect to the Student Workstation

### Step 7.1.1

In the previous lab, you learned how to connect to your workstation with either VSCode, SSH, or the web-based client.

One you've connected, make sure you've navigated to the `/home/iac_expedient/workspace/terraform_lab` directory. This is where we'll do all of our work for this training.

## Task 2: Get Ansible IP information from Terraform State

### Step 7.2.1

Run the following command to check the Ansible version:

```shell
ansible --version
```

You should see:

```text
ansible 2.5.1
```

Disable ssh key checking and python interpreter by running

```
export ANSIBLE_PYTHON_INTERPRETER=auto_silent
export ANSIBLE_HOST_KEY_CHECKING=False
```

Copy the `terraform.py`, `terraform.d` folder, and `ansible_inventory.tf` script from `lab` to `terraform_lab`

### Step 7.2.2 - Configure the LAMP stack

The playbook that will be run is located in the `ansible` folder

- install-stack.yml

This playbook will install and configure all the necessary packages on the Apache, MySQL (MariaDB) and HAProxy instances. It will also configure iptables and create a sample index.html file to illustrate the load balancing.


Run the playbook from the `terraform_lab` directory:
```
terraform init
terraform apply

EEC
ansible-playbook -k -i terraform.py ../../ansible/install-stack.yml 
```

Enter in the credentials for each of the servers when prompted:

```text
SSH User: packer
```

Once the playbook has completed successfully, you can reach the site by going to
```
http://<public_ip_of_load_balancer>:8080
```
Refresh the site several times to see the index.html files of both web servers behind the load balancer.

Finally run a `Terraform Destroy` of your environment.


# How to use this playbook

### _Prerequisites:_

###### 1. Install ansible on your machine

###### 2. Ensure the target machine can be reached from the machine where you run the playbook with the user you specify in the playbook using ssh without password

> You can create the ssh key pair and add the public key to the ~/.ssh/authorized_keys file on the target machine.

```sh
# Create the ssh key pair on your machine
    ssh-keygen -t rsa

    #Run the following command on your machine to add the public key to the ~/.ssh/authorized_keys file on the target machine
    ssh-copy-id -i ~/.ssh/id_rsa.pub <user>@<target_machine_ip>

    #Test the ssh connection from your machine to the target machine
    ssh <user>@<target_machine_ip>
```

###### 3. Ensure the user you specify in the playbook has sudo access without password on the target machine

```sh
# add the user to the sudoers file on the target machine
    sudo usermod -aG sudo <user>

    #Modify the sudoers file on the target machine to allow the user to access the target machine without password
    sudo visudo

    #Add the following line to the sudoers file on the target machine
    <user> ALL=(ALL) NOPASSWD:ALL

    #Test the sudo access on the target machine
    sudo -i
```

###### 4. Modify or Update the Inventory file

```sh
# Modify or Update the Inventory file on your machine
    nano inventory.ini

    [rouyi]
    <target_machine_ip> ansible_user=<user>
```

### _To run the playbook_

###### 1. Run the playbook

```sh
# Run the playbook on your machine 
    ansible-playbook -i inventory.ini main.yml
```

###### 2. To start the application, go to <app_dir> which the default values is "/java/RuoYi-Vue"

```sh
   cd <app_dir>
   sh start.sh
```

### _To use Ansible Vault_

###### 1. To create the vault secret, use the following command 

```sh
  ansible-vault create group_vars/all/vault.yml
```

###### 2. To update the vault secret, use the following command, my default password is `P@$$w0rd`

```sh
  ansible-vault edit group_vars/all/vault.yml
```

###### 3. To run with vault secret, my default password is `P@$$w0rd`

```sh
  ansible-playbook -i inventory.ini main.yml --ask-vault-pass
```

### _Unit Test_

###### 1. Update the variable (vars) accordingly in assert.yml

###### 2. To perform the unit test, run the following command, my default password is `P@$$w0rd`

```sh
  ansible-playbook -i inventory.ini assert.yml --ask-vault-pass
```

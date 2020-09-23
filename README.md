- [Ansible Demo](#ansible-demo)
  - [Configuring a Windows target](#configuring-a-windows-target)
    - [Ansible User Account](#ansible-user-account)
    - [Configuring WinRM](#configuring-winrm)
  - [Installing and Configuring the Ansible Control Machine](#installing-and-configuring-the-ansible-control-machine)
  - [Getting and Using Demo Resources](#getting-and-using-demo-resources)
  - [Resources](#resources)

# Ansible Demo
Resources for Ansible demo

## Configuring a Windows target
Configuring a Windows target to be managed by Ansible involves two parts - defining/creating an account that Ansible will use and configuring WinRM on the target

### Ansible User Account
1. Create a user account:  `net user ansible "@ns1bl3isg00d!" /add`
2. Add user to the local administrators group:  `net localgroup "administrators" "ansible" /add`

### Configuring WinRM
**NOTE: The script used for this part configures WinRM in a very insecure fashion.  While this may be appropriate for home lab environments, it's not for production environments**
1. Download and run the configuration script 
```powershell
$url = "https://raw.githubusercontent.com/ansible/ansible/devel/examples/scripts/ConfigureRemotingForAnsible.ps1"
$file = "$env:temp\ConfigureRemotingForAnsible.ps1"
(New-Object -TypeName System.Net.WebClient).DownloadFile($url, $file)
powershell.exe -ExecutionPolicy ByPass -File $file
```

## Installing and Configuring the Ansible Control Machine
These instructions are for CentOS 8.  You can find instructions for other platforms [here](https://docs.ansible.com/ansible/latest/installation_guide/index.html)
1. Add the EPEL repository:  `yum install epel-release`
2. Install Ansible:  `yum install ansible`
3. Confirm installation and version:  `ansible --version`
4. Edit `/etc/ansible/hosts`
5. Add an inventory section for the Windows machine
```shell
# Ansible Demo Inventory
[demowin]

192.168.20.205
```
6. Add a variables section so Ansible knows how to connect to it properly and what account to use
```shell
# Ansible Demo Variables
[demowin:vars]

ansible_connection=winrm
ansible_port=5985
ansible_user=ansible
ansible_password=@ns1bl3isg00d!
```
7. Save the file
8. Test that it works using the `win_ping` module: `ansible all -m win_ping`

## Getting and Using Demo Resources
The easiest way to get the demo resources onto the Control Machine would be to clone this repo: `git clone https://github.com/jpboyce/ansible-demo.git`.  Update the yml files as necessary - specifically `powershell.yml` and `notepadplusplus.yml` will need to have the path attributes updated.

To run one of the playbooks on the Control Machine, use the command: `ansible-playbook <playbookfilename>`.  For example: `ansible-playbook sysinternals.yml`

## Resources
* [Ansible Documentation](https://docs.ansible.com/ansible/latest/index.html)
* [Ansible Examples](https://github.com/ansible/ansible-examples)
* [Ansible Galaxy](https://galaxy.ansible.com/) (Community Contributions)
* [Chef Documentation](https://docs.chef.io/)
* [Chef Examples](https://github.com/chef-cft/chef-examples)
* [Chef Supermarket](https://supermarket.chef.io/) (Community Contributions)
* [Puppet Documentation](https://puppet.com/docs/puppet/latest/puppet_index.html)
* [Puppet Forge](https://forge.puppet.com/)  (Community Contributions)
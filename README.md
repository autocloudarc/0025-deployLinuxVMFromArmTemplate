## DESCRIPTION
This solution demonstrates deploying a CentOS VM using an ARM template, a template parameters file and the Azure CLI.
The VM will be deployed to an existing VNET, subnet, NIC w/public IP, storage account (used as the diagnostics storage account), and an availability set.
The authentication method used will be SSH, so an SSH public key must already exist also and be available as one of the deployment parameters.

### Create a public IP address resource
az network public-ip create --resource-group rgAdatumDev## --name azreaus2lx##01-pip --allocation-method Static
### Create a network interface and associate it with the public ip address azreaus2lx##01-pip
az network nic create --resource-group rgAdatumDev## --vnet-name AdatumDev-VNET## --subnet LNUX## --name lxnic##04 --public-ip-address azreaus2lx##04-pip

### Copy the results from the commands below to your favorite text editor. These will be used as parameter values
az vm availability-set list -g rgAdatumDev## - -query [].id -o tsv | grep LX
az network nic list -g rgAdatumDev##  - -query [].id | grep lxnic0004
az storage account list -g rgAdatumDev##  - -query [].name -o tsv
cat ~/.ssh/linux.user.sshkey.pub

### 4.3.5	Deploy the new azreaus2lx##04 CentOS 7.4 VM to Azure using the command below in the Azure CLI:
az group deployment create –resource-group rgAdatumDev00 - -template-file ~/lxdir/azuredeploy.json - -parameters ~/lxdir/params.json

### 4.3.6	If the deployment is validated, you will be prompted for the following parameters.
1. Enter your 2-digit student number, i.e. ## (00-16)
2. Enter the 5-character region code, ie. eaus2 for East US 2 using the hash table provided in section 3.5.2 of the delivery guide.
3. Please enter the SSH rsa public key file as a string. For example, use the output from: cat ~/.ssh/linux.user.sshkey.pub

### 4.3.7	After the VM is deployed, log into the new VM using the commands below to first retrieve the public IP address of the VM, then authenticate to the VM.

# Get public ip address for azreaus2lx##01-pip
az network public-ip show -g rgAdatumDev## -n azreaus2lx##04-pip --query "ipAddress" -o tsv

# Authenticate to the VM
ssh -i ~/.ssh/linux.user.sshkey linux.user@<public IP Address>


REQUIREMENTS:

1. A Windows Azure subscription

2. The latest version of Azure CLI, either the a local client version or the cloud shell.

**FEEDBACK**
Feel free to ask questions, provide feedback, contribute, file issues, etc. so we can make this even better!
# CLI

Azure provides a CLI `az` which allows you to manipulate all Azure resources through just the terminal.

**Important Terms:**

`az login` - This let's you login to your Azure resources. For example, in a corporate setting you would need to login to access company clusters. This is REQUIRED or else you can't access any Azure resources at all
`az account` - This is just your standard Azure account that manages your billing, identity, and access spaces
`az group` - In Azure, a Resource Group is a logical container (folder) that contains all related resources for a specific use case (ie. backend-server resource group only provides access to azure resources that are used by that backend server)
`az vm` - Manages the Virtual Machines
`az sshkey` - Manages the secure cryptographic keys used to connect to the VMs. On creation, the private key is automatically stored in your laptop while the public key is uploaded to the cloud
`az network` - This helps manage networks, firewalls, IP addresses, routing tables, etc

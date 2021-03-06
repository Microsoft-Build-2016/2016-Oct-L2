# Opening ports to a Linux VM in Azure
You open a port, or create an endpoint, to a virtual machine (VM) in Azure by creating a network filter on a subnet or VM network interface. You place these filters, which control both inbound and outbound traffic, on a Network Security Group attached to the resource that receives the traffic.

## Quick commands
To create a Network Security Group and rules you need the Azure CLI in Resource Manager mode:

```
azure config mode arm
```
You will need the name of the Resource Group you used when you created your CentOS VM. If you don't remember it, you can run the following command to list all the resource groups in your subscription:
```
azure group list
```

## 1. Create a new Network Security Group (NSG)
```
azure network nsg create --resource-group <your-resource-group> --location <location-of-your-VM> --name <name-for-the-new-NSG>
```

## 2. Get the Network Interface (NIC) of your VM
Assuming you have only 1 VM in the resource group, you can use the following command the get the name of the NIC:
```
azure network nic list --resource-group <your-resource-group> | awk 'NR==5{print $2}'
```

If you have more than 1 VM in the resource group you created, you can type the following command and find the respective NIC:
```
azure network nic list --resource-group <your-resource-group>
```

## 3. Associate the Network Security Group (NSG) with your Network Interface (NIC) of your VM
```
azure network nic set --resource-group <your-resource-group> --name <your-NIC-from-the-previous-step> --network-security-group-name <your-NSG-from-the-previous-step>
```

## 4. Opening port TCP 22
```
azure network nsg rule create --protocol tcp --priority 1060 --direction inbound --destination-port-range 22 --access allow --resource-group <your-resource-group> --nsg-name <name-for-the-new-NSG> --name TCP22
```

## 5. Opening port TCP 80
```
azure network nsg rule create --protocol tcp --priority 1010 --direction inbound --destination-port-range 80 --access allow --resource-group <your-resource-group> --nsg-name <name-for-the-new-NSG> --name TCP80
```

## 6. Opening port TCP 5000
```
azure network nsg rule create --protocol tcp --priority 1020 --direction inbound --destination-port-range 5000 --access allow --resource-group <your-resource-group> --nsg-name <name-for-the-new-NSG> --name TCP5000
```
## 7. Opening port TCP 5001
```
azure network nsg rule create --protocol tcp --priority 1030 --direction inbound --destination-port-range 5001 --access allow --resource-group <your-resource-group> --nsg-name <name-for-the-new-NSG> --name TCP5001
```



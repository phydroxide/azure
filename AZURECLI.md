
# Using Azure CLI

You'll need to first install the [Azure CLI](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli-linux?pivots=apt)

```
curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash
```

Or use this [Azure CLI](./tools/Dockerfile) docker container


## Configure Azure CLI 
Configure your default preferences such as timeout and display format.

``` az configure ```   

## Login

Login to the CLI. You'll be given a code to authorize in a browser. 

docker$ ``` az login ```   


## List Instance Sizes
List available vm sizes for the region.

docker$ ```az vm list-sizes westus3 ```

## Create / Delete Resource Groups

``` az group create --name PLW-AZURE --location westus3 ```
``` az group delete --resource-group PLW-AZURE ```

## Create VM Resources
Use the command line to create a VM resource

``` 
az vm  create --location westus3 \
                         --name plw-cli-vm \
                         --resource-group PLW-AZURE \
                         --image Debian11 --size Standard_DS1_v2 \
                         --ssh-key-values /root/id_rsa.pub 
```

`az vm create` will create network resources automatically. Or specify network resources with --vnet-name


We're going to install Kali and will need to disable secure boot for now so the image will boot. 

```
az vm  create --location westus3 \
                --name plw-cli-vm \
                --resource-group PLW-AZURE \
                --image Debian11 \
                --size Standard_DS1_v2 \
                --ssh-key-values /root/id_rsa.pub \
                --accelerated-networking false \
                --nic-delete-option Delete \
                --enable-hibernation false \
                --admin-username azureuser \
                --security-type TrustedLaunch \
                --enable-secure-boot false \
                --enable-vtpm true \
                --authentication-type ssh \
                --os-disk-delete-option Delete \
                --patch-mode ImageDefault \
                --vnet-name plw-vm-vnet \
                --subnet default \
                --public-ip-address-allocation static \
                --priority Spot \
                --max-price 5 \
                --eviction-policy Deallocate 

```
TODO - `-zone 1` PublicIP has an existing availability zone constraint NoZone and the request has availability zone constraint 1, which do not match

TODO - `-patch-mode AutomaticByPlatform ` The selected VM image is not supported for VM Guest patch operations.

## Manage VM Resources
[Azure VM CLI Docs](https://learn.microsoft.com/en-us/cli/azure/vm?view=azure-cli-latest)

Show VM details
```
az vm show --resource-group PLW-AZURE --name plw-vm --show-details
```

Stop VM. **WARNING:** This will kill running processes and shut down host! 
```
az vm stop --resource-group PLW-AZURE --name plw-vm
```

To avoid being billed unnecessarily make sure to deallocate the VM
```
az vm deallocate --resource-group PLW-AZURE --name plw-vm
```

Delete VM. **WARNING:** This will destroy the VM and associated disks. 
```
az vm delete --resource-group PLW-AZURE --name plw-vm
```

Use CLI to access VM

```
az ssh vm --resource-group PLW-AZURE --name plw-vm --local-user azureuser --private-key-file
```

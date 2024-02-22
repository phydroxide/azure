#Azure training 2024

You've lost your job! What are you going to do next!?

I am not going to Disneyland, unfortunately. The community support has been overwhelmingly positive. I have already had a number of colleagues reach out to me confident that my experience will land me a job fast. 

On the other hand, I also just applied to a remote position for which I'm super-qualified but I don't have Azure experience, or really any Microsoft Cloud experience beyond around 2012. 

So today I launched a free Azure account to kick the tires. This is my effort to pay it forward to the community or otherwise showcase my competency to a prospective employer.

## Create SSH Key
ssh-keygen -t rsa -b 4096 -C "Comment"

## Install CLI console
[Linux CLI](https://learn.microsoft.com/en-us/cli/azure/install-azure-cli-linux?pivots=apt)

```
curl -sL https://aka.ms/InstallAzureCLIDeb | sudo bash
```


```
az vm stop --resource-group PLW-AZURE --name plw-vm
```

```
az vm deallocate --resource-group PLW-AZURE --name plw-vm
```


```
az ssh vm --resource-group PLW-AZURE --name plw-vm
```

# Using Service Principal Identity to List AD Roles

**There are 2 objectives with this lab:**
* Log in to Azure using the Service Principal
* List the Role Definitions and Role Assignments



## Log in to Azure using the Service Principal

We first need to be able to log into Azure using the Linux jumpbox provided in the lab. We're going to use a Service Principal to view information in the portal from the jumpbox. So lets launch the Terminal first and then SSH into the Linux VM/jumpbox using
`ssh cloud_user@13.93.163.152` which is the IP address provided in the lab. And for those of you knew to remotely logging into machines, you have to specify `SSH` as the protocol, give a user (so in our case, `cloud_user`), and then use the @ symbol followed by 
the IP address of your machine. 

![Image](ServicePrincipal1.png)

Notice at the bottom of the image, we're now logged into the lab-VM as the cloud user. So we're officially in the VM. 

Once you're in the Linux VM, we're going to use the `Service Principal` to access resources in Azure. Ironically, we actually need to go to the portal to get the Tenant ID in order to use the Service Principal command. So I did the following:

Azure portal > Microsoft Entra ID > Properties > Tenant ID. I copied the ID because it's a long string of characters. We'll need this for the Linux VM portion. 

![Image](ServicePrincipal2.png)

Now, we'll use the `az login` command with the `--service-principal` argument. In this command, we have to specify a `user`, `password`, and `Tenant ID` (which we already copied from the portal). Put all of this information into the command. 

![Image](ServicePrincipal3.png)

There's only one resource group in this lab. Once that's listed, we're asked to apply the following tags to the entire resource group: `Environment=Production`, `Dept=IT`, and `CreatedBy=YourName` and in this case, I'm using my name, RockstarEV. I've never applied tags
to a resource in Azure, let alone have done it using PowerShell so I searched the Microsoft website ([link here](https://learn.microsoft.com/en-us/azure/virtual-machines/windows/quick-create-powershell)) and found the following commands:

![Image](Add_Remove_Update_Tags9.png)

The first command creates a variable named `$tags` that is then given the information for the tags. The second command creates a variable named `$resource` that gets the value of the resource and the resource group its in. The third command passes the tag information
by using the -ResourceId argument. I used the same tag and resource variables. 

![Image](Add_Remove_Update_Tags2.png)

I then printed the `$resource` variable just to make sure it was given the correct RG. Afterwards, I used the `New-AzTag` command listed on the website to give the resource group the required tags.

![Image](Add_Remove_Update_Tags3.png)

You can see from the above screenshot that the resource group now has the 3 tags required in the lab. 

## List the Role Definitions and Role Assignments

Next, we had to remove the tabs from a virtual machine. First, we had to list out the current tags on the VM.  

![Image](Add_Remove_Update_Tags4.png)

The only tag it had was the `defaultExperience=Yes` tag. We need to remove this tag. I used the same Microsoft webpage to find the remove tag commands. 

![Image](Add_Remove_Update_Tags10.png)

I created a new tag variable named `$removeTags` and it had the value of the original tag on the VM. Then, I passed the Update-AzTag and the Delete operation just as it was listed on the website. 

![Image](Add_Remove_Update_Tags5.png)

You can now see that the resource has no tags. Now, we need to add the `MarkForDeletion=Yes` tag to this VM. 

![Image](Add_Remove_Update_Tags6.png)

I used the same commands as the apply tag but I chose a new variable named `$addTag` just to keep things separate. With the `Update-AzTag` command, I used the Merge operation. Now, the VM has the new tag.



## Personal Notes

None for now. This was a pretty straightforward lab. It made me a little more familiar with tags which makes looking at all the information in the portal easier because I can separate different tabs more easily. 

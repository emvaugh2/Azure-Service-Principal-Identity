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

It should give you output that you're now logged into Azure. That completes the first objective of this lab! On to the next and final objective of this lab. 


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

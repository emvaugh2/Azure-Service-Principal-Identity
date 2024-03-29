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

So for the next task, we need to list the Azure role definitions and assignments using the Linux VM command line. Now I don't have much experience with Linux so I had to google a bunch of commands to see how to create a file, append a file, get a  JSON output, and etc.

I created a Notepad file with all the links I used but I'm sure I'll also post the links here as well. 

The first part of this tasks requires us to list the role definitions and output to a file named `roleinfo.json`. So we need to first find out the `az` command to list the role definitions so I found an article on the Microsoft Azure website ([link here](https://learn.microsoft.com/en-US/cli/azure/role/definition?view=azure-cli-latest#az-role-definition-list)). This is where I found the `az role definition list`. I received the following output. 

![Image](ServicePrincipal5.png)

If you use that same Microsoft website, you'll see there are additional commands on the left that relate to the `az role` list. So I found the command `az role assignment list --all` to get the role assignments. I wonder why this command required the `--all` argument
but that's fine. 

![Image](ServicePrincipal6.png)

We need to put all of this information into a JSON file named `roleinfo.json` so I needed to create that file first. I found an article ([link here](https://phoenixnap.com/kb/how-to-create-a-file-in-linux)) that showed an easy way to create files in the Linux terminal. So I used the command `touch` followed by `roleinfo.json` that would create the file. In order to see the file, you use the `ls` command which lists all the files or folders in a directory (s/o CompTIA A+). 

![Image](ServicePrincipal4.png)

Now that we have the command to list the roles and assignments and we now have the JSON file, lets actually get the output into the file. So I found another article ([link here]((https://askubuntu.com/questions/582536/how-can-i-input-to-a-file-directly-from-the-terminal))) that showed me the `echo` command and how to put that text from that command into a file. To overwrite a file, you use the `>` character. To APPEND a file (add onto the end of a file), you use two of those characters `>>`. I also noticed that you don't need the `echo` command since you're going to get an output from using the two role commands. 

So I used `az role defintition list --output json > roleinfo.json` to take the output from the role command put it into the roleinfo.json file. Then, I used `az role assignment list -all >> roleinfo.json` to append it onto the end of the file. 

![Image](ServicePrincipal7.png)

Afterwards, I had to see if the information was actually put into the file. I had to google which command to list the contents of the file ([link here](https://www.liquidweb.com/kb/how-to-display-contents-of-a-file-linux/#:~:text=The%20simplest%20way%20to%20view,the%20%2Fproc%2Fversion%20file.)) and I found the `cat` command. 

![Image](ServicePrincipal8.png)

I use `cat roleinfo.json` and received the proper output:

![Image](ServicePrincipal9.png)

So this concludes the lab but I found it hard to justify my output. From the above picture, you can see the one of the roles and one of the definitions in the output but you can search through the file. I then rediscovered the `vi` command from the follow article ([link here](https://www.geeksforgeeks.org/vi-editor-unix/)) where I was able to navigate the file much more freely. 

![Image](ServicePrincipal13.png)

Here is me having more concrete evidence of the role definitions and assignments in the `roleinfo.json` file:

![Image](ServicePrincipal10.png)
![Image](ServicePrincipal11.png)
![Image](ServicePrincipal12.png)

That should suffice. Lab completed!



## Personal Notes

So this lab was way more jarring than the previous very easy 3 labs. I'm not used to Linux, let alone the terminal so I needed to google what commands to use. Also, I didn't want to do a sloppy job so I was trying to find the cleanest way to get the job done. This is where I'm very appreciative of my Cisco CLI experience because command lines can be scary but I know you'll get used to it very quickly the more you use it. It's actually sometimes the easiest way to get jobs done depending on the scale. 

With that being said, I'm so glad I completed this lab without any help because it made me feel more confident in being able to figure out Linux when I have to take that next step. I do have some basic Linux experience from handling the time clock interface to make sure those devices were able to get on our network. I'm familiar with `vi editor`. I'm familiar with `ls` but that's about it right now. 

This was fun. Definitely going to add it to the mental cookie jar as to why I deserve to get a cloud role. 

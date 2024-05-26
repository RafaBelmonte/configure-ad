<p align="center">
<img src="https://i.imgur.com/pU5A58S.png" alt="Microsoft Active Directory Logo"/>
</p>

<h1>On-premises Active Directory Deployed in the Cloud (Azure)</h1>
This tutorial outlines the implementation of on-premises Active Directory within Azure Virtual Machines.<br />


<h2>Environments and Technologies Used</h2>

- Microsoft Azure (Virtual Machines/Compute)
- Remote Desktop
- Active Directory Domain Services
- PowerShell

<h2>Operating Systems Used </h2>

- Windows Server 2022
- Windows 10 (21H2)

<h2>High-Level Deployment and Configuration Steps</h2>

- Setup Resources in Azure (DC-1 [Windows Server] - Client-1 [Windows 10])
- Ensure Connectivity between the client and Domain Controller
- Create an Admin and Normal User Account in AD
- Join Client-1 to your domain (mydomain.com)
- Setup Remote Desktop for non-administrative users on Client-1
- Create additional users and attempt to log into client-1 with one of the users


<h2>Deployment and Configuration Steps</h2>


DC-1 Deployment

![image](https://github.com/RafaBelmonte/configure-ad/assets/170759303/5aec6a5a-9fed-478e-9e9a-e4d7b36215a3)

Client-1 Deployment

![image](https://github.com/RafaBelmonte/configure-ad/assets/170759303/a67cdd9c-d12a-45d3-a9f5-81bb9cf131f7)

Client-1 Networking config

![image](https://github.com/RafaBelmonte/configure-ad/assets/170759303/7c99e070-d522-4315-b2c5-06afe55f1abf)




Create the Domain Controller VM (Windows Server 2022) named “DC-1”.
Take note of the Resource Group and Virtual Network (Vnet) that get created at this time.
Set Domain Controller’s NIC Private IP address to be static.
Create the Client VM (Windows 10) named “Client-1”. Use the same Resource Group and Vnet that was created in Step 1.
Ensure that both VMs are in the same Vnet (you can check the topology with Network Watcher.

PS: Let's go through the red arrows as some reminders
- Use the Region that is closest to your location.
- On Azure Free Subscription you only have access to certain size machines in certain zones. Make sure you pick the correct one for your task and also ensure our second VM is in the same availability zone.
- Set your first VM as a Windows Server.
- Make sure to select the appropriate size of your machine - 2vcpus, 16GiB would work just fine.
- ! Since this is just a lab for practice TAKE NOTE of your username and password. They will be necessary in the following steps.
- In the Client-1 configuration, make sure they are in the same Resource Group.
- Move to Networking config and make sure they are in the same Virtual network. (DC-1 Vnet should be automatically created. If you cannot find this option, wait 1 minute, go back to the Azure portal and try re-creating your Client 1 machine, it should appear in the Virtual network section.)
- Create VM.

</p>
<br />

Setting Domain Controller's NIC IP address to Static

![image](https://github.com/RafaBelmonte/configure-ad/assets/170759303/00426a52-78f4-42d2-9a61-3107a2526f6b)

![image](https://github.com/RafaBelmonte/configure-ad/assets/170759303/c69de52e-9c35-42b2-ab62-682bc0754d47)

![image](https://github.com/RafaBelmonte/configure-ad/assets/170759303/3bd30742-d2f1-4e89-9279-b9dc0ca068e0)



Go back to your DC-1 VM - Networking - Network Settings - IP Configurations - click on ipconfig1 and change the Allocation to STATIC.

Make sure you save it.

</p>
<br />

Ensuring Connectivity between Client and Domain Controller

![image](https://github.com/RafaBelmonte/configure-ad/assets/170759303/b997602b-c389-492c-b479-3181fd9891b7)

![image](https://github.com/RafaBelmonte/configure-ad/assets/170759303/7aa9bbb3-d15c-4880-bbc5-0d6c4f7db876)

![image](https://github.com/RafaBelmonte/configure-ad/assets/170759303/53bddc12-8a78-4fd1-a46e-ce761b600770)

![image](https://github.com/RafaBelmonte/configure-ad/assets/170759303/9d124755-f1f1-4987-bb72-1ff0704c7e5b)

![image](https://github.com/RafaBelmonte/configure-ad/assets/170759303/90261158-8d75-4acf-9169-a06bb51f7e11)

![image](https://github.com/RafaBelmonte/configure-ad/assets/170759303/3f7bf20c-9b4e-41b5-8cd6-c3f5f74d0a40)

![image](https://github.com/RafaBelmonte/configure-ad/assets/170759303/da025469-1d8e-4e89-b0a9-c7ee12757f29)

![image](https://github.com/RafaBelmonte/configure-ad/assets/170759303/429b569d-fea0-4edd-9e3d-789ec6f0da37)

![image](https://github.com/RafaBelmonte/configure-ad/assets/170759303/b505ea48-47e1-4178-95d1-f1a23e4eee20)







- Remote Connect to Client-1 using your username and password.
- Go to your DC-1 VM and retrieve its Private IP address indicated by the poorly drawn red arrow.
- Open the Command Promp on your now running Client-1 VM and run a ping -t command on your DC-1 IP for a perpetual ping. (It will time out because of DC-1s firewall settings)
- Log into DC-1 with Remote Access so we can make the proper changes.
- Using your search bar on DC-1, search for firewall and click on the WD Firewall app. Go to inboud rules, organize it through PROTOCOL and find ICMPv4.
- Click and enable rule on both of these Info Rules.
- Go back to Client-1 and ping your DC-1 again. Notice that you will now get Replies from DC-1 whcih means the connection has been established.

</p>
<br />



Installing Active Directory

![image](https://github.com/RafaBelmonte/configure-ad/assets/170759303/581d2792-a86b-495b-966a-e5f50a502458)

![image](https://github.com/RafaBelmonte/configure-ad/assets/170759303/e1bbdf7a-e940-424a-8b01-9bdd07d10175)

![image](https://github.com/RafaBelmonte/configure-ad/assets/170759303/8d30926c-3661-454f-8433-b2d0b9b37d8d)

![image](https://github.com/RafaBelmonte/configure-ad/assets/170759303/dec882bd-e53a-4a14-b6f0-baa4c631a35c)

![image](https://github.com/RafaBelmonte/configure-ad/assets/170759303/3fc22f08-7f33-44fd-9b61-07a7d968ff31)

![image](https://github.com/RafaBelmonte/configure-ad/assets/170759303/84311ab0-bc5c-4f03-97b9-6d8b84cc2594)

![image](https://github.com/RafaBelmonte/configure-ad/assets/170759303/dbe4f725-934c-4b9d-87af-4e8a7463f751)

![image](https://github.com/RafaBelmonte/configure-ad/assets/170759303/a30199f0-36e8-43f9-9856-ac1fbaeec1c9)

![image](https://github.com/RafaBelmonte/configure-ad/assets/170759303/7833445c-db5a-4dbb-a678-85bc245d5acb)





- Click Start and open your Server Manager if it is not already running.
- Add Roles and features and click next until you reach the Server Roles where you wull enable Active Directory Domain Services. (This will install AD)
- Click next until confirmation and click install.
- On your Server Manager, a yellow exclamation point should appear on the top right corner. Click on it and "Promote this server to a domain controller.
- Add a new forest - since its private, we do not need to worry about the domain name, just pick one you can remember in the form of *.com
- Pick a random password and take note of it.
- Click next, wait for areas to be populated until you reach the Prerequisites Check and click Install.
- Once it is finished installing restart your DC-1 VM if it does not force you to restart automatically.
- Refresh your DC-1 in the Azure DC-1 Overview page and reconnect to your DC-1 machine (make sure to change the username to your previous username - it will have changed).
</p>
<br />

Create an Admin and Normal User Account in AD

![image](https://github.com/RafaBelmonte/configure-ad/assets/170759303/4bb95292-fb88-4b8d-a63d-5ed590d65ea0)

![image](https://github.com/RafaBelmonte/configure-ad/assets/170759303/9cd9ab8f-158a-4a1e-8cde-5c7861d324f6)

![image](https://github.com/RafaBelmonte/configure-ad/assets/170759303/4609c415-f9eb-4f4a-96f5-62d8e5381803)

![image](https://github.com/RafaBelmonte/configure-ad/assets/170759303/d5c209f1-7ae9-4c7f-8825-065ffb8d3b0c)

![image](https://github.com/RafaBelmonte/configure-ad/assets/170759303/8811bf56-0828-4ee8-bdc1-e8ed3a43f50a)

![image](https://github.com/RafaBelmonte/configure-ad/assets/170759303/8bb7d8c8-0dcb-4856-8664-66926d37dc47)

![image](https://github.com/RafaBelmonte/configure-ad/assets/170759303/b16605a7-de80-43f2-aa1f-91e50cd971d9)

![image](https://github.com/RafaBelmonte/configure-ad/assets/170759303/75c293bd-169a-4c31-af66-6aabf801b943)

![image](https://github.com/RafaBelmonte/configure-ad/assets/170759303/6b513440-48e3-4c2e-b369-9bc5f3ceecc1)

![image](https://github.com/RafaBelmonte/configure-ad/assets/170759303/80262b7f-bb2b-4a1a-a13a-a5aef19bf423)


- Go to Tools in your Server Manager and pick the Active Directory Users and Computers option.
- We will be creating 2 Organizational Units, try to use caps or a symbol to make it easily identifiable that those are the units YOU created.
- In this case we created _EMPLOYEES and _ADMINS. (We are using these names just as an example)
- Now lets create our own Admin account. Go to _ADMINS that we created in the previous step, right click, New User, fill it out anyway you want but make sure to take note of all your information. Make sure to uncheck the "must change password" box and check the "password never expires" box just for practical purposes for this exercise.
- After John Doe was created, although he is in the _ADMINS folder, he is not yet an Admin, so to do that, we right click on John, go to Properties, Member of tab, Add..., type Domains in the box and click check names. It will open different groups from which you will pick Domain Admins for John (or whatever name you picked). Ok everything and make sure you APPLY the changes before closing "John's" tab.
- We will log out of our DC-1 VM and from now on we will be using our created "mydomain.com\john_admin". (remember to change to whatever name you used to create it)
</p>
<br />

Join Client-1 to the Domain

![image](https://github.com/RafaBelmonte/configure-ad/assets/170759303/fe819de6-78b1-4140-977a-c78c805b2747)

![image](https://github.com/RafaBelmonte/configure-ad/assets/170759303/b1e39231-2e42-4f6a-976e-f1676e4d3ad9)

![image](https://github.com/RafaBelmonte/configure-ad/assets/170759303/d404f075-6b98-48ff-be91-e322759bc73a)

![image](https://github.com/RafaBelmonte/configure-ad/assets/170759303/d2e69c0c-f294-494b-ae3a-09624272fbbb)

![image](https://github.com/RafaBelmonte/configure-ad/assets/170759303/097d3787-50ca-4d0d-9418-089238164b59)

![image](https://github.com/RafaBelmonte/configure-ad/assets/170759303/a16b3f4a-d8e1-41ab-af27-d919f8b978d5)

![image](https://github.com/RafaBelmonte/configure-ad/assets/170759303/73dc8bce-96a1-4d5f-b566-0e1c65bc2f7c)

![image](https://github.com/RafaBelmonte/configure-ad/assets/170759303/fc9a9b4a-c8c3-49fd-a33a-8279222a38ae)

![image](https://github.com/RafaBelmonte/configure-ad/assets/170759303/9bc26a02-a162-4580-89fd-e8a1251dc9a7)

![image](https://github.com/RafaBelmonte/configure-ad/assets/170759303/56d9e72e-e2d1-49fd-adc7-654bfb73b481)

![image](https://github.com/RafaBelmonte/configure-ad/assets/170759303/afe7e457-2f5f-4d8d-bf6f-939aa8396c29)

![image](https://github.com/RafaBelmonte/configure-ad/assets/170759303/205055a4-8e77-4f61-bf84-c8a33cb0c88f)

![image](https://github.com/RafaBelmonte/configure-ad/assets/170759303/12872a32-979d-4386-af29-fd64df34e111)




- Before we can proceed with this step we must go back to our Azure Client-1 settings page and set Client-1's DNS settings to the DC Private IP address.
- Go to our Azure DC-1 page, Networking settings and retrieve DC-1s Private IP address
- Proceed to our Azure Cliente 1 page, Networking, DNS servers and change it to Custom so we can add DC-1 private IP address. Save it, wait for it to update, and restart Client-1's VM directly from the portal as indicated on image6 this will flush its DNS cache.
- After Client-1 has finished restarting we will log back in and try to add it to the domain.
- Go back to your Client-1 VM, Right click start and go to System and Rename this PC, Change.., check the domain box and add "mydomain.com" again, could be whatever domain name YOU chose if you didnt pick the same as mine.
- User your account and password that we created before as shown on image10
- We must restart our Client-1 to apply the changes made.
- Now, when we restart our VM, we will be able to log in with our admin account because Client-1 is a member of the Domain.
- Re-log into Client-1 with your "mydomain" username and password.
<br />


Setup Remote Desktop for non-administrative users on Client-1.

![image](https://github.com/RafaBelmonte/configure-ad/assets/170759303/f06ce63f-b69c-41be-a13f-74a53f1c520b)

![image](https://github.com/RafaBelmonte/configure-ad/assets/170759303/f721da9a-ebf5-4efe-b919-9e7b0d4bd235)

![image](https://github.com/RafaBelmonte/configure-ad/assets/170759303/a4b11cea-0392-4fab-9d20-8b7cbf85f9d8)

![image](https://github.com/RafaBelmonte/configure-ad/assets/170759303/a9b16dd9-d9ac-4022-8dc4-986585f1c7f8)

![image](https://github.com/RafaBelmonte/configure-ad/assets/170759303/86c758b5-36c8-4640-802b-8e4595f8cf4d)


- Once you signed in to Client-1 again, right click start -> System -> Remote Desktop -> Select users... -> Add...
- Instead of adding an individual user, we can use a special group called Domain Users -> Check Names... -> OK
- Now all Domain Users are allowed to log into this VM.
- Back to DC-1, use your search bar to get to Active Directory Users and Computers -> Users Folder -> Find and double click Domain Users -> Members. You can see the current members of the Domain Groups and they all have access to log in to this machine, including non-admins.
</p>
<br />

Creating additional users and attempting to log into client-1 with one of the users.

![image](https://github.com/RafaBelmonte/configure-ad/assets/170759303/7f4c24e2-b203-46d6-8245-5179b0d7c6a9)

![image](https://github.com/RafaBelmonte/configure-ad/assets/170759303/36064ba0-d963-4f88-a79b-edf187369415)

![image](https://github.com/RafaBelmonte/configure-ad/assets/170759303/52af9a4b-bbf0-4dd1-8e06-86ffb9fe67c7)

![image](https://github.com/RafaBelmonte/configure-ad/assets/170759303/851d10cc-b07b-4d81-b2c5-2783af01b27d)

![image](https://github.com/RafaBelmonte/configure-ad/assets/170759303/75e56610-1519-41da-9b32-76ab3492d1d7)

![image](https://github.com/RafaBelmonte/configure-ad/assets/170759303/370bba88-3963-4a37-b9d5-6cd74a045b93)

![image](https://github.com/RafaBelmonte/configure-ad/assets/170759303/0a574478-3a3a-4f57-939a-4f0e71293a52)

![image](https://github.com/RafaBelmonte/configure-ad/assets/170759303/beceaab3-ae40-471e-b7ac-d4d020f85318)

![image](https://github.com/RafaBelmonte/configure-ad/assets/170759303/5cd5df15-91a3-4ef9-85a1-cc5e75194ee4)

![image](https://github.com/RafaBelmonte/configure-ad/assets/170759303/bc8930f3-e78c-48ac-9976-048e2649c977)

![image](https://github.com/RafaBelmonte/configure-ad/assets/170759303/c86f4c40-5060-4c8a-b0a0-47f56241b695)



- In this step, we will start by opening Powershell ISE (make sure you open it as an administrator) in your DC-1.
- We will be using a pre-made script that you can get at (https://github.com/joshmadakor1/AD_PS/blob/master/Generate-Names-Create-Users.ps1)
- Copy raw code as indicated by the poorly drawn red arrow.
- Create a new file on Powershell ISE and paste the script to this new file (I would reduce the number of accounts from 10K to 1K to make this lab a bit quicker).
- Run the script by pressing F5 and observe the accounts being created.
- If you go back t o your Active Directory Users and Computers and check your _EMPLOYEES folder you can see it has been populated with 1000 new random users.
- Pick a random user from your list -> right click -> Properties -> Account tab -> you can now see "bax.sonat's" logon name.
- Copy the name of the user you picked and note it down so you dont forget it.
- Move to Client-1 and sign out from your admin account.
- Relog to Client-1 with Remote Desktop using your newly created user's name and the password utilized by the script (Password1)
- Just as an example, lets say you (or an actual user) tries to log in too many times with an incorrect password. You can go back to your DC-1 VM, find the user -> Properties -> Account tab - and if the user has locked him or herself out of their account, you can Unlock it as an admin.
- By right clicking a user's name you can reset the user's account password or disable an account.
- With this last setp we finished our tutorial on Active Directory! Hope everything went according to plan, and if it did, GOOD JOB!

- PS: Make sure you delete your Resource Groups and VMs once done with the lab so it doesnt drain your Azure Subscription resources!!!!
</p>
<br />


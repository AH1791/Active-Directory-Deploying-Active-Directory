# Active-Directory-Deploying-Active-Directory
<p align="center">
<img src="https://i.imgur.com/aMMGyHQ.jpeg" height="80%" width="80%" alt="Setting Up in Azure"/>
<br />

<h1>Deploying Active Directory in Azure</h1>



<h2>Description</h2>
For this portion of the Active Directory project, I install Active Directory on the domain controller, create a domain admin, and join the client VM to the domain.  <br/>
<br />

This project is a continuation of [Active Directory: Preparing Infrastructure in Azure](https://github.com/AH1791/Preparing-Active-Directory-Infrastructure-in-Azure), so this project picks up where I left off. <br />
<br />

<h2>Environments and Utilities Used</h2>

- <b>Microsoft Azure</b>
- <b>Virtual Machines</b>
- <b>Remote Desktop Connection</b>
- <b>Active Directory</b>

<h2>Operating Systems Used </h2>

- <b>Windows Server </b>
- <b>Windows 10</b>

<h2>Project Walk-through:</h2>

<p align="center">
To start, in the DC (Domain Controller), bring up the Server Manager dashboard, if it's not up already: <br/>

 ![image](https://github.com/user-attachments/assets/47f407ed-56ac-47db-b031-797f4c54b0fa)

<br />
<br />
Here, click on "Add roles and features" > Next > Next > For server selection there should only be one to select: <br/>

![image](https://github.com/user-attachments/assets/063d9c4d-1db8-4201-bca3-a5852b69173a)

<br />
Now, check "Active Directory Domain Services" > Click "Add Features":  <br/>

![image](https://github.com/user-attachments/assets/6981a9da-ae22-43ac-a773-52e455e7de9b)

<br />
<br />
Select "Active Directory Domain Services" > Next > Next > Next > Now select "Restart the destination server automatically if required" and click "Install": <br/>

![image](https://github.com/user-attachments/assets/f46526bb-dcb8-4bd2-b8fa-acbabbf5f4d6)

<br />
<br />
Next, I have to promote this machine as a DC by configuring it as a new forest as "mydomain.com" (this can be anything, mydomain.com is just easy to remember) So, to do this, I will click on the flag icon with the caution symbol, near the top-right of the Server Manager Dashboard and select "Promote this server to a domain controller":  <br/>

![image](https://github.com/user-attachments/assets/387eeaa5-a9d7-40b9-b9e0-44c3f49dbaa3)

<br />
<br />
Then select "Add new forest" and in "Root domain name" I'll type "mydomain.com". Then "Next":  <br/>

![image](https://github.com/user-attachments/assets/cd2483ab-f31c-4648-829e-8f1517288a2d)

<br />
<br />
Type in a password of your choosing on the next screen like so then hit "Next":  <br/>

![image](https://github.com/user-attachments/assets/65f8de1c-71bc-4c06-96ac-a7d9db5e1c1f)

<br />
<br />
Leave "Create DNS delegation" unchecked and click "Next". Hit "Next" until you reach this screen. On this screen, click "Install". The machine will restart after the installation:  <br/>

![image](https://github.com/user-attachments/assets/16f1446c-1ab8-4b83-90b1-421aa9b1cfee)

<br />
<br />
It may take some time to restart, but once it does, sign back in using "mydomain.com\labuser" (if you're using the same names as me) as the username. Use the password you set for "labuser". Here we have to sign in as "mydomain.com\labuser" instead of just "labuser", because now we've promoted this machine to a DC and it needs to know the context of who we're siging in as (someone in the domain, or a local user). I want to sign in as a user on the domain, so I specify that with the leading "mydomain.com\" part at sign in:  <br/>

![image](https://github.com/user-attachments/assets/fd41bbdf-f6f6-4c91-992e-1bda88e1ea74)

<br />
<br />
Now, we'll create an admin user. To do this, in the start search bar, search for "Active Directory Users and Computers":  <br/>

![image](https://github.com/user-attachments/assets/f7c772d8-e16c-4141-8a56-6ea197ed0d30)

<br />
<br />
Right click on "mydomain.com" and select "New" > "Organizational Unit" and name it EXACTLY "_EMPLOYEES" (In a future project we will run a script that uses this name to work). Do the same steps here to create an OU (Organizational Unit) named "_ADMINS":  <br/>

![image](https://github.com/user-attachments/assets/dd204c75-0c15-4704-b6d1-b2728f9aebe8)

<br />
<br />
Next, create a new user in "_ADMINS" by right-clicking on "_ADMINS" > New > User and fill it out like so:  <br/>

![image](https://github.com/user-attachments/assets/279ac997-1c16-4271-be5e-625d629d5258)

<br />
<br />
Then, create a password and uncheck "User must change password at next logon" and check "Password never expires" (You probably wouldn't do this in real life, but we'll do it for this lab where nothing's really at stake):  <br/>

![image](https://github.com/user-attachments/assets/fb29674b-4a54-42cd-8525-609eb57f3f33)

<br />
<br />
Jane Doe is now apart of the "_ADMINS" OU, but isn't actually an admin. To make her an admin right-click on her name > Properties > Member Of > Add... > Enter "domain admins" > Check Names > OK > Apply > OK:  <br/>

![image](https://github.com/user-attachments/assets/5975425d-9a1b-4d86-b6a0-85a222c196c4)

<br />
<br />
Now, we can log out of DC and log back in using Jane's credentials:  <br/>

![image](https://github.com/user-attachments/assets/d3a6f37e-9b51-4acf-81fd-f3fc36b0523c)

<br />
<br />
Once logged in, log into client-1 VM, if not already. Here I'll join this client to the domain by right-clicking the start menu > Systems > Rename this PC (advanced) > Change > Select "Domain" and enter "mydomain.com" and hit "OK":  <br/>

 ![image](https://github.com/user-attachments/assets/8bbe75d1-a545-4980-a61f-a48dd1ced48c)

<br />
<br />
It'll ask for an account with permission to join the domain. We can use our admin's credentials for Jane here. Then a pop up saying welcome to the domain will appear and the machine will try and restart:  <br/>

![image](https://github.com/user-attachments/assets/73d4e306-ec9b-4204-a132-d306ecfeeeb8)

<br />
<br />
After the restart, client-1 is now a member of the domain. To check, in the DC machine start seach bar, search for "Active Directory Users and Computers" > mydomain.com > Computers > client-1 should be listed:  <br/>
<img 
<br />
<br />
Now we can create another OU as we did before and name it "_CLIENTS". Then drag and drop client-1 from "computers" to "_CLIENTS":  <br/>
<img 
<br />
<br />

<h2>Active Directory is Deployed and Ready for Use! </h2>

<b>We've successfully installed AD on the domain controller, created a domain admin, and joined the client VM to the domain. Next, we'll create users in Powershell by running a script, and then manage the accounts and group policy!  </b>
<br />
<br />
</p>

<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>

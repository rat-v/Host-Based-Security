## Objective
In this lab, I am working with Active Directory to create units, users,
and set policies on different levels. This will be my first ever
exposure to Active Directory, but I've heard people talk about it a lot
and know it's importance so I am excited.

This lab seems like a good introduction to Active Directory, but I want
to do more so I continue staying in the lab environment and mess around
with settings and features to learn more.

## Skills Learned

## Creating an Organizational Unit and Users

Seems pretty trivial to create an OU and user, I can create an OU called
"Minecraft" and a user called "creeper" just by using simple right click
options. I can do the same to create more users named "zombie" and
"steve".

![image5](https://github.com/user-attachments/assets/eb2e0e81-99b0-4cb6-8140-30626418827c)
![image16](https://github.com/user-attachments/assets/245a91a2-66bd-4103-a750-6f0658595a2c)



## Setting a Domain-wide password policy

In Group Management Policy, I can right click on "Default Domain Policy"
and click Edit. There, I can head to Policies -\> Windows Settings -\>
Security Settings -\> Account Policies -\> Password Policy

I can change the minimum requirements for a valid password. For example,
setting the minimum character count to 10. Now, this password policy is
set to everything inside the campus.edu domain that I am working in.

![image3](https://github.com/user-attachments/assets/41dbc6b8-2ac8-4017-96c1-b6dcae262e40)


I can update the policy by typing "gpupdate /force" in command prompt.

Now, I cannot create a new user without a proper password that meets the
requirements.

![image9](https://github.com/user-attachments/assets/9b4786d1-142e-4caf-b45b-1cdbf5bb27a8)



## Setting an Organizational-wide policy

Now, I can edit the policies for the "Minecraft" Organizational Unit by
right clicking "Minecraft" in Group Policy Management, and clicking on
"Create a GPO in this domain, and link it here".

![image1](https://github.com/user-attachments/assets/fd4106ae-49e0-4f55-9adb-b370442f8c14)


Now editing Minecraft's policies, I can prohibit their access to opening
the Control Panel by navigating to User Configuration -\> Administrative
Templates -\> {Control Panel}
![image8](https://github.com/user-attachments/assets/968703ff-97ec-4062-899b-71d4fc34f022)

The lab ends here


## Logon/Logoff Scripts

I can setup Windows scripts that will run when a user logs in or out of
the computer. I restarted the lab environment and created a new OU and
users to manage. Then, I created a GPO for that OU and navigated to User
Config -\> Policies -\> Windows Settings -\> Scripts (Logon/Logoff)

![image12](https://github.com/user-attachments/assets/dc9e4c9c-6da8-48eb-8954-1e5dc109ae1e)


Right clicking and clicking Properties on the Logon script allows me to
add a script file which would then run for a user in the OU when they
logon the system. I believe the script would just be something like a
batch file, running the same commands you would enter in something like
command prompt.

## Assigning a home folder per user

By highlighting all the users in an OU, I can right click -\> Properties
-\> Profile and set a home folder for them by creating a folder on the
server, and then also adding %username% at the end of the path to create
a new folder for each user that will be titled as their username.

![image14](https://github.com/user-attachments/assets/2cfa9975-e28c-4859-870f-b782c31bb177)


![image13](https://github.com/user-attachments/assets/ed3515b3-b84b-459b-85fa-ca5b598dec7d)


Now if I add a new user to the OU and repeat the steps as before, a new
folder will be created for the new user, and no new folder will be
created for the existing users because it already exists for them

![image15](https://github.com/user-attachments/assets/1c1d6f7c-2956-4c61-9708-1d02953d44d8)


Now, logging into User1, I can access this folder

![image2](https://github.com/user-attachments/assets/21262d0c-9ff7-48aa-a40f-a93d6b6cd937)


As User1, I created a text file in the folder called "Secrit Dokumints"

![image7](https://github.com/user-attachments/assets/06058b27-1bf8-45c7-8959-10490837d82a)

Now, logging into AFTER1, I can see this unique home folder

![image11](https://github.com/user-attachments/assets/81cdd639-23f9-42c2-aef5-4ab35113c0c4)


### **Making each user's home folder secure to each other**

However, I noticed that I could just put in "\\\\SERVER\\" in the path
and I would be able to access all the folders, being able to see the
share folder and the TestA folder within. I was also able to enter
User1's folder and open "Secrit Dokumints". This is definitely a case
where restrictions on folder access need to be placed, but I don't have
the fundamental knowledge for that yet. I can still attempt it, however.

Back on the administrator's system, I create a new group in the OU
called "RESTRICTED_ACCESSS" and add the users in that group

![image6](https://github.com/user-attachments/assets/746ee357-7644-48e8-b2c4-aad5f2aff757)


I created a new GPO and tried to look through the folders there, but I
couldn't find anything. My next guess was to just edit the folder
directly by right clicking -\> Properties -\> Security. I thought I
could deny full control of AFTER1's folder to the entire group, and
specifically allow full control for AFTER1, but this did not work as
this restricted access to EVERYONE in the group, even the user himself.
I could always just manually set each user's permissions, but this
defeats the purpose of working with groups.

Also, I'm starting to understand I should use better names for example
folders and users so I can more easily follow what's happening.

After \~30 minutes of trial and error, I believe I found the intended
way to accomplish this. I had to uncheck an option that would make all
the user's home folders inherit permissions from their parent (TestA)
folder. Then, I cleared all the permissions within each user's home
folders, so that the only permissions were that Administrators and the
user had full access to the folder. However, I did have to do the same
steps for every folder, so perhaps there is a faster way to just do this
all at once. I hadn't used the new RESTRICTED_ACCESSS group I created
earlier, so perhaps it has something to do with that but I couldn't
figure it out on my own. Or, maybe there is a way to set new folders to
always NOT inherit permissions from the parent folder, so that the
system admin doesn't have to keep unchecking that box every time.

Now, logging back onto AFTER1, I can no longer access User1's folder,
but I can access my own (AFTER1's).

![image10](https://github.com/user-attachments/assets/415ddbd2-7b10-44ea-ae68-71c61edf5e3e)


And as User1, I can access my own User1 folder and the "Secrit
Dokumints" text file inside, but I cannot access AFTER1's folder

![image4](https://github.com/user-attachments/assets/a30a7500-cc4a-4b74-a593-10a2dadefb72)


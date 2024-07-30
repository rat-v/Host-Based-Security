In this lab, I am working with Active Directory to create units, users,
and set policies on different levels. This will be my first ever
exposure to Active Directory, but I've heard people talk about it a lot
and know it's importance so I am excited.

This lab seems like a good introduction to Active Directory, but I want
to do more so I continue staying in the lab environment and mess around
with settings and features to learn more.

### Creating an Organizational Unit and Users

Seems pretty trivial to create an OU and user, I can create an OU called
"Minecraft" and a user called "creeper" just by using simple right click
options. I can do the same to create more users named "zombie" and
"steve".

![](output\c. Active Directory/media/image5.png){width="5.359375546806649in"
height="3.6546041119860018in"}![](output\c. Active Directory/media/image16.png){width="4.902043963254593in"
height="2.9412270341207347in"}

### Setting a Domain-wide password policy

In Group Management Policy, I can right click on "Default Domain Policy"
and click Edit. There, I can head to Policies -\> Windows Settings -\>
Security Settings -\> Account Policies -\> Password Policy

I can change the minimum requirements for a valid password. For example,
setting the minimum character count to 10. Now, this password policy is
set to everything inside the campus.edu domain that I am working in.

![](output\c. Active Directory/media/image3.png){width="4.901042213473316in"
height="4.9876323272090985in"}

I can update the policy by typing "gpupdate /force" in command prompt.

Now, I cannot create a new user without a proper password that meets the
requirements.

![](output\c. Active Directory/media/image9.png){width="10.0in"
height="1.2361111111111112in"}

### 

###  

### Setting an Organizational-wide policy

Now, I can edit the policies for the "Minecraft" Organizational Unit by
right clicking "Minecraft" in Group Policy Management, and clicking on
"Create a GPO in this domain, and link it here".

![](output\c. Active Directory/media/image1.png){width="4.677083333333333in"
height="3.2916666666666665in"}

Now editing Minecraft's policies, I can prohibit their access to opening
the Control Panel by navigating to User Configuration -\> Administrative
Templates -\> {Control Panel}

![](output\c. Active Directory/media/image8.png){width="8.78125in"
height="7.270833333333333in"}

The lab ends here

###  

### Logon/Logoff Scripts

I can setup Windows scripts that will run when a user logs in or out of
the computer. I restarted the lab environment and created a new OU and
users to manage. Then, I created a GPO for that OU and navigated to User
Config -\> Policies -\> Windows Settings -\> Scripts (Logon/Logoff)

![](output\c. Active Directory/media/image12.png){width="10.0in"
height="6.097222222222222in"}

Right clicking and clicking Properties on the Logon script allows me to
add a script file which would then run for a user in the OU when they
logon the system. I believe the script would just be something like a
batch file, running the same commands you would enter in something like
command prompt.

### Assigning a home folder per user

By highlighting all the users in an OU, I can right click -\> Properties
-\> Profile and set a home folder for them by creating a folder on the
server, and then also adding %username% at the end of the path to create
a new folder for each user that will be titled as their username.

![](output\c. Active Directory/media/image14.png){width="7.544642388451444in"
height="6.130021872265967in"}

![](output\c. Active Directory/media/image13.png){width="8.45312554680665in"
height="3.7802230971128608in"}

Now if I add a new user to the OU and repeat the steps as before, a new
folder will be created for the new user, and no new folder will be
created for the existing users because it already exists for them

![](output\c. Active Directory/media/image15.png){width="8.223958880139982in"
height="4.1376793525809274in"}

Now, logging into User1, I can access this folder

![](output\c. Active Directory/media/image2.png){width="8.07812554680665in"
height="3.8034503499562553in"}

As User1, I created a text file in the folder called "Secrit Dokumints"

![](output\c. Active Directory/media/image7.png){width="7.78125in"
height="2.5625in"}

Now, logging into AFTER1, I can see this unique home folder

![](output\c. Active Directory/media/image11.png){width="10.0in"
height="3.611111111111111in"}

#### **Making each user's home folder secure to each other**

However, I noticed that I could just put in "\\\\SERVER\\" in the path
and I would be able to access all the folders, being able to see the
share folder and the TestA folder within. I was also able to enter
User1's folder and open "Secrit Dokumints". This is definitely a case
where restrictions on folder access need to be placed, but I don't have
the fundamental knowledge for that yet. I can still attempt it, however.

Back on the administrator's system, I create a new group in the OU
called "RESTRICTED_ACCESSS" and add the users in that group

![](output\c. Active Directory/media/image6.png){width="10.0in"
height="4.986111111111111in"}

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

![](output\c. Active Directory/media/image10.png){width="9.088542213473316in"
height="4.843386920384952in"}

And as User1, I can access my own User1 folder and the "Secrit
Dokumints" text file inside, but I cannot access AFTER1's folder

![](output\c. Active Directory/media/image4.png){width="9.088542213473316in"
height="5.348985126859143in"}

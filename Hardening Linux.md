## Skills Learned
Using Nmap to find vulnerable services<br>
Using FTP to access and browse files, including Linux's "passwd" and "shadow" files<br>
Using John the Ripper on the "shadow" file to pull usernames and passwords<br>
Accessing SSH and Telnet with the now known root credentials and creating new accounts<br>
Changing weak passwords of accounts<br>
Closing vulnerable ports and uninstalling unnecessary services<br>
Blocking anonymous access to SMB shares and restricting access to valid users<br>


## Exploiting an Ubuntu Machine

I am operating a Kali Linux machine and there is another Linux based
machine in the network that I can scan using Nmap. A port scan on the
device shows vulnerable services running such as ftp, telnet, and http.

![image19](https://github.com/user-attachments/assets/9ad1d584-eaad-4459-8af0-af6e6df726ad)

![image16](https://github.com/user-attachments/assets/faf8ad7d-ce16-47dc-b065-2562450f2370)


### **FTP**

I can use ftp anonymously by just putting the user as "ftp" and entering
any password in the prompt

![image27](https://github.com/user-attachments/assets/dd4ee53f-d48a-459e-aa5a-cf192adf32cb)


Now I can browse files in the system by navigating through directories
with 'ls' and 'cd', then grab files using 'get'

![image21](https://github.com/user-attachments/assets/c64313ae-77f6-4b28-8b0e-73754c6ffcb2)


![image11](https://github.com/user-attachments/assets/845a3ac7-5641-4218-8f75-867d8e0beab3)


The "passwd" file gives us a list of users and their UID. Any user with
a UID of 0 has root privileges. We can see that both root, but also toor
has a UID of 0

![image20](https://github.com/user-attachments/assets/c12a185e-da11-43a4-ad44-1a688c4d72ed)


![image3](https://github.com/user-attachments/assets/8d552ed4-4431-476a-8086-64a32290b586)


Next, the "shadow" file gives us a list of usernames and their password
hashes, this can be potentially be cracked using the John the Ripper
tool

![image1](https://github.com/user-attachments/assets/ee3e7a21-ac9e-44c0-ba57-2996d3ae4c2d)


![image2](https://github.com/user-attachments/assets/6d957add-d9d4-4abd-bafb-51025b174419)


Looks like toor and ubuntu have their password the same as their
username. student and root both have their password as just password.
These are extremely weak passwords which will be changed when hardening
the system.

### **SSH**

In the initial Nmap scan, it showed that ssh was also a service that was
running. Normally it's ok to run, but because we have the username and
password of some root-level users, we can do whatever we want. Username:
root, Password: password

![image28](https://github.com/user-attachments/assets/6c7ac6aa-070a-4c51-a6b8-484ce2aa8129)


Just for fun, I decided to add a new account with my new permissions

![image8](https://github.com/user-attachments/assets/60b3c3d3-6bbf-4d85-b99e-da4e5b589517)


![image17](https://github.com/user-attachments/assets/67eb6021-0532-49b8-bf7c-83049bd06f49)

### **Telnet**

I can also use the telnet service and login using the credentials that
were ripped

![image9](https://github.com/user-attachments/assets/9b129aa0-42bc-4db8-90b9-229d1a9f87cc)


### **HTTP**

The Nmap scan also showed HTTP as a service that was running. Putting in
the device's IP address in the web browser just gives a default page

![image15](https://github.com/user-attachments/assets/f24ae39c-4a4d-439d-bd9c-963123e9458f)


### **SMB**

Even though this is a Linux machine, it can still be a file server for
Windows machines. However, a critical flaw for this service is that it
allows anonymous access. I can connect to the system and not put
anything in the password and still get access to the shares. I can even
mount those shares on my own directory I created and write files to that
share without any authentication.

![image23](https://github.com/user-attachments/assets/2b378500-0e7d-4b6e-b6cd-9009286d56c6)


'mkdir /badShare' to create the new share

'mount -t cifs //192.168.1.25/MyShare /badShare' to attach the share to
my new directory

![image4](https://github.com/user-attachments/assets/8d73e645-a1a7-460b-bc75-63f9113f7580)


Here I create an example file in my new directory, but it could be
anything

![image14](https://github.com/user-attachments/assets/1dbab3b6-6814-4c03-b767-d5e7b9db5e18)


## Hardening the Ubuntu Machine

### **Managing Accounts and Passwords**

Logging into the Ubuntu Machine, I can see the accounts I added. I also
notice a Guest Account is active at the bottom which, I remember, can
lead to vulnerabilities involving not needing proper credentials to
access files

![image18](https://github.com/user-attachments/assets/21037dda-42c5-419f-b47d-5292bb82666c)

First, I'll edit the passwd file so that the root account is disabled by
pasting /usr/sbin/nologin

![image22](https://github.com/user-attachments/assets/6500a15e-3dec-4778-85f4-8cd23086ded5)


Then, I can try and find other users with a UID of 0 by using grep to
pull the info off passwd and highlight it for me

![image10](https://github.com/user-attachments/assets/98144804-8050-42d1-bf51-56eee992b7e8)


This is strange that there are two root accounts, so I will delete toor
using userdel --force, then verifying that the account is gone

![image26](https://github.com/user-attachments/assets/ee14d7b9-c08c-4d7e-9a3f-82f9429e2d1f)


Next, I will change the very weak passwords of the other two accounts,
student and ubuntu

![image12](https://github.com/user-attachments/assets/f22568a7-7f0f-46db-94c4-437a5e2059ee)


### **Closing Vulnerable Ports**

I can use the 127.0.0.1 loopback IP address and perform an Nmap scan on
myself, the Ubuntu device.

![image6](https://github.com/user-attachments/assets/9b07d05f-3007-47e4-9b39-42849e976e7f)


I can see that ftp is running on vsftpd, telnet is running on telnetd,
and http is running on Apache2, so I will use sudo apt-get remove to
uninstall those unnecessary and vulnerable services.

Another Nmap scan will show that these services are now remove

![image13](https://github.com/user-attachments/assets/f5ecf174-6eac-44df-9539-751a8a331a1b)

### Blocking Anonymous Access

Here, we can verify that the Kali Linux file of gg.txt in the new
directory did transfer to the Ubuntu's MyShare folder

![image5](https://github.com/user-attachments/assets/936a1abb-d87c-48eb-b185-a7afdbab0d7c)


Any file can be uploaded in this manner which is a serious risk as tons
of damage can be caused through uploading files such as spam and
malware.

To harden this, anonymous access and unauthorized share enumeration will
be disabled. In the smb.conf file, I set valid users to only "student",
and remove the semi-colon in the beginning of the line, then restart the
service using 'sudo service samba restart'

![image24](https://github.com/user-attachments/assets/01b61b10-d4a6-41e1-a8fa-a9c9ea84c3f5)


Now, on the Kali Linux machine, when I try to access the shares again, I
get hit with "ACCESS DENIED" because I don't have the correct password.

![image7](https://github.com/user-attachments/assets/5b519802-b173-45c6-a55a-c7b426f5dc64)


Now, no unauthenticated users can list the shares present and get
anonymous access to the files, and I cannot write any files to the share
on my mounted directory

###  

### Updates

Finally, keeping everything up-to-date using 'sudo apt-get update' and
'sudo apt-get upgrade'

![image25](https://github.com/user-attachments/assets/07ab2e2a-24ab-488d-bb63-6bde68e2179f)


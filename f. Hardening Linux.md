### Exploiting an Ubuntu Machine

I am operating a Kali Linux machine and there is another Linux based
machine in the network that I can scan using Nmap. A port scan on the
device shows vulnerable services running such as ftp, telnet, and http.

![](output\f. Hardening Linux/media/image19.png){width="5.494792213473316in"
height="2.697720909886264in"}

![](output\f. Hardening Linux/media/image16.png){width="5.307292213473316in"
height="4.813418635170604in"}

#### **FTP**

I can use ftp anonymously by just putting the user as "ftp" and entering
any password in the prompt

![](output\f. Hardening Linux/media/image27.png){width="5.697916666666667in"
height="2.9375in"}

Now I can browse files in the system by navigating through directories
with 'ls' and 'cd', then grab files using 'get'

![](output\f. Hardening Linux/media/image21.png){width="6.734375546806649in"
height="2.980916447944007in"}

![](output\f. Hardening Linux/media/image11.png){width="6.567708880139983in"
height="3.8081671041119862in"}

The "passwd" file gives us a list of users and their UID. Any user with
a UID of 0 has root privileges. We can see that both root, but also toor
has a UID of 0

![](output\f. Hardening Linux/media/image20.png){width="5.03125in"
height="1.0208333333333333in"}

![](output\f. Hardening Linux/media/image3.png){width="3.53125in"
height="0.6145833333333334in"}

Next, the "shadow" file gives us a list of usernames and their password
hashes, this can be potentially be cracked using the John the Ripper
tool

![](output\f. Hardening Linux/media/image1.png){width="5.635416666666667in"
height="3.6458333333333335in"}

![](output\f. Hardening Linux/media/image2.png){width="3.8020833333333335in"
height="0.6041666666666666in"}

Looks like toor and ubuntu have their password the same as their
username. student and root both have their password as just password.
These are extremely weak passwords which will be changed when hardening
the system.

#### **SSH**

In the initial Nmap scan, it showed that ssh was also a service that was
running. Normally it's ok to run, but because we have the username and
password of some root-level users, we can do whatever we want. Username:
root, Password: password

![](output\f. Hardening Linux/media/image28.png){width="5.588542213473316in"
height="3.2134109798775152in"}

Just for fun, I decided to add a new account with my new permissions

![](output\f. Hardening Linux/media/image8.png){width="4.703125546806649in"
height="1.1757808398950131in"}

![](output\f. Hardening Linux/media/image17.png){width="10.0in"
height="3.986111111111111in"}

#### **Telnet**

I can also use the telnet service and login using the credentials that
were ripped

![](output\f. Hardening Linux/media/image9.png){width="9.28125in"
height="4.510416666666667in"}

#### **HTTP**

The Nmap scan also showed HTTP as a service that was running. Putting in
the device's IP address in the web browser just gives a default page

![](output\f. Hardening Linux/media/image15.png){width="10.0in"
height="3.875in"}

#### **SMB**

Even though this is a Linux machine, it can still be a file server for
Windows machines. However, a critical flaw for this service is that it
allows anonymous access. I can connect to the system and not put
anything in the password and still get access to the shares. I can even
mount those shares on my own directory I created and write files to that
share without any authentication.

![](output\f. Hardening Linux/media/image23.png){width="10.0in"
height="2.5555555555555554in"}

'mkdir /badShare' to create the new share

'mount -t cifs //192.168.1.25/MyShare /badShare' to attach the share to
my new directory

![](output\f. Hardening Linux/media/image4.png){width="9.052083333333334in"
height="0.9791666666666666in"}

Here I create an example file in my new directory, but it could be
anything

![](output\f. Hardening Linux/media/image14.png){width="10.0in"
height="3.236111111111111in"}

### Hardening the Ubuntu Machine

#### **Managing Accounts and Passwords**

Logging into the Ubuntu Machine, I can see the accounts I added. I also
notice a Guest Account is active at the bottom which, I remember, can
lead to vulnerabilities involving not needing proper credentials to
access files

![](output\f. Hardening Linux/media/image18.png){width="7.223958880139983in"
height="5.086871172353455in"}

First, I'll edit the passwd file so that the root account is disabled by
pasting /usr/sbin/nologin

![](output\f. Hardening Linux/media/image22.png){width="6.354166666666667in"
height="2.375in"}

Then, I can try and find other users with a UID of 0 by using grep to
pull the info off passwd and highlight it for me

![](output\f. Hardening Linux/media/image10.png){width="7.114583333333333in"
height="0.75in"}

This is strange that there are two root accounts, so I will delete toor
using userdel --force, then verifying that the account is gone

![](output\f. Hardening Linux/media/image26.png){width="4.1875in"
height="0.84375in"}

Next, I will change the very weak passwords of the other two accounts,
student and ubuntu

![](output\f. Hardening Linux/media/image12.png){width="5.635416666666667in"
height="2.34375in"}

#### **Closing Vulnerable Ports**

I can use the 127.0.0.1 loopback IP address and perform an Nmap scan on
myself, the Ubuntu device.

![](output\f. Hardening Linux/media/image6.png){width="7.640625546806649in"
height="2.6503423009623797in"}

I can see that ftp is running on vsftpd, telnet is running on telnetd,
and http is running on Apache2, so I will use sudo apt-get remove to
uninstall those unnecessary and vulnerable services.

Another Nmap scan will show that these services are now remove

![](output\f. Hardening Linux/media/image13.png){width="9.706704943132108in"
height="2.415628827646544in"}

#### Blocking Anonymous Access

Here, we can verify that the Kali Linux file of gg.txt in the new
directory did transfer to the Ubuntu's MyShare folder

![](output\f. Hardening Linux/media/image5.png){width="5.1875in"
height="5.729166666666667in"}

Any file can be uploaded in this manner which is a serious risk as tons
of damage can be caused through uploading files such as spam and
malware.

To harden this, anonymous access and unauthorized share enumeration will
be disabled. In the smb.conf file, I set valid users to only "student",
and remove the semi-colon in the beginning of the line, then restart the
service using 'sudo service samba restart'

![](output\f. Hardening Linux/media/image24.png){width="5.619792213473316in"
height="3.966911636045494in"}

Now, on the Kali Linux machine, when I try to access the shares again, I
get hit with "ACCESS DENIED" because I don't have the correct password.

![](output\f. Hardening Linux/media/image7.png){width="6.625in"
height="1.21875in"}

Now, no unauthenticated users can list the shares present and get
anonymous access to the files, and I cannot write any files to the share
on my mounted directory

####  

#### Updates

Finally, keeping everything up-to-date using 'sudo apt-get update' and
'sudo apt-get upgrade'

![](output\f. Hardening Linux/media/image25.png){width="10.0in"
height="3.0416666666666665in"}

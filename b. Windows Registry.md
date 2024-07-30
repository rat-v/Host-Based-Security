In this lab, I am exposing myself to the Windows Registry Editor,
looking around the hives for useful information. I will then use FTK
imager to take a snapshot of the current Registry settings and analyze
some of the system files using Windows Registry Recovery. I can find
different information depending on the type of file I analyze.

#### **Checking Locale**

HKEY Current User \> Control Panel \> {International}

#### **Checking for Startup Apps**

HKEY Current User \> Software \> Microsoft \> Windows \> CurrentVersion
\> {Run}

#### **Checking DHCP Information**

HKEY Local Machine \> System \> CurrentControlSet \> Services \> Tcpip
\> Parameters \> Interface \> {'One of the folders will give info'}

### FTK Imager - Obtaining a Live Windows Registry

This tool takes a snapshot of the current Registry settings which is
useful to analyze them without worrying about changes

I can save the files to a created folder named RegsitryFiles by clicking
on Obtain System Files

![](output\b. Windows Registry/media/image1.png){width="8.28125in"
height="5.541666666666667in"}

This results in files being saved to that folder, but I don't yet
understand what they mean.

![](output\b. Windows Registry/media/image4.png){width="10.0in"
height="4.347222222222222in"}

###  

### **Windows Registry Recovery**

Using the Windows Registry Recovery application, I can view these files
in the folder. It's much safer to view registry files like this, rather
than viewing it life in the Registry Editor as accidentally changing
values can critically corrupt the machine.

I can find user information in the SAM file

![](output\b. Windows Registry/media/image5.png){width="7.458333333333333in"
height="3.7238801399825023in"}

I can find network information in the 'system' file, moving to the
TCP/IP tab

![](output\b. Windows Registry/media/image3.png){width="7.59375in"
height="5.2599923447069115in"}

I can also find information about running services in the same 'system'
file

![](output\b. Windows Registry/media/image2.png){width="8.072916666666666in"
height="4.660688976377953in"}

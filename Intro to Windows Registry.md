## Objective
In this lab, I am exposing myself to the Windows Registry Editor,
looking around the hives for useful information. I will then use FTK
imager to take a snapshot of the current Registry settings and analyze
some of the system files using Windows Registry Recovery. I can find
different information depending on the type of file I analyze.

## Skills Learned
Navigating the Windows Registry Editor<br>
Using FTK Imager to take Registry snapshots<br>
Analyzing system files with Windows Registry Recovery<br>
Locating Locale settings, IDing Startup Apps, and Looking through DHCP Settings in the Registry<br>
Saving and organizing Registry snapshot files<br>
Viewing and analyzing Registry files safely with Windows Registry Recovery<br>
Finding network information in the 'system' file<br>

### **Checking Locale**

HKEY Current User \> Control Panel \> {International}

### **Checking for Startup Apps**

HKEY Current User \> Software \> Microsoft \> Windows \> CurrentVersion
\> {Run}

### **Checking DHCP Information**

HKEY Local Machine \> System \> CurrentControlSet \> Services \> Tcpip
\> Parameters \> Interface \> {'One of the folders will give info'}

## FTK Imager - Obtaining a Live Windows Registry

This tool takes a snapshot of the current Registry settings which is
useful to analyze them without worrying about changes

I can save the files to a created folder named RegsitryFiles by clicking
on Obtain System Files

![image1](https://github.com/user-attachments/assets/7beba5fd-3978-46b1-b55a-2167101548ba)


This results in files being saved to that folder, but I don't yet
understand what they mean.

![image4](https://github.com/user-attachments/assets/5beac684-1475-4304-a9a8-4c335014830b)


## **Windows Registry Recovery**

Using the Windows Registry Recovery application, I can view these files
in the folder. It's much safer to view registry files like this, rather
than viewing it life in the Registry Editor as accidentally changing
values can critically corrupt the machine.

I can find user information in the SAM file

![image5](https://github.com/user-attachments/assets/9d3d6563-61b3-4c9e-b1c3-0718c1e99c31)


I can find network information in the 'system' file, moving to the
TCP/IP tab

![image3](https://github.com/user-attachments/assets/998f7839-c3e4-45c1-b49b-c789f30ec2a1)


I can also find information about running services in the same 'system'
file

![image2](https://github.com/user-attachments/assets/18759477-34aa-4b68-bc58-1195cfb4eb25)


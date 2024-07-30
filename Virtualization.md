## Objective
In this mini-project, I am enabling Virtual PC in Windows 7 and Hyper-V in Windows 10

## Virtual PC on Windows 7

Opening Windows Virtual PC -\> Create Virtual Machine

![image4](https://github.com/user-attachments/assets/244e27f0-9f08-4d19-b440-db76b22faeab)


I don't have an ISO file in this lab, so I skip the next step of
inserting the boot media to set up the operating system for that
machine. However, I could always download one online from Microsoft's
website for Windows.

![image9](https://github.com/user-attachments/assets/950a6c60-cf88-4525-bb23-3d13740697df)


### Windows XP Mode

By installing Virtual PC on this system, I can create virtual machines
as well as also enable "Windows XP" mode to run Windows XP directly on a
Windows 7 system. The only benefit I could ever think as to why someone
would use this is if some niche application ONLY works on Windows XP and
does not work for Windows 7.

After just going through the installation wizard, I can run a Windows XP
virtual machine on the system.

![image2](https://github.com/user-attachments/assets/035a8ce4-a2ee-4dbb-b416-86d4ed44b041)


## Hyper-V on Windows 10

First, I need to enable the feature, then restart the PC
![image1](https://github.com/user-attachments/assets/b369123a-4ff8-4772-b078-7faf248cc8bd)
![image6](https://github.com/user-attachments/assets/2082a4f3-698f-43bb-84b3-61123c1e6588)


Next, I can open the Hyper-V Manager and create a virtual switch that
the virtual machines can use. **External** allows VMs to communicate
with the external/outside network. **Internal** allows VMs to
communicate with each other and the host. **Private** allows VMs to
communicate with each other privately.

Now, I can create a new virtual machine and go through the wizard. I
will click next for custom settings, rather than to finish with default
settings. This is so I can set the switch as the virtual switch I just
created.

![image8](https://github.com/user-attachments/assets/28b5cf79-61a5-47b3-8e6c-8c87c2a02213)


**In this installation, I do have an iso file that I can use to install
the operating system**

![image3](https://github.com/user-attachments/assets/60815fee-d11a-4232-86f8-74035782c383)


![image7](https://github.com/user-attachments/assets/7b5c05ec-21d6-4c1e-889f-bd505056003d)


From here, I can then continue to install windows which I will not do as
I don't have the correct drivers to install.

![image5](https://github.com/user-attachments/assets/c50b3849-fa10-4a54-99b4-f242796e02dd)


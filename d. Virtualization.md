In this mini-project, I am enabling Virtual PC in Windows 7 and Hyper-V
in Windows 10

### Virtual PC on Windows 7

Opening Windows Virtual PC -\> Create Virtual Machine

![](output\d. Virtualization/media/image4.png){width="7.630208880139983in"
height="5.722656386701662in"}

I don't have an ISO file in this lab, so I skip the next step of
inserting the boot media to set up the operating system for that
machine. However, I could always download one online from Microsoft's
website for Windows.

![](output\d. Virtualization/media/image9.png){width="6.223958880139983in"
height="4.3645505249343834in"}

#### Windows XP Mode

By installing Virtual PC on this system, I can create virtual machines
as well as also enable "Windows XP" mode to run Windows XP directly on a
Windows 7 system. The only benefit I could ever think as to why someone
would use this is if some niche application ONLY works on Windows XP and
does not work for Windows 7.

After just going through the installation wizard, I can run a Windows XP
virtual machine on the system.

![](output\d. Virtualization/media/image2.png){width="10.0in"
height="7.541666666666667in"}

### Hyper-V on Windows 10

First, I need to enable the feature, then restart the PC

![](output\d. Virtualization/media/image1.png){width="5.145833333333333in"
height="4.65625in"}![](output\d. Virtualization/media/image6.png){width="4.5in"
height="3.4791666666666665in"}

Next, I can open the Hyper-V Manager and create a virtual switch that
the virtual machines can use. **External** allows VMs to communicate
with the external/outside network. **Internal** allows VMs to
communicate with each other and the host. **Private** allows VMs to
communicate with each other privately.

Now, I can create a new virtual machine and go through the wizard. I
will click next for custom settings, rather than to finish with default
settings. This is so I can set the switch as the virtual switch I just
created.

![](output\d. Virtualization/media/image8.png){width="10.0in"
height="6.847222222222222in"}

**In this installation, I do have an iso file that I can use to install
the operating system**

![](output\d. Virtualization/media/image3.png){width="10.0in"
height="6.597222222222222in"}

![](output\d. Virtualization/media/image7.png){width="10.0in"
height="6.847222222222222in"}

From here, I can then continue to install windows which I will not do as
I don't have the correct drivers to install.

![](output\d. Virtualization/media/image5.png){width="7.747201443569554in"
height="6.488280839895013in"}

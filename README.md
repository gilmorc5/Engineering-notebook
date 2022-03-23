# Engineering-notebook
CS 490 Engineering Notebook

Captstone Project - Boeing
**Aircraft Network Simulation **

Assigned: CUPS Printer
As: Network Engineer 

Research on CUPS Printer

**What is CUPS: **

Apart from something to drink off CUPS in Linux is: Common Unix Printing System

CUPS is responsible in general for making you connect a printer to Ubuntu and not needing to install any drivers. You can say that CUPS is the one responsible for making almost all printers into a Out of the Box experience. No need for additional drivers, Printer CDs/DVDs or in most cases having to compile and do a lot of work to have it working instantly.

The other acronym that stands out as CUPS is HPLIP which is HP Linux Imaging and Printing. This one gives some enhancements over HP printers and all-in-one. If you are the 1% that has problems with detecting your HP printer with CUPS then try HPLIP. It covers that extra 1%. And it shows stuff like ink level and some other specific HP variables.

**How to connect to a Network Printer**

Assuming you are using the default Ubuntu which comes with CUPS go to the Cog symbol in the top right part of the screen (Looks like a Gear). Click on it and select System Settings. Then click on Printers. Now click on the Add button to add the printer.

Note that since the printer is on the network (Not connected to you directly) the option for Network Printer will appear empty for a couple of seconds. The time depends on the network, how the router handles the queue and any other devices connected on the network. After a while the printer should appear under the Network Printer section and you will be able to select it. CUPS will take care of either installing the drivers if available or downloading one if not found on the system (Eg: Epson L555). In some cases for me it took 45 seconds, in others 5 seconds.

In this case, the computer that is sharing the printer should make sure that the printer configuration is correctly configured as mentioned above. If the Printer has wireless access, then make sure the printer is correctly connected to the router and that your PC is also correctly connected to the same wireless router. In the case you are connected via wired cable and the printer via wireless to the same router or same network, you need to also configure the router to make sure all wireless devices see wired ones.

How to configure the Printer section in Samba
Verify you have the printer connected and samba/cups installed (The samba and cups packages should be installed, if not simply install them). This is because CUPS handles the printer while Samba handles the sharing but just in case you don't have them simply:

sudo apt-get install cups samba
After that we start with the configuration of them. If you are on either the desktop or server version of Ubuntu you can quickly configure it with this terminal way (The GUI way is explained above):

Edit your samba main configuration which is located in /etc/samba/smb.conf. Type:

sudo nano /etc/samba/smb.conf
We want to change the following variables and remove the comment (# or ;) symbols in front of them if they have it. So for example:

Change the WORKGROUP to the one the network is using, so other devices on the same workgroup will be able to see the printer:

workgroup = WORKGROUP_NAME
If you wish to allow guest to print then look for the ;GUEST = YES and remove the ; in front of it:

guest = yes
Now look for the security option and change it to share, this will save you several issues later on:

security = share
Now we go directly to ther samba printer section which would look something like this:

 [printers]
    comment = All Printers
    browseable = no
    path = /var/spool/samba
    printable = yes
;   guest ok = no
;   read only = yes
    create mask = 0700
what we want to do is change browseable to yes and guest ok to yes

Should look like this after editing:

 [printers]
    comment = All Printers
    browseable = yes
    path = /var/spool/samba
    printable = yes
    guest ok = yes
;   read only = yes
    create mask = 0700
This is all. Now restart the samba service. you can do it in several ways:

**sudo service smbd restart**

Now go to your windows system or Linux system and look for the printer in the network or the server IP. It might take some time because of how the network is configured and devices connected to it, but this should be the basic to configure any printer on the network.

############################################################################################################################################
Second Semester

## Physical vs. Cloud
Physical:
-Expensive
-Lead time for equipment
-Configurations for remote access require acces through school netowrk
-Easy to configure when all hardware is present
-Better to pass off when done
-"Hands on" experiences

Cloud
-Much more cost effective 
-Available immediately
-Remote access very simple to set up and can access anywhere
-Difficult to set up due to the nature of being virtual
-Valueable experience in the cloud enviroment

Cloud is the best fit for our project and we can use it to emulate all of our hardware and configure it any way we want.

How?

EVE-NG is a type 1 Hypervisor that will run on the "bare metal" server to emulate our hardware. 

Where?

We will need a rented server with dedicated CPU and high memory. Linode offers both of these for a fraction of the cost of AWS or Azure. We will rent a Linode server with 2 dedicated CPU's and 48 Gb of RAM. This can be done for a base of $120/month. The closest server database is located in Atlanta, GA, we have a very low ping in relation to this database and this would be idea for our needs. 

How can we emulate on a remote server?

We will send Linode a custom ISO image that they will load onto the server and this ISO image will be the EVE-NG hypervisor. EVE-NG will allow us to emulate a network enviroment that is identical to any physical network. EVE-NG pulls in ISO images of actual real world hardware. 

How can we connect?

When Linode rents us a server, we will get either a public IP or a FQDN that will allow us to connect to this server's GUI through a web interface. Setting this up will not be difficult but time consuming. 

**NETWORK TOPOLOGY**
![BoeingProjectTopo](https://user-images.githubusercontent.com/79158319/159765697-fcebaa27-3322-4997-9e91-58af0ec403f6.png)
Above is the intented network topology as of 3/23/2022. When the funding comes through to purchase the Linode Virtual enviroment, we will load the server with the EVE-NG ISO image and begin the build out the network for the Capture the flag event at the end of April.


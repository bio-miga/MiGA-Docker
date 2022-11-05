## Create a MiGA Instance

The following steps are for creating a new MiGA instance on AWS. The procedures are slightly different if you are relaunching a stopped instance (see the section on **Using MiGA AWS**).

1. Log into your AWS account.  
Open your browser and log into your AWS account.  
1. Select the region.  
From near the right-hand end of the menu bar at the top of the screen, select the region "US East (Ohio) us-east-2." The MiGA AMI is available in this region.  

1. Select the EC2 service.  
Click on "Service" in the upper left-hand corner of your screen and under the heading Compute choose EC2.  

1. Create a security group.  
From the menu along the left-hand side of the screen, under Network & Security, choose Security Groups. On the page that opens, enter a name and description in the form fields. These should be something you can easily associate with Miga, like "my_miga_security_group" and "allows SSH and Web access to MiGA."  
In the Inbound rules section, click on Add rule three times.  
For the first rule, enter SSH for the type, leave the Port range at 22 and change the source to Anywhere IPv4.  
For the second rule, set the type to HTTP, the Port range to 80 and the Source to Anywhere IPv4.  
For the third rule, leave the Type as Custom TCP, enter 8080 for the Port range and change the Source to Anywhere IPv4. 
Scroll down and click on the Orange button Create Security group. This will save the information.

1. Launch your EC2 instance.  
From near the left-hand end of the menu bar at the top of the screen, select "Services" and then "EC2" located under the "Compute" category. On the page that opens, click "Launch instance" and select "Launch instance" from the drop-down menu that opens.   

1. Choose the MiGA AMI.  
On the page that opens, look for "Application and OS Images (Amazon Machine Image)". Enter MiGA in the search bar and press Enter or Return. Choose the "Community AMIs" tab and select the latest version of MiGA. 

1. Choose the instance type.  
In the next section of the page, choose an instance type. For running the tutorials, we suggest choosing an instance with at least 8 vCPUs and 32 Gb RAMM, *e.g.* t3.2xlarge for $0.3328 per hour. For "real work" with perhaps hundreds of genomes, you will need to choose an instance with more vCPUs, memory, and preferably higher network performance. vCPUs should be two times the number of MiGA jobs you wish to run at a time. We recommend 4 Gb of memory per vCPU.   
If you know the instance type you want to use, based on prior experience or someone's recommendation, you may find it more easily by clicking on the "All instance families" button and then choosing an instance family. For example, choosing the m5n family you can more easily find m5n.16xlarge with 64 vCPUs, 256 GB of RAMM and 75 Gigabit Network Performance.  
You may compare costs at [https://aws.amazon.com/ec2/pricing/on-demand/](https://aws.amazon.com/ec2/pricing/on-demand/), but keep in mind that instances with greater resources will finish in less time.  
After clicking on the instance type you want to use, click on "Next: Configure Instance Details" at the bottom of the screen.  

1. Select a key.   
Under the heading "Key pair (login)" enter the name of your key pair or select it from the pull-down menu.  

1. Configure the security group.  
Choose the MiGA security group you created above from the pull-down menu.  

1. Add EBS storage.   
Click on "Next: Add storage" at the bottom of the page. On the page that opens, change the size of the root volume to 50 Gb. (This may seem unnecessarily large for the root volume, but is the minimum size required to perform installation of the TypeMat\_Lite database.) Then click on "Add New Volume." The volume type should be EBS. Under the heading "Size (GiB)" enter the size of the volume you want to add, but we suggest at least 100 Gb. If you add the Phyla\_Lite and TypeMat\_Lite databases, they will take up at least 63 Gb on the storage disk, and you will need more room for your projects. We suggest that you leave the box "Delete on Termination" unchecked so that you may save the storage volume separately from the instance. That way the storage volume will not be automatically deleted if you terminate the instance. When you are sure you are finished with a storage volume, you can manually delete it.   

1. Launch the instance.  
Click on the orange "Launch instance" button near the bottom right of the screen.    

1. Get the IP address.    
Click on the name of the instance near the top of the page that opens. You will be taken to the Instances dashboard. Enter a meaningful name In the "Name" field so that you can recognize the instance later. Click on the instance and information about it will appear. Write down the public IPv4 address. You may also copy the address to your clipboard by clicking on the copy icon next to the address. You will need the address to log into the instance.  
Wait until "running" appears under the heading "Instance State" and "2/2 checks" appears under the heading "Status checks."  

1. Connect to the instance via a terminal.  
Open a terminal on your computer (the terminal app for Mac OS users, git bash or PowerShell for Windows users) and log into the instance with the following command, replacing "\<key pair.pem>" with the name of your key pair file in directory ~/.ssh and "\<ip address>" with the public IP address you just wrote down or copied to your clipboard.

```
ssh -i .ssh/<key pair> ubuntu@<ip address>
```
Answer the prompt with "yes" to connect.

12. Get the name of the storage volume.  
Enter the following into the session terminal. The lines beginning with # need not be entered. They are comments explaining the purpose of the following line.   

```
# Log in as user ubuntu.
sudo su - ubuntu

# Check that you are in the ubuntu directory.
pwd

# List the block devices.
lsblk
```
This will display something like:
```
ubuntu@ip-172-31-2-190:~$ lsblk
NAME        MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
loop0         7:0    0   25M  1 loop /snap/amazon-ssm-agent/4046
loop1         7:1    0 55.5M  1 loop /snap/core18/2246
loop2         7:2    0 55.5M  1 loop /snap/core18/2253
loop3         7:3    0 61.9M  1 loop /snap/core20/1242
loop4         7:4    0 67.3M  1 loop /snap/lxd/21545
loop5         7:5    0 67.2M  1 loop /snap/lxd/21835
loop6         7:6    0 61.9M  1 loop /snap/core20/1270
loop7         7:7    0 43.3M  1 loop /snap/snapd/14295
loop8         7:8    0 32.5M  1 loop /snap/snapd/13640
nvme0n1     259:0    0   50G  0 disk
└─nvme0n1p1 259:1    0   50G  0 part /
nvme1n1     259:2    0  100G  0 disk
ubuntu@ip-172-31-2-190:~$
```
You should be able to distinguish the root and storage disks based on their sizes (assuming you did give them different sizes!). Also, there will be a branch under the root disk. Note the entries under the column "NAME." In the example above, `nvme0n1` is the root volume and `nvme1n1` is the storage volume.  

13. Format, mount, and setup the storage volume.  
The code below formats and mounts the storage volume, and then copies some database files to it. If the name of your storage device is `nvme1n1` you may copy the code and paste it into your terminal all at once. Otherwise, substitute `nvme1n1` with the actual name as necessary.      
```
# Format the storage device.
sudo mkfs -t xfs /dev/nvme1n1

# Mount miga-data on the /dev/nvme1n1 (storage) device.
sudo mount /dev/nvme1n1 miga-data

# Check that miga-data is available.
lsblk

# Make miga-data writable (change permissions).
sudo chmod 777 miga-data

# Make a database directory in miga-data.  
mkdir miga-data/db

# Copy the database files from miga-web/db.
cp -r miga-web/db/* miga-data/db/

# Move to the miga-data directory.
cd miga-data/

# Bind the two database directories.
sudo mount --bind db ../miga-web/db 
```
14. Configure MiGA  
To begin configuring MiGA, enter the following into the terminal:  

```
cd
miga init
```
Accept all of the default values by pressing Enter or Return **++except++** for the following questions:

- Should I include MyTaxa modues?  
Answer no. MyTaxa modules are not included in the MiGA AMI.  
- A template daemon already exits. Do you want to preserve it?  
Answer no.  
- How many jobs can I launch at once?  
Answer one-half of the number of vCPUs for your instance.  

14. Start the server.  
You are less likely to have problems with the web server stopping if you start it from a tmux session. To do this, enter:  

```
tmux new -s web-server
```

A green band will appear across the bottom of the session terminal window indicating that you are in a tmux session named web-server. Then enter the code below; you may copy and paste it into the terminal all at once.  

```
# Move to the miga-web directory.
cd
cd miga-web

# Start the server.
export SECRET_KEY_BASE='bundle exec rake secret'
export RAILS_SERVE_STATIC_FILES=true
bundle exec rails server -e production -b 0.0.0.0 -p 8080 -u Puma
```
Wait a few seconds until messages stop scrolling down the screen. Then exit the tmux session by entering ```Ctrl-B d``` by pressing the Ctrl key, then the b key, releasing both, and pressing the d key. The green band will disappear from the bottom of the screen indicating that you are no longer in the tmux session, but the web server will continue to run in the background.  

If you wish, you may now close the session terminal and log out of your AWS account (but make sure you have written down the public IP address first!).  

15. Log into MiGA-Web  
Open your browser and enter the following into the address bar where \<ip address\> is the public IP address for the instance.  

```
http://<ip adddress>:8080
```
The MiGA-Web Welcome page will open. Click on the "Create user" button and create your account by entering a user name, email address, and password. Log into your account by clicking on the "Log in" button and entering your email address and password.  

16. Install the reference databases.  
For full functionality, the reference databases need to be installed using the MiGA-Web interface. (You will not be able to do this if you are using the t2.micro instance and/or chose volume sizes smaller than 50 Gb.)   
Go to the Admin console by clicking on the down arrow next to the green globe in the upper right corner and choosing "Admin console." Scroll down the page that opens and click on "Download databases."  Download Phyla\_Lite first; it will take only a few seconds. Then download TypeMat\_Lite. It will take approximately 20 minutes to download and install. Even if you close the browser at this point, the database installation should continue.  

Installation of MiGA-AWS is complete. See the next section for how to stop and restart the instance. (You do not want to pay for it when you are not using it!) Then run the tutorials that follow to gain experience using MiGA-AWS. For the command line version there are some important differences from the Docker version of MiGA.  

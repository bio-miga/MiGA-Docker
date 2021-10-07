# MiGA for Amazon Web Services

## Introduction
For those who have large MiGA jobs to run and no access to a compute cluster at their institution, Amazon Web Services (AWS) provides an answer. While one could install Docker for Linux in an AWS EC2 instance and then the Docker version of MiGA, we offer a MiGA Amazon Machine Image (AMI) which provides both command line and web-based (MiGA-Web) functionality.

## Create an AWS Account

If you do not have an AWS account, go to https://aws.amazon.com/ and create one. You will need to provide a credit card for billing purposes. New account holders get up to 750 compute hours per month for 12 months on free tier instances.

## Create a Key Pair

To log into an instance via a terminal, you will need to download a key pair to your local computer. You will also need it to download results via an FTP client *e.g.* FileZilla.  

https://aws.amazon.com/getting-started/launch-a-virtual-machine-B-0/

## Setting up MiGA on AWS

First time users should take the following steps. The procedures are slightly different if you are returning.

1. Log into your AWS account.  
Open your browser and log into your AWS account.  
1. Select the region.  
From near the right-hand end of the menu bar at the top of the screen, select the region "US East (Ohio) us-east-2." The MiGA AMI is available in this region.  
1. Launch an EC2 instance.  
From near the left-hand end of the menu bar at the top of the screen, select "Services" and then "EC2" located under the "Compute" category. On the page that opens, click "Launch instance" and select "Launch instance" fromt he drop-down menu that opens.   
1. Choose the MiGA AMI.  
Click on "My AMIs" on the left-hand side of the screen and then select "MiGA-Prima1.0.5.1."  
1. Choose the instance type.  
On the page tha opens, choose an instance type. For test purposed, you may choose the free tier "t2-micro" instance. For "real work," you will need to choose an instance with more vCPUs, memory, and preferably higher network peroformance. We recommend 4 Gb of memory per vCPU. You may compare costs at https://aws.amazon.com/ec2/pricing/on-demand/. After clicking on the instance type you want to use, click on "Next: Configure Instance Details" at the bottom of the screen.  
1. Configure the IAM role  
On the page that opens, choose "SSMRole" fromthe drop-down menu next to "IAM role."  
1. Add EBS storage. 
Click on "Next: Add storage" at the bottomof the page. The 100 GiB EBS volume /dev/sdb is already attached to the instance. If you want a larger volume, delete, click on "Add New Volume" and choose what you want. We suggest that you leave the box "Delete on Termination" unchecked so that you may save the storage volume separately from the instance.  
1. Add tags.  
Click on "Next: Add tags" at the bottom of the screen. You do not need to add any, but can include a name tag if you wish.  
1. Add the security group.  
Click on "Next: Add Security Group" at the bottom of the page. The "Create a **new** security group" button should already be checked. You may enter a security group name and description meaningful to you if you wish. Under "Type," "SSH" should already be included. Click on "Add Rule" and select "HTTP." Click on "Review and Launch" a the bottom of the screen. Scroll down the page that opens and click "Launch."   
1. Select the key pair.  
In the boc that opens, select the key pair you previously created from the lower drop-down mneu, or if necessary, choose "Creat a new key pair" from the upper drop-down menu. A key pair is not necessary if you will use only MiGA-Web, but it is necessary to log into the instance using a terminal and to retieve results via FTP. Check the "I accknowlege ..." box and then click on "Launch instances."  
1. View instances.  
On the page that opens, clickon "Services" near the left-hand end of the meu bar at the top of the page, choose "EC2" and the "Instances running." Under "Name", enter a meaningful name so that you can recognize the instance later. Wait until "2/2 checks" is displayed under "Status Checks."
1. Open the session terminal.   
Clickon "Services" near the left-hand end of the menu bar at the top of the page and choose "Systems Manager." If the choice "Systems Manager" is not visible, enter "Systems Manager" in the search box at the top of the page and then choose "Systems Manager." On the page that opens, choose "Session Manager" from the menu on the left of the screen. On the page tha opens, click on "Start session,"  choose your instance by clicking on the empty circle (radio button) next to its name, and then click  on "Start session." This should open a terminal window.  

1. Enter the folloiwng into the session terminal, one line at a time.  
The lines beginning with # need not be entered. They are comments explaining the purpose fo the following line.


```
# Get super user status
sudo su 
# Go to the root directory.
cd
# Check that you are in the root directory.
pwd
# List the block devices.
lsblk
# Creat a file system on block /dev/xvdb.
# Note the last entry onder the colume "NAME."
# You need to create a file system on this device.
# Enter the following, substituting device_name withthe actual name of the device.
# For example, if the name is xvdb, enter
sudo mkfs -t xfs /dev/xvdb
# Mount miga-data on the /dev/xvdb device.
sudo mount /dev/xvdb miga-data
# Check tha miga-data is available.
lsblk
# Make a database directory in miga-data  
mkdir miga-data/db
# Copy the database files.
cp -r /miga-web/db/* /root/miga-data/db/
# Move to the miga-data directory 
cd miga-data/
# Bind the two database directories.
sudo mount --bind db ../../miga-web/db 
# Move to the miga-web directory.
cd ../../miga-web
# Start the server.
export SECRET_KEY_BASE='bundle exec rake secret'  
bundle exec rails server -e production -b 0.0.0.0 -p 80 Puma
```
After a few seconds the srver should start. 

Return to the EC2 page and get the public IP address. Should have inclulded instructions earlier to get it and write it down.

Open your browser and enter ```http://the _public_ip_address```. The MiGA-Web Welcome page will open and you may begin using MiGA-Web. If you close your browser, the instance will continue to run until you log back into your AWS account and stop it.   

Alternatively, open a terminal (Git Bash is recommended for Windows users) and enter:

```
ssh -i .ssh/your_key_file_name ubuntu@the _public_ip_address
```

Enter "yes" when prompted to log into your AWS instance. You will be in user ubuntu's home directrory. MiGA projects must be created in the /root/miga-data directory, so to get there, enter:

```
sudo su
cd
cd miga-data
```
Wehn you close the terminal, a job enterd fromthe terminal will be killed. This can be avaoided by first createing a tmux session running the job in that session. After exiting the session, you may close the terminal and the job will continue running. See vvvvvvvvv for an example of how to so this.  


 

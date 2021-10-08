## Setting up MiGA on AWS

First time users should take the following steps. The procedures are slightly different if you are returning.

1. Log into your AWS account.  
Open your browser and log into your AWS account.  
1. Select the region.  
From near the right-hand end of the menu bar at the top of the screen, select the region "US East (Ohio) us-east-2." The MiGA AMI is available in this region.  
1. Launch an EC2 instance.  
From near the left-hand end of the menu bar at the top of the screen, select "Services" and then "EC2" located under the "Compute" category. On the page that opens, click "Launch instance" and select "Launch instance" from the drop-down menu that opens.   

1. Choose the MiGA AMI.  
Click on "My AMIs" on the left-hand side of the screen and then select "MiGA-Prima1.0.5.1."  

1. Choose the instance type.  
On the page tha opens, choose an instance type. For test purposes, you may choose the free tier "t2-micro" instance. For "real work," you will need to choose an instance with more vCPUs, memory, and preferably higher network peroformance. We recommend 4 Gb of memory per vCPU. You may compare costs at https://aws.amazon.com/ec2/pricing/on-demand/, but keep in mind that instances with greater resoures will finish in less time.  
After clicking on the instance type you want to use, click on "Next: Configure Instance Details" at the bottom of the screen.  

1. Configure the IAM role  
On the page that opens, choose "SSMRole" from the drop-down menu next to "IAM role."  

1. Add EBS storage.   
Click on "Next: Add storage" at the bottom of the page. The 100 GiB EBS volume /dev/sdb is already attached to the instance. If you want a larger volume, delete, click on "Add New Volume" and choose what you want. We suggest that you leave the box "Delete on Termination" unchecked so that you may save the storage volume separately from the instance.  

1. Add tags.  
Click on "Next: Add tags" at the bottom of the screen. You do not need to add any, but can include a name tag if you wish.  

1. Add the security group.  
Click on "Next: Add Security Group" at the bottom of the page. The "Create a **new** security group" button should already be checked. You may enter a security group name and description meaningful to you if you wish. Under "Type," "SSH" should already be included. Click on "Add Rule" and select "HTTP." Click on "Review and Launch" a the bottom of the screen. Scroll down the page that opens and click "Launch."   

1. Select the key pair.  
In the box that opens, select the key pair you previously created from the lower drop-down menu, o, if necessary, choose "Creat a new key pair" from the upper drop-down menu. A key pair is not necessary if you will use only MiGA-Web, but it is necessary to log into the instance using a terminal and to retieve results via FTP. Check the "I acknowlege ..." box and then click on "Launch instances."  

1. View instances.  
On the page that opens, clickon "Services" near the left-hand end of the meu bar at the top of the page, choose "EC2" and the "Instances running." Under "Name", enter a meaningful name so that you can recognize the instance later. Wait until "2/2 checks" is displayed under "Status Checks." Scroll down the page until you ocan see the public IP address. Write down the address. You will need it to log into the instance.

1. Open the session terminal.   
Click on "Services" near the left-hand end of the menu bar at the top of the page and choose "Systems Manager." If the choice "Systems Manager" is not visible, enter "Systems Manager" in the search box at the top of the page and then choose "Systems Manager." On the page that opens, choose "Session Manager" from the menu on the left of the screen. On the page tha opens, click on "Start session,"  choose your instance by clicking on the empty circle (radio button) next to its name, and then click  on "Start session." This should open a terminal window.  

1. Enter the following into the session terminal, one line at a time.  
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
```
Note the last entry under the column "NAME." You need to create a file system on this device. In the code below, the device name is xvdb. Substitute xvdb with the actual name as necessary.  

```
sudo mkfs -t xfs /dev/xvdb
# Mount miga-data on the /dev/xvdb device.
sudo mount /dev/xvdb miga-data

# Check that miga-data is available.
lsblk

# Make a database directory in miga-data.  
mkdir miga-data/db

# Copy the database files.
cp -r /miga-web/db/* /root/miga-data/db/

# Move to the miga-data directory.
cd miga-data/

# Bind the two database directories.
sudo mount --bind db ../../miga-web/db 

# Move to the miga-web directory.
cd ../../miga-web

# Start the server.
export SECRET_KEY_BASE='bundle exec rake secret'  
bundle exec rails server -e production -b 0.0.0.0 -p 80 Puma
```
After a few seconds the server should start. 

Open your browser and enter ```http://the _public_ip_address_you_wrote_down```. The MiGA-Web Welcome page will open and you may begin using MiGA-Web. If you close your browser, the instance will continue to run until you log back into your AWS account and stop it.   

Alternatively, open your Mac, Ubuntu or git bash terminal. Assuming the following:  
- .ssh is the path to your key file
- MyKeyPair.pem is the name of your key file
- 18.190.158.158 is the public IP address
 
Enter:

```
ssh -i ~/.ssh/MyKeyFile.pem ubuntu@18.190.158.158
```

Enter "yes" when prompted to log into your AWS instance. You will be in user ubuntu's home directory. MiGA projects must be created in the ```/root/miga-data``` directory; to get there, enter:

```
sudo su
cd
cd miga-data
```
If you create a MiGA job at this point, the job will be killed if you close the terminal. This can be avoided by first creating a ```tmux``` session and running the job in that session. In that case, you may exit the ```tmux``` session, close the terminal, and the job will continue running. See the **MiGA-CLI Project** section for details on how to do this.  

## Saving and Restarting the MiGA Instance

To save your MiGA instance, go the the page displaying it and from the "Actions" menu choose "Instance State" and then "Stop." Then the next time you log into your AWS account you may restart it by choosing it and from the "Actions" menu choosing "Instance State" and then "Start." It will be assigned a new public IP address when you do this, so be sure to note it. If you choose "Terminate" instead of "Stop" the instance will be deleted and you will not be able to return to it.  

When you restart the MiGA instance, the server will not be running and so the instance will not be accessible via a browser. Also, any previous results will not be available until you remount the device they are on. See the next section **Using MiGA AWS** for how to address these problems.  

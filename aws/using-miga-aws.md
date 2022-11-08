
## Stopping a MiGA AWS Instance

In setting up the MiGA AWS instance, we configured it so that all data is written to a storage volume separate from the root volume. We did this for two reasons: 1) so that the data can still be retrieved even if the MiGA instance is terminated, and 2) to allow the data volume to be attached to a different MiGA instance. For example, you will likely want to create a new MiGA instance after a newer MiGA AMI becomes available. Also, you might have different instance types: a cheaper one with fewer vCPUs and less memory for running tutorials and browsing results, and another with more resources for running large jobs.  

There are two ways of stopping an instance, depending on whether or not you want to detach the storage volume.  

### Stopping WITHOUT Detaching the Storage Volume

This method is appropriate if you will be returning to the same instance and storaqge volume shortly. Simply log into your AWS account, go to the EC2 Dashboard and find your running instance. Right click on it, choose Instance State, and then "Stop." **CAUTION**: If you instead choose "Terminate" the instance will be deleted. If you did not leave the box unchecked when you attached the storage volume, it will also be deleted.  

### Restarting if Storage Volume Was Not Detached

If you did not detach the storage volume when you stopped the instance, log into your AWS account, go to the EC2 Dashboard and find your instance. Right click on it, choose "Instance State," and then "Start." Confirm by clicking "Yes, Start." Wait until "2/2 checks" appears in the column "Status Checks." A new public IP address will be assigned to the instance. Open a teminal and connect to the instance using this new IP address and check the name of the storage volume with the command `lsblk`.   


```
ssh -i .ssh/<key pair> ubuntu@<ip address>
cd
sudo su - ubuntu
lsblk
```

As you did when you created the instance, you should get something like:

```
NAME        MAJ:MIN RM  SIZE RO TYPE MOUNTPOINT
loop0         7:0    0   25M  1 loop /snap/amazon-ssm-agent/4046
loop1         7:1    0 55.5M  1 loop /snap/core18/2246
loop2         7:2    0 61.9M  1 loop /snap/core20/1242
loop3         7:3    0 61.9M  1 loop /snap/core20/1270
loop4         7:4    0 55.5M  1 loop /snap/core18/2253
loop5         7:5    0 67.3M  1 loop /snap/lxd/21545
loop6         7:6    0 67.2M  1 loop /snap/lxd/21835
loop7         7:7    0 32.5M  1 loop /snap/snapd/13640
loop8         7:8    0 43.3M  1 loop /snap/snapd/14295
nvme1n1     259:0    0  100G  0 disk
nvme0n1     259:1    0   50G  0 disk
└─nvme0n1p1 259:2    0   50G  0 part /
```

You need to remount the storage volume, `nvme1n1` in this example. And then bind mount the db directories in `miga-web  and    miga-data`.  

```
# Mount miga-data on the /dev/nvme1n1 device.
sudo mount /dev/nvme1n1 miga-data

# Bind the two database directories.
cd miga-data
sudo mount --bind db ../miga-web/db
```

And then start the web-server in a tmux session:  

```
tmux new -s web-server
cd
cd miga-web/
export SECRET_KEY_BASE='bundle exec rake secret'
export RAILS_SERVE_STATIC_FILES=true
bundle exec rails server -e production -b 0.0.0.0 -p 8080 -u Puma
```

Exit the tmux session by entering Ctrl-B d. You can then access the MiGA instance by both browser and command line interfaces.   


## Detaching the Storage Volume and Stopping the Instance

While connected to the MiGA instance via a terminal, enter:  

```
cd
sudo umount miga-web/db
```
If necessary, log into your AWS account. From the menu on the left side of the AWS console page, select Volume under Elastic Block Store and find the storage volume attached to your MiGA instance. Right click on the volume and choose "Detach." Wait until the detachment is complete.  

Then select "Instances" (under "Instances") from the menu on the left of the screen and find your running MiGA instance. Right click on the instance and choose first "Instance State" and then "Stop" from the drop-down menu. If instead you chose "Terminate" the MiGA instance will be deleted.  

## (Re-)Attaching the Storage Volume and Starting a MiGA AWS Instance 

You can reattach the storage volume to the same instance from which it was detached, or you can attach it to a new or different instance provided the instance is in the same sub-region as the storage volume. The sub-region is indicated by the letter appended to the availability zone. To check that the regions match, log into the AWS console and look under the Availability Zone column in the list of instances and the list of EBS volumes. 

Go to the EC2 Dashboard and choose Instances. On the page that opens, choose your MiGA instance and click on "Actions" at the top of the screen. From the drop-down menu, choose "Instance State" and then "Start." Write down the public IP address displayed in the lower part of the screen. Note the name and instance id of your instance. Wait until the entry for the instance under "Status Checks" is "2/2 checks."  

Next you need to attach or reattach the data storage volume. Under "Elastic Block Store" on the left of the screen, choose "Volumes" and find the storage volume you want to attach to your instance. Left click on the instance and choose "Attach Volume". In the box that opens, click in the field next to "Instance," select your running instance, and then click on the blue "Attach" button in the bottom of the box. Wait until the operation is complete (the spinning wheel will disappear).  

The rest of the procedure is the same as above: log into the instance from a terminal, mount the storage volume, bind mount the two db directories, and start the server.  

You can then access your MiGA instance by both browser (MiGA-Web) and command line interfaces, and previous projects should be available. 

## MiGA-Web Projects

Access MiGA-Web by entering http://\<public IP address\>:8080 into your browser. Log into your account; you should not have to create a new one. You can then practice by following any of the tutorials in the **MIGA_WEB PROJECTS** section.

## MIGA Command Line Projects

Open a Mac, Ubuntu or git bash terminal and log into the MiGA instance as instructed in **Setup a MiGA Instance**:

```
cd
ssh -i ~/.ssh/<key_pair_file> ubuntu@<ip_address>
```

Command line (CLI) projects need to be created in the /home/ubuntu/miga-data directory. To go there after logging into your instance, enter:

```
cd miga-data
```
You can then practice running the tutorials included in the **MIGA-CLI PROJECTS** section. The results will be available via the web interface if you link them. 

## Making CLI Projects Accessible via the Web Interface

CLI projects created in the /home/ubuntu/miga-data directory can also be accessed via the web interface if they are linked. To make the link, in MiGA-Web go to the Admin console, scroll to the bottom of the page and click on "Link existing projects." Be sure to link the project as a public project so that it will still be available if you ever have to log in under a different account.

## Long Running Projects

MiGA-Web projects will continue to run after you close your browser. As long as you do not stop the instance, you can log back in with your browser using the same IP address. CLI projects, however, will terminate when you close your terminal or your connection to the internet is interrupted. This inconvenience can be overcome by using ```tmux```. It is good practice to give the ```tmux``` session a name. For example, create a ```tmux``` session named "my_session" by entering:  

```
tmux new -s my_session
```

A green band will appear at the bottom of your terminal indicating you are in a ```tmux``` session. Enter commands to run your project and after it has started, enter Ctrl and b, release both keys and type d. This will take you out of the ```tmux``` session and you can close your terminal, turn off your computer and go home. As long as you do not stop the instance, you can resume the ```tmux``` session by logging back in from your terminal and entering:

```
tmux attach-session -t my_session
```

## Accessing Results
See the **Exploring Results** section for general ways to get results. Some results may be downloaded using the web interface, but in some cases *e.g.* downloading an archive of a project, you may wish to use an FTP client such as FileZilla.  With the instance running, you can log in using FileZilla and download files.  

To configure FileZilla to access an instance:  
- For protocol, select SFTP - SSH File Transfer Protocol.  
- For host, enter the IP address.  
- For Logon Type, select Key file.  
- For User, enter ubuntu.  
- For Key file, enter the path to and name of your key file. You may do this using the Browse button.  

You may save this configuration as a new site, but remember that you will have to edit the IP address each time you re-start the instance.
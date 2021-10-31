## Trouble Shooting

### Terminal is Disconnected

The instance will continue running until you stop or terminate it. Your terminal's connection to the instance, however, may be automatically terminated after a period of inactivity. If this happens, you can simply log back in (substitute \<keypair file> with the actual file name and \<public IP address> with the actual address):

```
cd  # To take you to your home directory.
ssh -i .ssh/<keypair file> ubuntu@<public IP address>
```
Any jobs running when the terminal was disconnected, however, will be lost. For this reason, any important jobs should be run in a tmux session. See the "Long Running Projects" section below.    

### MiGA-Web is Disconnected

If your browser's connection to MiGA-Web is terminated and the MiGA web server is still running, you can reconnect by re-entering ```http://<public IP address>:8080``` into your browser's URL bar. If this does not work it is likely that the MiGA web server has stopped. In this case, log into the instance using a terminal or the Session Manager and restart the server.  
If you use the Session Manager, be sure to log into the user ubuntu account:  

```
sudo su - ubuntu
```
If you use a terminal, log in the usual way:

```
cd
ssh -i .ssh/<keypair file> ubuntu@<public IP address>
```
The web-server is less likely to stop if it is started in a tmux session. Begin a tmux session named web-server by entering:  

```
tmux new -s web-server
```
You may of course use a different name if you wish. A green bar will appear across the bottom of the screen inidcating that you are in a tmux session. Then enter:  

```
cd /miga-web/
export SECRET_KEY_BASE='bundle exec rake secret'  
bundle exec rails server -e production -b 0.0.0.0 -p 8080 Puma
```
Exit the tmux session with Ctrl-B d. The server will conitnue to run even after you close the session manager or log out and close the terminal.  

## Stopping a MiGA AWS Instance

In setting up the MiGA AWS instance, we configured it so that all data is written to a storage volume separate from the instance itself. We did this for two reasons: 1) so that the data can still be retrieved even if the MiGA instance is terminated, and 2) to allow the data volume to be attached to a different MiGA instance. For example, you will likely want to create a new MiGA instance after a newer MiGA AMI becomes available. Also, you might have different instance types: one with fewer vCPUs and less memory for running tutorials and another with more resources for running large jobs.  

To stop your MiGA instance, log into your AWS account, go to the EC2 Dashboard and find your running instance. Connect to it  using the Session manager and enter:  

```
sudo umount /miga-web/db
```
From the menu on the left side of the AWS console page, select Volume under Elastic Block Store and find the storage volume attached to your MiGA instance. Left click on the volume and choose "Detach." Wait until the detachment is complete.  

Then select "Instances" (under "Instances") from the menu on the left of the screen, and find your running MiGA instance. Left click on the instance and choose first "Instance State" and then "Stop" from the drop-down menu. If instead you chose "Terminate" the MiGA instance will be deleted.  

## Restarting a MiGA AWS Instance 

Log into the AWS console, go to the EC2 Dashboard, and choose Instances. On the page that opens, choose your MiGA instance and click on "Actions" at the top of the screen. From the drop-down menu, choose "Instance State" and then "Start." Write down the public IP address displayed in the lower part of the screen. Note the name and instance id of your instance. Wait until the entry for the instance under "Status Checks" is "2/2 checks."  

Next you need to reattach the data storage volume. Under "Elastic Block Store" on the left of the screen, choose "Volumes" and find the storage volume you want to attach to your instance. Left click on the instance and choose "Attach Volume". In the box that opens, click in the field next to "Instance," select your running instance, and then click on the blue "Attach" button in the bottom of the box. Wait until the operation is complete (the spinning wheel will disappear).

Go back to your instance, choose it, and connect to it using the Session Manager. Enter the following commands:  

```
# Log in as user ubuntu
sudo su - ubuntu
 
# Check that you are in the /home/ubuntu directory.
pwd

# List the block devices.
lsblk
```
Note the name of your storage device. You need to mount the miga-data directory on this device. In the code below, the device name is xvdb. Substitute xvdb with the actual name as necessary.  

```
# Mount miga-data on the /dev/xvdb device.
sudo mount /dev/xvdb miga-data

# Check that miga-data is available.
lsblk

# Move to the /home/ubuntu/miga-data directory.
cd /home/ubuntu/miga-data

# Bind the two database directories.
sudo mount --bind db /miga-web/db 

# Move to the miga-web directory.
cd /miga-web
```
Creat a tmux session by entering:  
```
tmux new -s web-server
```
You may of course use a different name if you wish. A green bar will appear across the bottom of the screen inidcating that you are in a tmux session. Then enter:  

```
cd /miga-web/
export SECRET_KEY_BASE='bundle exec rake secret'  
bundle exec rails server -e production -b 0.0.0.0 -p 8080 Puma
```
Exit the tmux session with Ctrl-B d. The server will conitnue to run after you close the session manager.  

You can then access your MiGA instance by both browser (MiGA-Web) and command line interfaces, and previous projects should be available. 

## MiGA-Web Projects

Access MiGA-Web by entering http://\<public IP address\>:8080 into your browser. Log into your account; you should not have to create a new one. You can then practice by following any of the tutorials in the **MIGA_WEB PROJECTS** section.

## MIGA Command Line Projects

Open a Mac, Ubuntu or git bash terminal and log into the MiGA instance as instructed in **Setup a MiGA Instance**:

```
cd
ssh -i ~/.ssh/MyKeyPair.pem ubuntu@ip_address
```

Command line (CLI) projects need to be created in the /home/ubuntu/miga-data directory. To go there after logging into your instance, enter:

```
cd miga-data
```
You can then practice running the tutorials included in the **MIGA-CLI PROJECTS** section. The results will be available via the web interface if you link them. 

## Making CLI Projects Accessible via the Web Interface

CLI projects created in the /home/ubuntu/miga-data directory can also be accessed via the web interface if they are linked. To make the link, in MiGA-Web go to the Admin console, scroll to the bottom of the page and click on "Link existing projects." Be sure to link the project as a public project so that it will still be available if you ever have to log in under a different account.

## Long Running Projects

MiGA-Web projects will continue to run after you close your browser. As long as you do not stop the instance, you can log back in with your browser using the same IP address. CLI projects, however, will terminate when you close your terminal or your connection to the internet is interupted. This inconvenience can be overcome by using ```tmux```. It is good pracice to give the ```tmux``` session a name. For example create a ```tmux``` session named "my_session" by entering:  

```
tmux new -s my_session
```

A green band will appear at the bottom of your terminal indicating you are in a ```tmux``` session. Enter commands to run your project and after it has started, enter Ctrl and b, release both keys and type d. This will take you out of the ```tmux``` session and you can close your teminal, turn off your computer and go home. As long as you do not stop the instance, you can resume the ```tmux``` session by logging back in from your terminal and entering:

```
tmux attach-session -t my_session
```

## Accessing Results
See the **Exploring Results** section for general ways to get results. Some results may be downloaded using the web interface, but in some cases *e.g.* downloading an archive of a project, you may wish to use an FTP client such as FileZilla.  With the instance running, you can log in using FileZilla and download files.  

To configure FileZilla to access to an instance:  
- For protocol, select SFTP - SSH File Transfer Protocol.  
- For host, enter the IP address.  
- For Logon Type, select Key file.  
- For User, enter ubuntu.  
- For Key file, enter the path to and name of your key file. You may do this using the Browse button.  

Yoiu may save this cofiguration as a new site, but remember that you will have to edit the IP address each time you re-start the instance.
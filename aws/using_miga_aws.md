## Restarting a MiGA Instance
When you restart a MiGA instance any peviouos results will not be immediately available, nor will the instance be accessible via a browser. You will have to repeat some of the session manager steps in the **Setup a MiGA Instance** section. Open the session terminal by repeating the instructions under item 12 of that section. Then enter the following into the terminal:

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
Note the last entry under the column "NAME." You need mount the miga-data directory on this device. In the code below, the device name is xvdb. Substitute xvdb with the actual name as necessary.  

```
# Mount miga-data on the /dev/xvdb device.
sudo mount /dev/xvdb miga-data

# Check that miga-data is available.
lsblk

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
You can then access your MiGA instance by both browser (MiGA-Web) and command line interfaces, and previous projects should be available. You will, however need to create a new MiGA-Web account. For this reason, via the web interface you should always create public projects. Otherwise peviously created private projects will not be available via the web.  

## MiGA-Web Projects

Access MiGA-Web by entering http://The_ip_address into your browser. Create an account if necessary. Log into your account. You can then practice by following any of the tutorials in the **MIGA_WEB PROJECTS** section.

## MIGA Command Line Projects

Open a Mac, Ubuntu or git bash terminal and log into the MiGA instance as instructed in **Setup a MiGA Instance**:

```
ssh -i ~/.ssh/MyKeyPair.pem ubuntu@ip_address
```

Command line (CLI) projects need to be created in the /root/miga-data directory. To go there after logging into your instance, enter:

```
sudo su
cd 
cd miga-data
```
You can then practice my running the tutorials included in the **MIGA-CLI PROJECTS** section. The results will be available via the web interface if you link them. 

## Making CLI Projects Accessible via the Web Interface

CLI projects created in the /root/miga-data directory can also be accessed via the web interface if they are linked. To make the link, in MiGA-Web go to the Admin console, scroll to the bottom of the page and click on "Link existing projects." Be sure to link the project as a public project.

## Long Running Projects

MiGA-Web projects will continue to run after you close your browser. As long as you do not stop the instance, you can log back in with your browser using the same IP address. CLI projects, however, will terminate when you close your terminal or your connection to the internet is interupted. This incovnience can be overcome by using ```tmux```. After logging in via your terminal, simply enter the command ```tmux```. A green band will appear at the bottom of your termina indicating you are in a ```tmux``` session. Enter commands to run your project and after it has started, enter Ctrl and b, release both keys and type d. This will take you out of the ```tmux``` session and you can close your temina, turn off your computer and go home. As long as you do not stop the instance, you can resume the ```tmux``` session by logging back in from your terminal and entering:

```
sudo su # Because the project was created using su status
tmux a
```

## Accessing Results
See the **Exploring Results** section for general ways to get results. Some results may be downloaded using the web interface, but in some cases *e.g.* downloading an archive of a project, you may wish to use an FTP client such as FileZilla.  However, the only directory that can be accessed via FileZilla is the user home directory /home/ubuntu. Therefore you first have to copy results for FTP download to that directory. Then, with the instance running, you can log in using FileZilla and download the files.  

To configure FileZilla to access to an instance:  
For protocol, select SFTP - SSH File Transfer Protocol.  
For host, enter the IP address.  
For Logon Type, select Key file.  
For User, enter ubuntu.  
For Key file, enter the path to and name of your key file. You may do this using the Browse button.  

Yoiu may save this cofiguration as a new site, but remeber that you will have to edit the IP address each time you re-start the instance.
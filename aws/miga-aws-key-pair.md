## Create a Key Pair

To log into an instance via a terminal, you will need to download a key pair to your local computer. You will also need it to download results via an FTP client *e.g.* FileZilla.  

The following instructions will allow you to create and download a key pair and test using it to connect to an Amazon instance.  

You need a Linux bash shell to complete these instructions. For Linux and Mac users, the terminal provides this. Windows users have a choice of Windows PowerShell, the Ubuntu terminal if they have WSL2 and Ubuntu installed, or git bash, a lightweight tool also useful for working with (surprise!) git and that is included in Git for Windows (https://gitforwindows.org/).  

1. Log into your AWS account and choose the region US East (Ohio) from the menu bar at the top of the screen. Key pairs are specific to a region, and the MiGA AMI is in the US East (Ohio) region.  
1. Under Services, select "Compute" and then "EC2".  
1. Click on "Launch Instance" and select "Launch instance" from the pull-down menu that opens.    
1. On the page that opens, under "Application and OS Images" click on the "Amazon Linux AWS" button and select the Amazon Linux 2 AMI. It is likely the first listed. Leave the architecture as 64-bit (x86).  
1. Under "Instance type", select  the "t2.micro" instance.  
1. Under "Key pair (login)," click on "Create new key pair." In the smaller window that opens, enter a key pair name in the box provided. Leave Key pair type as "RSA" and Private key file format as ".pem." Then click on the orange butom "Create key pair." The key pair file will be downloaded to your comuter, most likely into the downloads folder.
1. Open your terminal (Mac orLinux) or start git bash (Windows). When the terminal opens, you should be in your home directory. 
1. Move the downloaded file to the hidden directory .ssh in your home directory with the command below. You may need to edit the path to the downloaded file. (And, of course, use the actual name of your key pair file in all following commands.) 

```
mv ~/Downloads/MyKeyPair.pem ~/.ssh/MyKeyPair.pem
```
**Note for Mac users:** The hidden directory ~/.ssh may not exist on your system, in which case you need to create it before issuing the above command. You can check if it exists by listing the files in your home directory with:  

```
cd
ls -la
```

If the directory does not exist, create it with:  

```
cd 
mkdir .ssh
```

Also, the extension "text" may have been appended to the downloaded file name, in which case you can rename it as it is moved to .ssh with:  

```
mv ~/Downloads/MyKeyPair.pem.text ~/.ssh/MyKeyPair.pem
```

10. Make the key file read only with the following commands:

```
cd ~/.ssh
chmod 400 MyKeyPair.pem
```

11. Back in your browser, launch your instance by clicking on the "Launch" button.   
1. Click on "Services" near the left-had end of the menu bar at the top of the screen and select EC2 on the page that opens.  
1. On the page that opens, select "Instances" and then on the next page select the running instance. Information about the instance will be displayed. Copy the public IPv4 address. You may copy it to your clipboard by clicking on the copy icon just to the left of it.  
1. To test that you can connect to your instance, go back to your terminal and enter:  

```
ssh -i ~/.ssh/MyKeyPair.pem ec2-user@the_ip_address
```
15. Enter "yes" to continue. Ignore any message about an invalid format when the key loads. After a moment, the terminal prompt should change to something like `[ec2-user@ip-172-31-4-137 ~]$` indicating that you are logged into your Amazon instance.

1. Disconnect by entering:  

```
exit
```
17. Go back to the web page displaying your instance and select it if necessary. Click on the drop-down menu button "Actions," choose "Instance State," and then select "Terminate." A box will open, asking you to confirm. Click "Terminate."
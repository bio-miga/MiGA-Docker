## Create a Key Pair

To log into an instance via a terminal, you will need to download a key pair to your local computer. You will also need it to download results via an FTP client *e.g.* FileZilla.  

The following instructions will allow you to create and download a key pair and test using it to connect to an Amazon instance.  

You need a Linux bash shell to complete these instructions. For Mac users, the terminal provides this. If Windows users have WSL2 and Ubuntu installed, they may use the Ubuntu terminal. Otherwise Windows users should install Git for Windows (https://gitforwindows.org/) which includes git bash, a lightweight tool also useful for working with (surprise!) git.  

1. Log into your AWS account and choose the region US East (Ohio) from the menu bar at the top of the screen. Key pairs are specific to a region, and the MiGA AMI is in the US East (Ohio) region.  
1. Under Services, select EC2.  
1. Click on the orange pull-down menu "Launch Instance" and select "Launch instance."  
1. On the page that opens, select the Amazon Linux AMI. It is likely the first listed.  
1. On the page that opens, select  the "t2.micro" instance. Then click on the blue button  "Review and Launch."  
1. On the page that opens, scroll down and click the blue button "Launch." A box will open for you to select a key pair or create a new key pair. In the space under Key pair name, enter a name for the key pair, *e.g.* MyKeyPair, and then click the button "Download Key Pair."   
1. Open you Mac or Ubuntu terminal, or start git bash. When the terminal opens, you should be in your home directory. 
1. Move the downloaded file to the hidden directory .ssh in your home directory with the command below. You may need to edit the path to the downloaded file.  

```
mv ~/Downloads/MyKeyPair.pem ~/.ssh/MyKeyPair.pem
```
**Note for Mac users:** The hidden directory ~/.ssh may not exist on your system, in which case you need to create it beforee issuing the above command. You can check if it exists by listing the files in your home directory with:  

```
cd
ls -la
```

If the file does not exist, create it with:  

```
cd 
mkdir .ssh
```

Also, the extention "text" may have been appended to the downloaded file name, in which case you can rename it as it is moved to .ssh with:  

```
mv ~/Downloads/MyKeyPair.pem.text ~/.ssh/MyKeyPair.pem
```

8. Make the file read only with the following commands:

```
cd ~/.ssh
chmod 400 MyKeyPair.pem
```

9. Back in your browser, launch your instance.  
10. Click on "Services" near the left-had end of the menu bar at the top of the screen and select EC2 on the page that opens.  
11. On the page that opens, select "Instances (running)." Copy the public IP address. You may copy it to your clipboard by hovering just to the right of it.  
12. To test that you can connect to your instance, go back to your terminal and enter:  

```
ssh -i ~/.ssh/MyKeyPair.pem ec2-user@the_ip_address
```
13. Enter "yes" to continue. After a moment, the terminal prompt should change to something like [ec2-user@ip-172-31-4-137 ~]$ indicating that you are logged into your Amazon instance.

14. Disconnect by entering:  

```
exit
```
15. Go back to the web page displaying your instance and select it if necessary. Click on the drop-down menu button "Actions," choose "Instance State," and then select "Terminate." A box will open, asking you to confirm. Click on the blue button, "Yes, Terminate."
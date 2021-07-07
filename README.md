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

I used the nmap -sV -v 192.168.1.105 command to do a service and version scan on the webserver.
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

 I opened the webbrowser on the attacker machine(Kali) and accessed the webserver using  its  ip 192.168.1.105.
 
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
 
 Downloaded the rockyou.txt file. 
 
![alt-text]( https://github.com/Reeti4cyber/Red-vs-Blue-Team-Project/blob/main/Images/Imagerockyou.png)
 
 and used the hydra command to Brute Force the password.
 
```
hydra -l ashton -P ./root/Downloads/rockyou.txt -s 80 -f -vV 192.168.1.105 http-get "/company_folders/secret_folder"
```

![alt-text](https://github.com/Reeti4cyber/Red-vs-Blue-Team-Project/blob/main/Images/ashtonpsswd.png)
 
 


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
### **Following is the output:**
 
![alt-text](https://github.com/Reeti4cyber/Red-vs-Blue-Team-Project/blob/main/Images/Image%202.png)
 
 I got the ip details of all the machines on the subnet.
 
| IP | Machine |
|:-------------:|:-------------:|
| 192.168.1.1 | Hyper-V, Gateway IP |
| 192.168.1.105 | Capstone, target machine |
| 192.168.1.100 | ELK server |

Capstone machine has an ip of 192.168.1.105.
 
 ### **Navigating the Webserver:**

As I got the ip of Capstone Vm, which is a webserver. I opened the webbrowser on the attacker machine(Kali) and accessed the webserver using  its  ip 192.168.1.105

![alt-text](https://github.com/Reeti4cyber/Red-vs-Blue-Team-Project/blob/main/Images/Image%203.png)

In a text document the blog directory we can see a 3rd potential username – Ryan, who would potentially have the highest level access as CEO:

![alt-text](https://github.com/Reeti4cyber/Red-vs-Blue-Team-Project/blob/main/Images/Image%204.png)

In the _company folders_ directory, we can see reference to a &quot;_secret\_folder_&quot; in ALL documents within this directory, which is now a target for this Penetration Test.

![alt-text](https://github.com/Reeti4cyber/Red-vs-Blue-Team-Project/blob/main/Images/Image%205.png)

The _meet\_our\_team_ folder confirms the three potential users, and each document references the _secret\_folder:_

![alt-text](https://github.com/Reeti4cyber/Red-vs-Blue-Team-Project/blob/main/Images/Image%206.png)
As we can see below, we will need Ashton&#39;s password to gain access to the secure hidden folder.

![alt-text](https://github.com/Reeti4cyber/Red-vs-Blue-Team-Project/blob/main/Images/Image%206.png)


# Red vs Blue Team Project

# **RED TEAM - Penetration Test**

## **EXPLOITATION**
As the Red Team, I attacked a vulnerable Capstone VM within my environment, ultimately gaining root access to the machine. Entire attack was conducted using the Kali Machine.

### **Discover IP address of Linux web server:**
I used the command ip a to know the ip subnet.
```
ip a
```
![alt-text](https://github.com/Reeti4cyber/Red-vs-Blue-Team-Project/blob/main/Images/Image 1.png)

I used the netdiscover -r <subnet ip> command to discover the target ip. The subnet ip is 192.168.1.0/24:
```
netdiscover -r 192.168.1.0/24
```
### **Following is the output:**
 
![alt-text](https://github.com/Reeti4cyber/Red-vs-Blue-Team-Project/blob/main/Images/Image1.png)
 
 I got the ip details of all the machines on the subnet.
 
| IP | Machine |
|:-------------:|:-------------:|
| 192.168.1.1 | Hyper-V, Gateway IP |
| 192.168.1.105 | Capstone, target machine |
| 192.168.1.100 | ELK server |

Capstone machine has an ip of 192.168.1.105.
 

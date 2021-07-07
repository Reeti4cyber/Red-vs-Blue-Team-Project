# Red vs Blue Team Project

# **RED TEAM - Penetration Test**

## **EXPLOITATION**
As the Red Team, I attacked a vulnerable Capstone VM within my environment, ultimately gaining root access to the machine. Entire attack was conducted using the Kali Machine.

### **Discover IP address of Linux web server:**

I used the netdiscover -r <subnet ip> command to discover the target ip. The subnet ip is 192.168.1.0/24:
```
netdiscover -r 192.168.1.0/24
```
### **Following is the output:**
 


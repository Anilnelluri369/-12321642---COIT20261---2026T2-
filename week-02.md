
## Task 1: Static IP Configuration Approaches

### Implementation & Reflective Analysis
![](./images/week2-task1-topology.png)


#### Approach A: GUI Pre-Boot Editing (Hosts 1 & 2)  
Edited `/etc/network/interfaces` via GNS3's node context menu prior to booting the devices.  
Pre-boot configuration guarantees persistent initial states. Because the GNS3 daemon provisions these configurations before interface initialization, network interfaces load operational parameters immediately upon system start.
![](./images/week2-host1.png)
![](./images/week2-host1-addressshow.png)
![](./images/week2-host2.png)
![](./images/week2-host2-addressshow.png)

#### Approach B: In-Console Configuration File Editing (Host 3)  
Edited `/etc/network/interfaces` live using `nano`, then executed `ifdown eth0 && ifup eth0`.  
Editing configuration files on active devices demonstrates that Linux reads network settings at boot or interface refresh. Without invoking `ifdown`/`ifup`, runtime memory retains existing parameters despite disk modifications.

#### Approach C: Runtime Command-Line Configuration (Host 4)  
Assigned the IP address dynamically via CLI:    
ip address add 10.1.1.4/24 dev eth0  
  ![](./images/week2-host4-addressshow.png)  
  
  # Task 2 - Testing Network Connectivity and Delay with Ping

### Standard Connectivity Probing  
Initiated a basic ping from Host A to Host B (`10.1.1.2`) without command-line options and allowed at least 5 response messages before manually stopping the process via `Ctrl+C`.
## Command:
  ping 10.1.1.2  
   ![](./images/week2-task2-standardping.png)  
   ### Unreachable Host Diagnostics & Packet Loss  
   Executed a ping request to an inactive/non-existent IP address on the network (10.1.1.250) and let the request run for over 10 seconds before terminating.
    ![](./images/week2-task2-advancedparameters.png)  
    
   ### Customized ICMP Parameter Testing
Tested advanced ping flags by restricting the packet count, adjusting the transmission interval, and altering payload data size.
     ![](./images/week2-task2-unreachable.png)  
  
## GNS PROJECT
Project file Uploaded in Images file.

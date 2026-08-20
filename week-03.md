# Network Communications & Packet Capture 




## Task 1: Simple Application Communications with Netcat
![](./images/week3-topology.png)

### Step-by-Step Execution
1. Launched the Setting-IP-12321642 GNS3 topology containing four Linux hosts and an Ethernet switch.
2. Configured **Host 1** as the Netcat server listening on custom port 8080:
   nc -l -p 8080
3. Connected Host B as the Netcat client to Host A's IP address:
     nc 10.1.1.1 8080
4. Transmitted required messages across the active terminal sessions:
  -  Sent Full Name from client (Host B) to server (Host A).
  -  Sent Student ID from server (Host A) to client (Host B).
    ![](./images/wee3-task1-netcat.png)
 5. Terminated both connections using Ctrl+D

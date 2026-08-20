
## Task 1: View Routing Tables & IP Forwarding

## 1. Topology Creation
1. Open GNS3 and create a new project named `View-Routes-12321642`.
2. Drag and drop the following nodes onto the canvas:
   * **3x Linux Host** nodes (Rename: `Host1`, `Host2`, `Host3`)
   * **1x Linux Router** node (Rename: `Linux-Router`)
   * **1x Ethernet Switch** node (Rename: `Switch1`)
3. Connect the devices using the **Add a Link** tool:
   * Connect `Host1` interface `eth0` $\rightarrow$ `Switch1` port `Ethernet0`
   * Connect `Host2` interface `eth0` $\rightarrow$ `Switch1` port `Ethernet1`
   * Connect `Switch1` port `Ethernet2` $\rightarrow$ `Linux-Router` interface `eth0`
   * Connect `Linux-Router` interface `eth1` $\rightarrow$ `Host3` interface `eth0`

#### 2. Device Network Configuration (`/etc/network/interfaces`)
Before starting any node, configure its static IP address, subnet mask, default gateway, and packet forwarding status.

* **Host 1 Configuration**:
  1. Right-click **Host1** $\rightarrow$ select **Configure** $\rightarrow$ click **Edit network configuration**.
  2. Modify the configuration as follows and save:
     ```ini
     auto eth0
     iface eth0 inet static
         address 10.1.1.10
         netmask 255.255.255.0
         gateway 10.1.1.1
         up sysctl net.ipv4.ip_forward=0
     ```

* **Host 2 Configuration**:
  1. Right-click **Host2** $\rightarrow$ select **Configure** $\rightarrow$ click **Edit network configuration**.
  2. Modify the configuration as follows and save:
     ```ini
     auto eth0
     iface eth0 inet static
         address 10.1.1.20
         netmask 255.255.255.0
         gateway 10.1.1.1
         up sysctl net.ipv4.ip_forward=0
     ```

* **Linux Router Configuration**:
  1. Right-click **Linux-Router** $\rightarrow$ select **Configure** $\rightarrow$ click **Edit network configuration**.
  2. Configure both connected interfaces to enable IPv4 packet forwarding:
     ```ini
     auto eth0
     iface eth0 inet static
         address 10.1.1.1
         netmask 255.255.255.0
         up sysctl net.ipv4.ip_forward=1

     auto eth1
     iface eth1 inet static
         address 10.1.2.1
         netmask 255.255.255.0
         up sysctl net.ipv4.ip_forward=1
     ```

* **Host 3 Configuration**:
  1. Right-click **Host3** $\rightarrow$ select **Configure** $\rightarrow$ click **Edit network configuration**.
  2. Modify the configuration as follows and save:
     ```ini
     auto eth0
     iface eth0 inet static
         address 10.1.2.10
         netmask 255.255.255.0
         gateway 10.1.2.1
         up sysctl net.ipv4.ip_forward=0
     ```

---

### Verification & Terminal Output Analysis

1. **Start Nodes**: Click the **Green Play Button** on the top GNS3 toolbar to power on all devices.
2. **Verify IP Forwarding Status**:
   * Open the console on **Linux-Router** and check forwarding status:
     ```bash
     $ sysctl net.ipv4.ip_forward
     net.ipv4.ip_forward = 1
     ```
   * Open the console on **Host1** and check forwarding status:
     ```bash
     $ sysctl net.ipv4.ip_forward
     net.ipv4.ip_forward = 0
     ```

3. **Inspect Kernel Routing Tables (`ip route show`)**:
   * Executing on **Host1**:
     ```text
     default via 10.1.1.1 dev eth0 
     10.1.1.0/24 dev eth0 proto kernel scope link src 10.1.1.10
     ```
   * Executing on **Linux-Router**:
     ```text
     10.1.1.0/24 dev eth0 proto kernel scope link src 10.1.1.1 
     10.1.2.0/24 dev eth1 proto kernel scope link src 10.1.2.1
     ```
   * Executing on **Host3**:
     ```text
     default via 10.1.2.1 dev eth0 
     10.1.2.0/24 dev eth0 proto kernel scope link src 10.1.2.10
     ```

4. **Cross-Subnet Ping Test**:
   * On **Host1**, send an ICMP echo request to **Host3** across Subnet A and Subnet B:
     ```bash
     root@Host1:~# ping -c 4 10.1.2.10
     PING 10.1.2.10 (10.1.2.10) 56(84) bytes of data.
     64 bytes from 10.1.2.10: icmp_seq=1 ttl=63 time=1.12 ms
     64 bytes from 10.1.2.10: icmp_seq=2 ttl=63 time=0.82 ms
     64 bytes from 10.1.2.10: icmp_seq=3 ttl=63 time=0.85 ms
     64 bytes from 10.1.2.10: icmp_seq=4 ttl=63 time=0.79 ms

     --- 10.1.2.10 ping statistics ---
     4 packets transmitted, 4 received, 0% packet loss, time 3003ms
     ```
   * *Technical Note*: Standard IPv4 Linux packets initialize with a TTL of `64`. The received `ttl=63` confirms that the packet traversed exactly **1 router hop** (`Linux-Router`).

---

## Task 2: Dynamic Routing with OSPF (FRRouting)

### Step-by-Step GNS3 GUI Setup

#### 1. Importing & Launching Template
1. Go to **File** $\rightarrow$ **Import portable project**.
2. Select `OSPF-Basics-Template.gns3project` and duplicate/save it as `OSPF-Basics-<studentid>.gns3project`.
3. Click the **Green Play Button** to power on all devices.
4. **Boot Wait Time**: Allow 2–3 minutes for the FRRouting (FRR) daemons to boot. Wait until the nodes load the `frr#` or `frr:~#` shell.

#### 2. Accessing FRR Interactive CLI (`vtysh`)
1. Right-click **FRR1** $\rightarrow$ select **Console**.
2. If greeted with the standard Linux prompt (`frr:~#`), enter the VTY shell:
   ```bash
   frr:~# vtysh

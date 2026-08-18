# Network Exploration: HTTP Client GUI & CLI in GNS3



## Topology Architecture

The network consists of three distinct subnets linked by two static routers:

* **Subnet A (`192.168.1.0/24`)**: Host 1 (HTTP Client) connected via Switch 1 to Router 1.
* **Subnet B (`10.0.0.0/24`)**: Inter-router link connecting Router 1 and Router 2 via Switch 2.
* **Subnet C (`192.168.2.0/24`)**: Host 2 (HTTP Server) connected via Switch 3 to Router 2.
  

---

## Task 1: HTTP Client with GUI

### Setup & Execution
1. Configured static IP assignments on `/etc/network/interfaces` across all nodes:
   * **Host 1 (Firefox)**: `192.168.1.10/24` | Gateway: `192.168.1.1`
   * **Router 1**: `e0/0`: `192.168.1.1/24` | `e0/1`: `10.0.0.1/24`
   * **Router 2**: `e0/0`: `10.0.0.2/24` | `e0/1`: `192.168.2.1/24`
   * **Host 2 (Linux Server)**: `192.168.2.10/24` | Gateway: `192.168.2.1`
2. Configured static routing between Subnet A and Subnet C across Router 1 and Router 2.
3. Started packet capture on the Subnet B link (between Router 1 and Switch 2).
4. Connected to Host 1 via **noVNC**, launched Firefox, and navigated to `http://192.168.2.10/`.
5. Stopped packet capture and saved results.

### Artifacts
* `HTTPClient-GUI-<studentid>.gns3project`
* `HTTPClient-GUI-<studentid>-network.png`
* `HTTPClient-GUI-<studentid>-subnetB.pcap`

---

## Task 2: HTTP Client with Command Line Interface (CLI)

### Setup & Execution
1. Cloned the Task 1 project into `HTTPClient-CLI-<studentid>`.
2. Replaced the Firefox Host with a lightweight Linux Host, assigning it the same IP address (`192.168.1.10/24`).
3. Initiated packet capture on Subnet B.
4. Opened Host 1 Web Console and fetched the web server content using `wget`:
   ```bash
   wget [http://192.168.2.10/](http://192.168.2.10/)

# Week 01- Introduction to GNS3 Basics



## 1. Task Description & Learning Objectives
The primary objective of this task was to gain practical familiarity with the GNS3 network emulation environment. Key objectives included creating projects, deploying host nodes, annotating network topologies, configuring static IP interfaces prior to device boot-up via Linux configuration files, and validating interface states using Linux commands.

---

## 2. Step-by-Step Implementation & Reflective Analysis

### Step 1: Project Creation & Canvas Workspace Setup
Initialized a new GNS3 workspace named `GNS3-INTRO-12321642`.  
Setting up structured project naming conventions in GNS3 ensures proper workspace organization, making topologies easily searchable and portable across environments.

### Step 2: Node Deployment & Visual Documentation
Placed a Linux Host node on the canvas  and node metadata (Target IP address).  
Annotating network diagrams directly within the simulator is a critical habit in network engineering. Proper labeling helps document topology layouts, IP schemas, and administrative data, reducing ambiguity during visual audits.

### Step 3: Network Interface Configuration (`/etc/network/interfaces`)
Configured the static IP address for eth0 before booting up the node by editing /etc/network/interfaces.
  auto eth0
  iface eth0 inet static
     address 10.10.1.1
     netmask 255.255.255.0
     up sysctl net.ipv4.ip_forward=0
  

# Troubleshooting Cloud Network Connectivity with Traceroute, Ifconfig, Telnet, Netstat, Curl, Dig, and Tcpdump

## INTRODUCTION
Cloud network troubleshooting is the process of identifying and resolving issues that prevent servers, applications, and other cloud resources from communicating with one another. 

In cloud environments like AWS, connectivity problems frequently stem from incorrect IP addressing, DNS resolution failures, blocked ports, restrictive firewall rules, routing misconfigurations, or application errors.

As a Cloud Network Engineer, Cloud Engineer, or DevOps Engineer, mastering foundational networking diagnostic tools is critical for isolating communication failures and pinpointing root causes. 

This phase provides a comprehensive, hands-on guide to utilizing seven essential Linux networking tools (traceroute, ifconfig, telnet, netstat, curl, dig, and tcpdump) across a multi-instance AWS cloud architecture.

##### ACCESS PROJECT MATERIALS HERE
##### PDF GUIDE: [HOW TO TROUBLESHOOT CLOUD NETWORK CONNECTIVITY.pdf](https://github.com/user-attachments/files/33905348/HOW.TO.TROUBLESHOOT.CLOUD.NETWORK.CONNECTIVITY.pdf)
##### WATCH VIDEO WALKTHROUGH HERE: https://youtu.be/XF0wc0vktI4


### STEP-BY-STEP DEPLOYMENT AND TROUBLESHOOTING GUIDE

##### STEP 1: PROVISION INFRASTRUCTURE AND SECURITY GROUPS
  
1) Launch Two EC2 Instances:
   Create two Amazon Linux instances named cloud-network-troubleshooting-a and cloud-network-troubleshooting-b.
   Enable public IPv4 addresses on both instances.
   Configure and establish remote access via AWS Systems Manager (Session Manager).

2) Configure Security Group Inbound Rules:

   For cloud-network-troubleshooting-a:
   Custom TCP Port 23 (Telnet) from Anywhere (0.0.0.0/0).
   HTTPS (443) from Anywhere (0.0.0.0/0).
   HTTP (80) from Anywhere (0.0.0.0/0).

   For cloud-network-troubleshooting-b:
   Custom TCP Port 23 strictly from cloud-network-troubleshooting-a Security Group.
   HTTPS (443) from Anywhere (0.0.0.0/0).
   HTTP (80) from Anywhere (0.0.0.0/0).
   HTTPS (443) from cloud-network-troubleshooting-a Security Group.
   HTTP (80) from cloud-network-troubleshooting-a Security Group.
   SSH (22) from cloud-network-troubleshooting-a Security Group.


#### STEP 2: DIAGNOSTIC TOOL EXECUTION AND VERIFICATION.
Access your instances via Session Manager and execute the following troubleshooting commands:

1) Traceroute (Tracing Packet Routing Paths).
   Traceroute works at the network layer (IP/ICMP/UDP) using incremental Time-To-Live (TTL) values to track router hops and round-trip times (RTT) between a source and destination.

   Install and run traceroute on Instance A:
   <PRE>sudo yum install traceroute -y</PRE>
   <PRE>traceroute aws.amazon.com</PRE>

2) Ifconfig (Inspecting Network Interfaces).
   `ifconfig` (or the modern alternative `ip addr show`) displays network interface configurations, IP addresses, netmasks, MAC addresses, and packet transmission statistics.
   Check primary network interface (eth0) and loopback (lo):
   
   <PRE>ifconfig</PRE>
   <PRE>ip addr show</PRE>

3) Telnet (Testing Port Accessibility).
   Telnet (Port 23) tests raw TCP port connectivity and remote terminal access.
   Install telnet on Instance A and test connectivity to Instance B over SSH port:
   
   <PRE>sudo yum install telnet -y</PRE>
   <PRE>telnet private-ip-of-instance-b 22</PRE>

4) Netstat (Analyzing Active Connections & Listening Ports).
   `netstat` lists active TCP/UDP connections, connection states (`ESTABLISHED`, `TIME_WAIT`), and listening services bound to system ports.
   Inspect active TCP connections and listening ports:
   <PRE>sudo netstat -tp</PRE>
   <PRE>sudo netstat -tlp</PRE>
   <PRE>sudo netstat -ntlp</PRE>

5) Curl (Transferring Data and Testing HTTP/HTTPS APIs).
   `curl` interacts with web servers and APIs to validate HTTP response codes (such as `200 OK`).
   Check connection headers without downloading full HTML content:
   
   <PRE>curl -vLo /dev/null https://aws.amazon.com</PRE>

   Fetch raw web page contents:
   
   <PRE>curl https://aws.amazon.com</PRE>

6) Dig (Querying DNS Records).
   `dig` (Domain Information Groper) performs DNS lookups (A, CNAME, MX records) to debug DNS resolution paths and response times.
   Install bind-utils and query DNS domain records:
   
   <PRE>sudo yum install bind-utils -y</PRE>
   <PRE>dig aws.amazon.com</PRE>

7) Tcpdump (Capturing Low-Level Network Packets).
   `tcpdump` captures raw network packets passing through a specified network interface for security analysis and protocol debugging.
   On Instance B: Install Nginx, start the service, and capture HTTP packets on port 80:
   
   <PRE>sudo yum install tcpdump nginx -y</PRE>
   <PRE>sudo systemctl enable --now nginx</PRE>
   <PRE>sudo tcpdump -nn port 80</PRE>

   On Instance A: Send test traffic to Instance B's public IP:
   <PRE>curl -I public-ip-of-instance-b</PRE>
   

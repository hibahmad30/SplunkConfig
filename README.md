<h1>Centralized Log Management Solution with Splunk Enterprise</h1>

*This project is currently in progress*
  
<h2>Description</h2>
In this project, I configured a virtualized environment using Oracle's VirtualBox to simulate a NAT network with three distinct systems: a Windows client host, an Active Directory (AD) server, and a Splunk server. The Windows client host operates with a dynamic IP assigned via DHCP, while both the AD server and the Splunk server utilize static IP addresses to maintain consistent connectivity. To enhance the security monitoring capabilities of this setup, Sysmon (System Monitor) and the Splunk Universal Forwarder were installed on the Windows client and the AD server. Sysmon was configured to capture detailed system events, and Splunk Universal Forwarder was used to send these captured logs to the Splunk server, which acts as a central repository and analysis tool. This configuration allows for real-time log collection and monitoring, enabling efficient security event analysis within this simulated environment. Here is a high-level architecture diagram that displays the build for this project: 
<br />
<br />
<p align="center">
<img src="https://i.imgur.com/qmPh1bq.png" alt="Architecture Diagram"/>

<h2>Environments Used </h2>

- <b>Oracle VM VirtualBox</b>
- <b>Splunk Enterprise</b>
- <b>Active Directory</b>
- <b>System Monitor (Sysmon)</b>
- <b>Splunk Universal Forwarder</b>


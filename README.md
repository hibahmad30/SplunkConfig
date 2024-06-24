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

<h2>Install/Configure the Splunk Server</h2> 

<p align="center">
To begin, we will navigate to the following link and install the Ubuntu Server ISO file: https://ubuntu.com/download/server. In VirtualBox, we will create a virtual machine instance as depicted below: 
<br/>
<br/>
<img src="https://i.imgur.com/UnSJnSK.png" alt="Download Ubuntu Server" height=85% width=85%/>
 <br/>
 <br/>  
<img src="https://i.imgur.com/Ssk7CSu.png" alt="Ubuntu Server VM Instance" height=85% width=85%/>
<br/>
 <br/> 
We will then start up the VM instance and proceed with the installation of Ubuntu Server. Once a username and password have been configured, login to the server and run the following command to update system packages: 'sudo apt-get update && sudo apt-get upgrade -y'
 <br/>
 <br/>
<img src="https://i.imgur.com/xwwvyoV.png" alt="Install Ubuntu Server" height=85% width=85%/> 
<br/>
<br/>
<img src="https://i.imgur.com/mkvSwf7.png" alt="Update Packages" height=85% width=85%/> 
<br/>
<br/>
On our host machine, we will download Splunk Enterprise 9.2.1, and in our VirtualBox settings for the Ubuntu instance, add a shared folder with path of Splunk download: 
<br/> 
<br/>
<img src="https://i.imgur.com/2SJYZ8s.png" alt="Download Splunk Enterprise" height=85% width=85%/>
<br/>
<br/>
<img src="https://i.imgur.com/d7ZIe7K.png" alt="Add Shared Folder" height=70% width=70%/>
<br/>
<br/>
We will then navigate back to our Ubuntu server and create a directory called ‘share’ using ‘mkdir share’ command. Then. we will mount the directory to the Splunk download folder, and install Splunk using the following commands: 
<br/>
<br/>
<img src="https://i.imgur.com/Smrjm1o.png" alt="Mount Directory" height=85% width=85%/>
<br/>
<br/>
<img src="https://i.imgur.com/l40QkYv.png" alt="Install Splunk" height=85% width=85%/>

<h2>Install Splunk Universal Forwader and Sysmon</h2> 
<p align="center">
We will now install Splunk Universal Forwarder and Sysmon on both our Windows 10 client machine and our Active Directory server. Prior to this step, ensure that you have two VM instances configured for both Windows 10 and Windows Server 2019. Also, configure a static IP address for both machines to prevent IP conflicts, and confirm the configuration using the 'ipconfig' commmand. 
<br/>
<br/>
<img src="https://i.imgur.com/iBArEzk.png" alt="IPv4 Configuration" height=50% width=50%/>
<br/>
<br/>
<img src="https://i.imgur.com/t0CmsPG.png" alt="Confirm Configuration" height=70% width=70%/>
<br/>
<br/>
In the Windows client machine, navigate to your preferred web browser and type the IP address of the Splunk server with ':8000' appeneded - this is added because Splunk listens on port 8000. Proceed by logging into Splunk in another tab, and download Splunk Universal Forwarder which can be found by navigating <a href="https://www.splunk.com/en_us/download/universal-forwarder.html?utm_campaign=google_amer_en_search_brand&utm_source=google&utm_medium=cpc&utm_content=Uni_Forwarder_Demo&utm_term=splunk%20universal%20forwarder&device=c&_bt=471686934615&_bm=p&_bn=g&gad_source=1&gclid=Cj0KCQjwsuSzBhCLARIsAIcdLm4oJ2ShpMfo74w6W0IWSqvvsrLsYjWbWxiPR90PO0CMyaDZLkiOMV4aAqQcEALw_wcB">here</a>. While that is installing, navigate to the following link to download Sysmon on the Windows 10 client machine: https://learn.microsoft.com/en-us/sysinternals/downloads/sysmon. 
<br/>
<br/>
<img src="https://i.imgur.com/18cNhvX.png" alt="Splunk Login Page" height=70% width=70%/>
<br/>
<br/>
<img src="https://i.imgur.com/IUTnBFr.png" alt="Splunk Universal Forwarder Setup" height=70% width=70%/>
<br/>
<br/>
<img src="https://i.imgur.com/MIvnOGN.png" alt="Install Sysmon" height=70% width=70%/>
<br/>
<br/>
Once both Splunk Universal Forwarder and Sysmon have been installed, navigate to the following path in the system's 'C://' drive:  'Program Files > SplunkUniversalForwarder > etc > system > default'. The 'inputs.conf' file is located in this folder. This file must also be duplicated in the following path: 'Program Files > SplunkUniversalForwarder > etc > system > local'. To do this, run 'Notepad' as an administrator and create the 'inputs.conf' file with the following content, ensuring to save the file to the 'local' directory: 
 <br/>
 <br/>
 <img src="https://i.imgur.com/cHem22Y.png" alt="Inputs File"/>
  <br/>
  <br/>
Be sure to restart the 'SplunkForwarder' service in Windows Services once the file has been added to the 'local' directory. Now that Splunk Universal Forwarder and Sysmon have been downloaded on the Windows 10 client machine, repeat the same process in the Windows Server 2019 VM. 

<h2>Test Splunk Forwarding Configuration</h2> 
 <p align="center">
Once  Splunk Universal Forwarder and Sysmon have been installed on both machines, login to Splunk Enterprise on both the Windows 10 client and Windows Server machines, using the IP address of the Splunk server and port 8000. Once logged in, navigate to: 'Settings > Indexes > New Index' and create a new index called 'endpoint'. Then,  we will setup the Splunk server to receive data by navigating to: 'Settings > Forwarding and receiving > Configure receiving > New receiving port', and type the port ‘9997’. 
 <br/>
 <br/>
 <img src="https://i.imgur.com/10PDKcY.png" alt="Splunk Enterprise Login"/>
  <br/>
  <br/>
 <img src="https://i.imgur.com/scZaQ8n.png" alt="Configure Receiving Port"/>
 <br/>
 <br/>
Once this is configured, use the search ‘index=endpoint’ in Splunk to test that it is working correctly. There should be two hosts: one for the client machine, and one for the Windows Server machine: 
<br/>
 <br/>
 <img src="https://i.imgur.com/c75ddnz.png" alt="Splunk Confirmation"/>

<h2>Install Active Directory:</h2>
*In progess*
<h2>Key takeaways:</h2>
*In progess*
<p align="center">
<!--
 ```diff
- text in red
+ text in green
! text in orange
# text in gray
@@ text in purple (and bold)@@
```
--!>


<h1>Centralized Log Management Solution with Splunk Enterprise</h1>

*This project is currently in progress (95% complete)*
  
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
<img src="https://i.imgur.com/7nfTqFp.png" alt="IPv4 Configuration" height=50% width=50%/>
<br/>
<br/>
<img src="https://i.imgur.com/t0CmsPG.png" alt="Confirm Configuration" height=70% width=70%/>
<br/>
<br/>
Once all three machines have been installed and configured, create a NAT network in VirtualBox, and add all three machines to this NAT network: 
<br/>
<br/>
<img src="https://i.imgur.com/ZkEOUDu.png" alt="Create NAT Network" height=70% width=70%/>
<br/>
<br/>
<img src="https://i.imgur.com/xcUBjLW.png" alt="Add Machines to Network" height=70% width=70%/>
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
 <p align="center">
We will now install Active Directory on our Windows Server 2019 machine and promote it to Domain Controller. In Server Manager, navigate to 'Manager > Add Roles and Features'.  Add the ‘Active Directory Domain Services’ role, and proceed with the installation. 
  <br/>
  <br/>
 <img src="https://i.imgur.com/vPUC0WW.png" alt="Add Active Directory Domain Services Role"/>
 <br/>
 <br/>
Once installed, navigate back to Server Manager and click the flag icon in the top right corner. This will reveal the option to promote the server to a domain controller. We will then add a new forest and create a domain name: 
  <br/>
  <br/>
 <img src="https://i.imgur.com/fWfCI8Z.png" alt="Promote Server to Domain Controller"/>
 <br/>
 <br/>
 <img src="https://i.imgur.com/5a6bXVd.png" alt="Create Domain Name"/>
 <br/>
 <br/>
Once installed, the machine will automatically restart. In Server Manager, navigate to ‘Tools > Active Directory Users and Computers’. For practice, we will create a new organizational unit and name it ‘IT’, as well as some users: 
 <br/>
 <br/>
 <img src="https://i.imgur.com/zaEJktE.png" alt="Create OUs and Users"/>
 <br/>
 <br/>
We will now login to our Active Directory server from our Windows 10 client machine as the user ‘Mary Kristel.’ In the Windows 10 machine, navigate to ‘System Properties > Computer Name > Change’. Select ‘Domain’ and enter the domain name of the Active Directory server. The system will restart, and we can now login using the credentials of the user 'Mary Kristel.' 
 <br/>
 <br/>
 <img src="https://i.imgur.com/gKiLb6E.png" alt="Change Computer Name"/>
 <br/>
 <br/>
We have successfully authenticated! 
 <br/>
 <br/>
 <img src="https://i.imgur.com/915OIX6.png" alt="User Login"/>

<h2>Key takeaways:</h2>
In this project, I built a virtualized network using Oracle’s VirtualBox to simulate a realistic IT environment. The network consisted of three primary components: a Windows client host, an Active Directory (AD) server, and a Splunk server. The setup was designed to mirror a typical small to medium-sized enterprise environment. The Windows client received its IP address dynamically via DHCP, whereas the AD and Splunk servers were assigned static IPs to ensure consistent communication across the network. This architecture enabled the implementation of robust security monitoring and log management through the use of Sysmon (System Monitor) and the Splunk Universal Forwarder.
 <br/>
 <br/>
The goal of this project was to create a controlled environment where security events could be monitored and analyzed effectively. By simulating a NAT network with dedicated roles for each system, the project provided a practical setting for understanding how enterprise networks function and how security tools like Sysmon and Splunk can be used to enhance visibility into system activities. The AD server represented a central authority for user authentication and policy enforcement, which is crucial in managing and securing network resources. The Splunk server, serving as a centralized log repository and analysis platform, demonstrated how security events can be aggregated, analyzed, and acted upon in real-time.
 <br/>
 <br/>
To build this environment, I configured Oracle VM VirtualBox to create three virtual machines: one for the Windows client, one for the AD server, and one for the Splunk server. Static IP addresses were assigned to the AD and Splunk servers to maintain their connectivity within the network, while the Windows client received a dynamic IP address. Sysmon was installed on both the Windows client and the AD server to capture detailed system activity logs, such as process creations, network connections, and file modifications. These logs were then forwarded to the Splunk server using the Splunk Universal Forwarder, a lightweight agent designed for log collection and transmission. The Splunk server aggregated these logs and provided tools for real-time monitoring and analysis, allowing for the detection of potential security threats and anomalies within the network.
 <br/>
 <br/>
By installing and configuring this build, I gained practical experience in setting up and managing virtualized environments, configuring network settings, and deploying security monitoring tools. This project enhanced my understanding of how enterprise networks operate and how critical security tools can be integrated to improve network visibility and incident response capabilities.
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


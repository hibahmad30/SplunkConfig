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
In the ‘Basic SAML Configuration’ section, enter placeholder URLs for ‘Identifier’ and ‘Reply URL’, and proceed to save the configuration. Select 'Download for Certificate (Base64)' in the SAML Signing Certificate area to download the certificate to your computer, which will be required for the next steps. 

<h2>Mapping Microsoft Entra ID Attributes to Okta Attributes</h2> 
 <p align="center">
SAML claims are pieces of information about a user that are shared between different systems to help with logging in and accessing services, such as their name or email address. We will be editing our attributes and claims, which is essentially what information we want to send to Okta for authentication. 
 <br/>
 <br/>
 <img src="https://i.imgur.com/Pzmbstz.png" alt="Attributes and Claims"/>
  <br/>
  <br/>
In addition to the default claims, we will add 'company name' and 'telephone number': 
<br/>
 <br/>
 <img src="https://i.imgur.com/c3QstfC.png" alt="Additional Claims"/>
 <br/>
 <br/>
Return back to Okta, and navigate to SAML Certifications > Edit > New Certificate. Enter the following values to update the placeholders: the 'IdP Issuer URI' is the 'Microsoft Entra Identifier', the 'IdP Single Sign-On URL' is the 'Login URL', and the 'IdP Signature Certificate' is the 'Base64 download'. 
<br/>
 <br/>
 <img src="https://i.imgur.com/DR3tYq1.png" alt="SAML Certificate"/>
 <br/>
 <br/>
 <img src="https://i.imgur.com/peyTXNw.png" alt="SAML Values"/>
  <br/>
 <br/>
 Be sure to make note of the values above. The 'Assertion Consumer Service URL' will be the 'Reply URL in Azure', and the 'Audience URI' will be the 'Identity Entity ID' in Azure.  
<br/>
<br/>
In Entra ID, update the placeholder values: 
<br/>
<br/>
<img src="https://i.imgur.com/lDoz3p7.png" alt="SAML Configuration Update"/>
 <br/>
 <br/>
Back in Okta, navigate to: Edit profile and mappings > Mappings. We will then unmap all attributes except 'username'. 
<br/>
<br/>
<img src="https://i.imgur.com/trmCk9u.png" alt="Unmap Attributes"/>
 <br/>
 <br/>
We will then navigate to 'Custom', and proceed to delete and recreate the attributes. For the attribute mapping, I will be referencing the following Okta documentation: https://help.okta.com/en-us/content/topics/provisioning/azure/azure-map-attributes.htm. Once the attributes are created, we will proceed with updating the mappings: 
 <br/>
 <br/>
<img src="https://i.imgur.com/nzHv3uy.png" alt="Create Mappings"/>
<br />
<br />
 <img src="https://i.imgur.com/r36IKV9.png" alt="Mapping Attributes"/>
 <br />
<br />
We will test the authentication by first navigating to our Okta application in Entra ID and assigning users to the application: Manage > users and groups > Add user/group 
<br />
<br />
<img src="https://i.imgur.com/519WGJ6.png" alt="Assign Users"/>
<br />
<br />
We are now ready to test the authentication! Navigate to Single sign-on > Test. 
<br />
<br />
<img src="https://i.imgur.com/XC7Yhjs.png" alt="Test the SSO"/>
<br />
<br />
<img src="https://i.imgur.com/ytzqWAk.png" alt="SSO Login"/>
<br />
<br />
For advanced testing and troubleshooting, you may optionally download the 'My Apps Secure Sign-in Extension' Chrome extension. As demonstrated below, we successfully authenticated!
<br />
<br />
<img src="https://i.imgur.com/fIbnlYn.png" alt="Successful SSO"/>
<h2>Key takeaways:</h2>
This project focuses on integrating federation between Microsoft Entra ID and Okta to create a seamless and secure authentication experience across hybrid environments. Federation in this context means connecting these systems so that users can log in once and gain seamless access to resources managed by either platform. This integration simplifies the authentication process, enhances security, and improves user experience across hybrid IT environments. The process begins with the essential step of setting up a "break-glass" account in Entra ID, an emergency backup to ensure continuous access even if primary authentication methods fail. Entra ID is then configured to act as the primary identity provider for Okta by utilizing the SAML 2.0 protocol. 
<br/>
<br/>
This involves establishing a secure communication link between Azure and Okta through the creation of an Okta enterprise application in Entra ID. The integration involves the setup of SAML claims, which define and map user attributes such as name, email, company name, and telephone number. These claims are fundamental in identifying and authenticating users across both systems. The final step in the implementation is thorough testing and validation to confirm that users can successfully log in to Okta applications using their Entra ID credentials.
<br/>
<br/>
Implementing federation between Azure and Okta offers significant benefits such as enhancing user experience by enabling Single Sign-On (SSO), allowing users to log in once and access multiple systems without repeated authentication. This streamlines the process and reduces the frustration of managing multiple passwords. From an administrative perspective, federation simplifies centralized management of user identities and access rights, reducing the overhead for IT teams. 
<br/>
<br/>
Federation is particularly valuable in several use cases such as for organizations operating in hybrid environments, where a mix of on-premises and cloud-based resources needs unified access management. It also facilitates secure cross-organizational collaboration, allowing external partners to access shared resources using their own identity systems. As companies expand their IT infrastructure, federation supports scalable identity solutions by seamlessly integrating multiple identity providers. 
<br/>
<br/>   
Additionally, it aids in regulatory compliance by offering a centralized, auditable log of user access across all systems. Key terms like federation, SAML, identity provider, attributes and claims, and break-glass accounts play crucial roles in understanding this project. By combining the strengths of Microsoft Entra ID and Okta through federation, this project creates a unified and efficient identity management solution that simplifies and secures user access to diverse systems.
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


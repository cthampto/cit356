<center>

# CIT35600 | Network Operating System Administration</center> 
<center>

## LAB | Web Services</center>

---

## NIST NICE Framework Mapping

The below items are mapped to the NIST NICE Framework for this lab. The NIST NICE Framework maps Knowledge, Skills, and Abilities (KSA) for a cybersecurity job function.

## Knowledge:
- **K0004**: Knowledge of operating systems.
- **K0018**: Knowledge of system administration concepts for operating systems such as Linux and Windows.
- **K0318**: Knowledge of operating system command-line tools.
- K0349 : Knowledge of website types, administration, functions, and content management system (CMS).
- **K0471**: Knowledge of Internet network addressing (IP addresses, classless inter-domain routing, TCP/UDP port numbering).
## Skills:
- **S0002**: Skill in using command-line tools.
- **S0158**: Skill in operating system administration. (e.g., account maintenance, data backups, maintain system performance, install and configure new hardware/software).
## Abilities:
- **A0001**: Ability to administer network operating systems.
- **A0055**: Ability to operate common network tools (e.g., ping, traceroute, nslookup).
- **A0058**: Ability to execute OS command line (e.g., ipconfig, netstat, dir, nbtstat).




---
<div style="page-break-after: always;"></div>
<h2 style="width: 100%; background-color: black; border-bottom: 4px solid #dc8823; margin-bottom: 0px;" ><span style="color: #ffffff;">Purpose</span></h2>

The purpose of this lab is to provide students with hands-on experience in implementing and managing web services on both Windows Server and Linux operating systems. By completing this lab, students will gain practical skills in configuring, deploying, and troubleshooting essential web services such as HTTP, HTTPS, and DNS. These skills are critical for network administration and management, ensuring students are well-prepared for real-world scenarios.

<br/>
<div style="page-break-after: always;"></div>
<h2 style="width: 100%; background-color: black; border-bottom: 4px solid #dc8823; margin-bottom: 0px;" ><span style="color: #ffffff;">Overview</span></h2>

In this lab, students will work with both Windows Server and Ubuntu virtual machines to set up and manage web services. The lab is divided into several tasks, each focusing on different aspects of web service management:

1. **Initial Setup**: Students will prepare their virtual machines by installing necessary packages and tools.
2. **Intro to Web Servers**: Students will learn about HTTP ports, status codes, and response types, and will set up a simple web server.
3. **Windows IIS**: Students will install and configure Internet Information Services (IIS) on a Windows Server, deploy websites, implement SSL/TLS, and manage multiple sites and application pools.
4. **Linux Apache**: Students will install and configure Apache on Ubuntu, create virtual hosts, implement HTTPS, and troubleshoot common issues.
5. **Linux Nginx**: Students will install and configure Nginx on Ubuntu, create virtual hosts, implement HTTPS, and troubleshoot common issues.

Each task includes detailed instructions and activities designed to reinforce the concepts and skills being taught. Students are expected to document their process and submit their work according to the lab submission guidelines.

<br/>

**Lab Submission Guidelines**: 
Submissions must be in Microsoft Word (.docx) (e.g., **studentNameCIT35600_LABXX.docx**. ) or PDF format (e.g., **studentNameCIT35600_LABXX.pdf** ). Be sure to clearly label the question being answered. As well as where possible, screenshots should be embedded directly in the document. Screenshots should also be cropped to only include the necessary content to answer the question with text easily readable.

Your image must show your work and must be specific. Don’t take a screenshot of the whole Desktop or terminal. Just show the commands which have been used and its output. Try to avoid showing the results from previous or next steps.

<br/>

<h2 style="width: 100%; background-color: black; border-bottom: 4px solid #dc8823; margin-bottom: 0px;" ><span style="color: #ffffff;">PreReq</span></h2>

Virtualization Platform (VirtualBox, VMware Workstation/Fusion, Hyper-V)
Ensure the following virtual machines are setup:
- Ubuntu 22.04
- Windows Server 2019

Before starting the lab, it may be beneficial to take a snapshot of the VMs in their powered off state.
<br/>

<div style="page-break-after: always;"></div>
<h2 style="width: 100%; background-color: black; border-bottom: 4px solid #dc8823; margin-bottom: 0px;" ><span style="color: #ffffff;">Tasks</span></h2>


----
# Initial setup
Linux packages and tools
- curl
- net-tools
sudo apt install net-tools curl

**Update DNS**
Linux (/etc/hosts): 
- apache.local
- Avhost1.local
- Avhost2.local
- example.local
Windows (local or DNS server if installed):
- mylocaldomain.local
- test.cit356.local

**Objective:** To provide students with practical experience in setting up and managing 

# Intro To Web Servers

## **HTTP Ports**
Web servers can be configured to listen on various ports, depending on the requirements and network setup. Here are some common alternative ports:

HTTP (Default Port: 80)
- **8080:** Often used as an alternative to port 80, especially for development and testing environments.
- **8000:** Another common alternative, sometimes used for proxy servers or web applications.
- **81, 82:** Less common but can be used for specific applications or services.
HTTPS (Default Port: 443)
- **8443:** Frequently used as an alternative to port 443, especially in development or for applications like Tomcat.
- **4443:** Another alternative port for HTTPS, used in some specific scenarios.
- **444, 4433:** Less common but can be configured for secure web traffic.

## **Understanding HTTP Status Codes & Response Types**
**Introduction to HTTP Status Codes:**
HTTP status codes are standardized codes that a web server uses to communicate the result of a client's request. These codes are part of the HTTP protocol and are sent in the response from the server to indicate whether the request was successful, encountered an error, or requires further action. Each status code consists of three digits and falls into one of five categories (1xx, 2xx, 3xx, 4xx, 5xx):
- Informational Response (1XX)
- Successful Response (2XX)
- Redirection Messages (3XX)
- Client Error Responses (4XX)
- Server Error Responses (5XX)

Examples of common status codes (e.g., 200 OK, 404 Not Found, 500 Internal Server Error).


**Introduction to HTTP Response Types:**
An HTTP response type refers to the format of the data that a web server sends back to a client (like a web browser) after receiving an HTTP request. The response type is indicated by the `Content-Type` header in the HTTP response.
- Different types of HTTP responses (e.g., HTML, JSON, XML, plain text).


**Activity: Simulating HTTP Status Codes & Response Types:**    
- Set up a simple web server (e.g., using Python’s `http.server` module)
- Save the below Python script to a file called **simpleHttpActivity.py** and run it in a terminal with: <code class="language-python">python3 simpleHttpActivity.py</code>


<pre><code class="language-python">
from http.server import BaseHTTPRequestHandler, HTTPServer

class SimpleHTTPRequestHandler(BaseHTTPRequestHandler):
    def do_GET(self):
        if self.path == '/':
            self.send_response(200)
            self.end_headers()
            self.wfile.write(b'Hello, world!')
        elif self.path == '/html':
            self.send_response(200)
            self.send_header('Content-Type', 'text/html')
            self.end_headers()
            self.wfile.write(b'<html><body>Hello, world!</body></html>')
        elif self.path == '/json':
            self.send_response(200)
            self.send_header('Content-Type', 'application/json')
            self.end_headers()
            self.wfile.write(b'{"message": "Hello, world!"}')
        elif self.path == '/xml':
            self.send_response(200)
            self.send_header('Content-Type', 'application/xml')
            self.end_headers()
            self.wfile.write(b'<message>Hello, world!</message>')
        elif self.path == '/text':
            self.send_response(200)
            self.send_header('Content-Type', 'text/plain')
            self.end_headers()
            self.wfile.write(b'Hello, world!')
        elif self.path == '/notfound':
            self.send_response(404)
            self.end_headers()
            self.wfile.write(b'Not Found')
        elif self.path == '/error':
            self.send_response(500)
            self.end_headers()
            self.wfile.write(b'Internal Server Error')

    def do_HEAD(self):
        if self.path in ['/', '/html', '/json', '/xml', '/text', '/notfound', '/error']:
            if self.path == '/':
                self.send_response(200)
            elif self.path == '/html':
                self.send_response(200)
                self.send_header('Content-Type', 'text/html')
            elif self.path == '/json':
                self.send_response(200)
                self.send_header('Content-Type', 'application/json')
            elif self.path == '/xml':
                self.send_response(200)
                self.send_header('Content-Type', 'application/xml')
            elif self.path == '/text':
                self.send_response(200)
                self.send_header('Content-Type', 'text/plain')
            elif self.path == '/notfound':
                self.send_response(404)
            elif self.path == '/error':
                self.send_response(500)
            self.end_headers()

httpd = HTTPServer(('localhost', 8000), SimpleHTTPRequestHandler)
httpd.serve_forever()
</code></pre>

Open a browser and navigate to different paths or use the curl command in a terminal. You should be able to observe the status codes returned and content types.

Using the browser Inspect Element to view the sites information:
![[CIT35600-Lab-BrowserInspectElement.png]]

Expected output of the response type of an html file with the Content-Type as text/html:
<pre>
user@ubuntu:~$ curl -I http://localhost:8000/html
HTTP/1.0 200 OK
Server: BaseHTTP/0.6 Python/3.10.12
Content-Type: text/html
</pre>
Expected output of a 404 Status Code:
<pre>
user@ubuntu:~$ curl -I http://localhost:8000/notfound
HTTP/1.0 404 Not Found
Server: BaseHTTP/0.6 Python/3.10.12
</pre>




# Windows IIS
This is performed on a Windows Server running IIS version 10

After completing this lab, students will be able to:
- Install and configure Internet Information Services (IIS) on a Windows Server.
- Deploy and manage websites using IIS.
- Implement security measures such as SSL/TLS for websites hosted on IIS.
- Deploy and manage FTP and WebDav?
- Set up and manage virtual directories and websites.
- Monitor and troubleshoot IIS using built-in tools and logs.



## 00 Getting Started with IIS
This section introduces students to the basics of Internet Information Services (IIS), including installation, understanding the web root directory, configuring web logs, and exploring the default site. These foundational skills are essential for managing and deploying web services on a Windows Server.

### Installation
Open Server Manager.
Navigate to Add roles and features.
Select Role-based or feature-based installation.
Choose the appropriate server.
Select Web Server (IIS) and complete the installation process.
Verify the installation by accessing the default IIS welcome page.
![[CIT35600-Lab-IIS.png]]
### Web Root Directory
Open IIS Manager.
Navigate to Sites and select Default Web Site.
Note the Physical Path (usually C:\inetpub\wwwroot).
You can place your web content in this directory to serve it via IIS.
![[CIT35600-Lab-IISRoot.png]]
### Web Logs
In IIS Manager, select your site.
Click on Logging in the Features View.
Configure log file settings (format, directory, rollover options).
Access logs in the specified directory (default is C:\inetpub\logs\LogFiles).

### The Default Site
Open IIS Manager.
Select Default Web Site.
Explore the default settings and configurations.
Test the default site by navigating to http://localhost in a web browser. You should see a webpage similar to the one below.
![[CIT35600-Lab-IISdefaultPage.png]]

In IIS Manager, in the list of sites on the left pane, click **Default Web Site**. In the Actions panel under Manage Website, select the Stop option to turn off this website.
## 01 Deploying a Website
This section guides students through the process of deploying a new website on IIS. Students will learn how to create and configure a website, set up site bindings, and deploy web content.

Within IIS Manager, go to Sites in the Connection panel. You should see in the Actions panel on the far right an option to **Add Website...**

You will then be able to provide details regarding the website you want to create. Giving the Site name, Physical path, and specifying the binding for the website. We will choose the following details:
- Site Name: MyFirstSite
- Physical Path: C:\inetpub\wwwroot\MyFirstSite
	- You will need to create this folder in the wwwroot directory
- Binding: http All Unassigned on port 80
- Start Website immediately: checked

You may get a warning regarding the binding to port 80 is already assigned to another site. You can select Yes.

Before you browse to the URL, you will need to create a default document for the webserver to serve. The below are the default documents in order of priority:
- Default.htm
- Default.asp
- index.htm
- index.html
- iisstart.htm

Once you create the default document in the physical path for the website, you should be able to go to a web browser and navigate to http://localhost


## 02 Implement SSL/TLS | Securing with HTTPs
This section focuses on securing web services with SSL/TLS. Students will learn how to obtain and install SSL certificates, configure HTTPS bindings, and enforce secure connections.

The Certificate Store
When creating SSL/TLS self-signed certificates in IIS, you have the option to store the certificate in either the **Personal** or **Web Hosting** certificate store.
- Personal Certificate Store
	- **Purpose:** The Personal store is typically used for certificates that are intended for individual use or for a smaller number of certificates.
	- **Usage:** This store is suitable for scenarios where you have fewer than 20-30 certificates. It is the default choice and is often used for general purposes, including personal or development environments.
- Web Hosting Certificate Store
	- **Purpose:** The Web Hosting store is designed to handle a larger number of certificates efficiently, making it ideal for web servers that host multiple sites or services.
	- **Usage:** This store is recommended when you have 30 or more certificates. It is optimized for performance and scalability, ensuring better management and faster access to a large number of certificates.

### Using IIS Manager to make a self signed for the system
Open IIS Manager and on the left pane with the connections, select your server. Then in the middle pane under the IIS section, select Server Certificates.
![[CIT35600-Lab-IISserverCerts.png]]

Under Actions on the far right pane, choose **Create Self-Signed Certificate...**
![[CIT35600-Lab-IIS-createSelfSign.png]]

The Create Self-Signed wizard will start and you can choose **localhost** for now for the friendly name. You will also notice the options on where to store the certificate, The **Personal** option is good for now.
![[CIT35600-Lab-IIS-SelfSignFriendly.png]]

Once you push **OK** the certificate will be generated and should see if in the Server Certificates. You will notice it Issues this certificate to the local system by it's name in the Issued To column.

### Using PowerShell and `New-SelfSignedCertificate` cmdlet
Because we using self-signed the certificate creation in IIS Manager is a bit limiting for creating specific certificates to a certain domain. We will use PowerShell to be able to have the ability to specify a DNS name. 

The below example generates a self-signed certificate to act as a Certificate Authority (CA) with specific key usages like certificate signing, CRL signing, and digital signature. As well as creates a new certificate for a website, signed by the CA certificate, ensuring the website certificate is trusted.
```powershell
#Create the CA certificate  
$rootcert = New-SelfSignedCertificate `
-DnsName "CIT356 Local CA" `
-CertStoreLocation cert:\LocalMachine\My `
-KeyUsage CertSign, CRLSign, DigitalSignature `
-FriendlyName "CIT356 Local CA" `
-TextExtension @("2.5.29.19={text}CA=true")
  
#Output the thumbprint of the Root Cert  
$thumbPrint = $($rootcert.Thumbprint)  
Write-host "Certificate Thumbprint: $($rootcert.Thumbprint)"
  
#Export the Cert to the Users Desktop folder so can be added to Trusted Root
Export-Certificate -Cert $rootcert -FilePath $env:USERPROFILE\Desktop\CIT356-CA.cer
  
#Import CA to Trusted Root Certificates
Import-Certificate -CertStoreLocation Cert:\LocalMachine\Root -FilePath $env:USERPROFILE\Desktop\CIT356-CA.cer

# Get the root CA certificate from the Trusted Root store using its thumbprint
$rootca = Get-ChildItem cert:\LocalMachine\Root | Where-Object {$_.Thumbprint -eq $thumbPrint}

#Generate Cert for website signed by CA
New-SelfSignedCertificate `
-CertStoreLocation cert:\LocalMachine\My `
-DnsName "test.cit356.local" `
-Signer $rootca `
-KeyUsage DigitalSignature, KeyEncipherment `
-FriendlyName "test.cit356.local"
```

The below example generates a self signed certificate and puts it directly in the Trusted Root store as a workaround for the system to consider it trusted.
```powershell
# Create a self-signed cert and store it in the Personal store
$cert = New-SelfSignedCertificate `
-DnsName "mylocaldomain.local", "*.mylocaldomain.local" `
-CertStoreLocation "cert:\LocalMachine\My" `
-FriendlyName "MyLocalCert"

# Write out the thumbprint of the created certificate
Write-host "Certificate Thumbprint: $($cert.Thumbprint)"

# Export the certificate to the userprofile Desktop
Export-Certificate -Cert $cert -FilePath $env:USERPROFILE\Desktop\$($cert.FriendlyName).cer

# Add the certificate to the Trusted Root Certification Authorities store
$store = New-Object System.Security.Cryptography.X509Certificates.X509Store("Root", "LocalMachine")
$store.Open("ReadWrite")
$store.Add($cert)
$store.Close()
```

You can go back into IIS Manager and select the site and bind port https and select the SSL certificate. You will notice that there is a hostname and Require Server Name Indication. These can be useful when dealing virtual hosting multiple sites.

## 03 MultiSite
This section teaches students how to host multiple websites on a single IIS server. Students will learn to configure site bindings with unique hostnames or ports and manage multiple sites.

You will need to ensure your system can resolve names for the sites once you setup the Sites to listen for a Host Name. This can be done via the DNS manager or editing the local hosts file on Windows. 

Open IIS Manger and create a new site called **mylocaldomain.local** and ensure the hostname is also set to **mylocaldomain.local**.

Add a Site Binding for HTTPS and ensure you have the Host name specified and the Require Server Name Indication checked. You will also need to select the SSL Certificate that was generated from the PowerShell New-SelfSignedCertificate cmdlet.

The bindings should look like the below image.
![[CIT35600-Lab-IISmylocaldomainBindings.png]]

Navigate to the name in a web browser on both HTTP and HTTPS. 

Now create another website with a different hostname that your system can resolve. You should be able to then go to both websites by name and get different webpages. 

## 04 Application Pools
Application Pools in IIS are used to isolate web applications for better performance, security, and reliability. Each application pool runs independently, ensuring that issues in one application do not affect others. This isolation also allows for different configurations and resource allocations for each application, enhancing overall server stability.

If you wish to create a separate application pool, you can do this in the IIS Manager. Select Application Pools in the Connections section and the the Add Application Pool...

You can then give it a name and change settings such as .NET CLR version or Managed pipeline mode. 
![[CIT35600-Lab-IISnewAppPool.png]]

Once it's created you should see the new Application Pool in the list of Application Pools.

## 05 Virtual Directories
Virtual Directories in IIS allow administrators to create directories that map to physical directories on the server or on remote servers. This feature enables the organization and management of web content without altering the physical directory structure. Virtual Directories are useful for sharing content across multiple sites or applications and for organizing large amounts of data.

Within IIS Manager, go to one of your sites that are active. You will see an option in the Actions pane on the right called **View Virtual Directories**. Select this option and then choose Add Virtual Directory...

You will give the Alias and the Physical path. In this example we are using the alias files and the path C:\inetpub\wwwroot\files
![[CIT35600-Lab-IISvirtDirs.png]]

You will need to create a index.html file within the folder with content. Once you do you should be able to go to the URL/files to view the content. In this example it would be http://mylocaldomain.local/files



## 06 Rewrite Module
Now we will setup and implement a URL rewrite module to redirect HTTP to HTTPS in IIS. Before doing this step, you should make sure you have a SSL certificate installed for your site. This is because once implemented, when a client visits your site with the http:// protocol it will be redirected to use https:// instead.

Download and install the URL Rewrite module
https://www.iis.net/downloads/microsoft/url-rewrite

Relaunch the IIS Manager

Navigate to your site and in the middle pane, you should see a URL Rewrite option in the IIS section.

Add a new inbound blank rule

Input a rule name

**Match URL** options:
Request URL: Matches the Pattern
Using: Regular Expressions
Pattern: (.\*)
Ignore case: checked
![[CIT35600-Lab-IISrewriteMatch.png]]
**Conditions:**
Add and use the following options:
Condition input: {HTTPS}
Check if input string: Matches the Pattern
Pattern: ^OFF$
![[CIT35600-Lab-IISrewriteCondition.png]]
**Action:**
Action Type: Redirect
Redirect URL: https://{HTTP_HOST}{REQUEST_URI}
Append query string: unchecked
Redirect type: Permanent (301)
![[CIT35600-Lab-IISrewriteAction.png]]

Once done apply the rule, restart the website and navigate to the site with HTTP. It should redirect you to the HTTPS version. 

## 07 PowerShell
In this section, we will explore how to use PowerShell cmdlets to manage Internet Information Services (IIS). PowerShell provides a powerful and flexible way to automate and streamline IIS management tasks, making it easier to configure, monitor, and maintain your web server environment.
https://learn.microsoft.com/en-us/powershell/module/iisadministration/?view=windowsserver2025-ps
https://learn.microsoft.com/en-us/powershell/module/webadministration/?view=windowsserver2025-ps

There are a handful of cmdlets that can be used to manage your IIS sites. When it comes to administering IIS with PowerShell, you have two primary modules to choose from: **IISAdministration** and **WebAdministration**. Below we can see how many cmdlets are associated with each module. 
<pre>
PS C:\inetpub> (Get-Command -module webadministration  ).count
79
PS C:\inetpub> (Get-Command -module iisadministration  ).count
34
PS C:\inetpub>
</pre>
For modern environments running IIS 10.0 or later, the **IISAdministration** module is generally the better choice due to its efficiency, improved pipeline support, and simplified cmdlets.

### Managing Websites
Create a website
<pre><code class="language-powershell">
New-Item -Path "C:\inetpub\wwwroot\MyNewSite" -ItemType "dir"
New-IISSite -Name "MyNewSite" -PhysicalPath "C:\inetpub\wwwroot\MyNewSite" -BindingInformation "*:80:mynewsite.com"
</code></pre>

Retrieve Website Information:
<pre><code class="language-powershell">
Get-IISSite -Name "MyNewSite"
</code></pre>

Starting a website
<pre><code class="language-powershell">
Start-IISSite -Name "MyNewSite"
</code></pre>

Stopping a website
<pre><code class="language-powershell">
Stop-IISSite -Name "MyNewSite"
</code></pre>

Remove a Website:
<pre><code class="language-powershell">
Remove-IISsite -Name "MyWebsite"
</code></pre>

### Managing Application Pools
**Creating a New Application Pool:**
<pre><code class="language-powershell">
#Create a new AppPool
$appPoolName = "testPSpool"
$AppPoolsSection = Get-IISConfigSection -SectionPath "system.applicationHost/applicationPools"
$AppPoolsCollection = Get-IISConfigCollection -ConfigElement $AppPoolsSection
New-IISConfigCollectionElement -ConfigCollection $AppPoolsCollection -ConfigAttribute @{"name"=$appPoolName}
</code></pre>

**Retrieving Application Pool Information:**
<pre><code class="language-powershell">
Get-IISAppPool -Name "MyAppPool"
</code></pre>

### Managing Configurations?



Add HTTPS Binding
<pre><code class="language-powershell">
New-WebBinding -Name "MyNewSite" -Protocol "https" -Port 443 -IPAddress "*" -HostHeader "www.mynewsite.com"
</code></pre>


Removing HTTPS binding
<pre><code class="language-powershell">
# Remove the HTTPS binding from the specified site
# With -Confirm:$false: Suppresses the confirmation prompt
$siteName = "MyNewSite"
$hostName = "MyNewSite"
$port = "443"
$bindingInformation = "*:" + $port + ":" + $hostName
Remove-IISSiteBinding -Name $siteName -BindingInformation $bindingInformation -Protocol https -RemoveConfigOnly -Confirm:$false
</code></pre>

## Introduction to IIS Configuration Backup and Restore
We will explore the processes of backing up and restoring Internet Information Services (IIS) configurations using PowerShell. These tasks are crucial for maintaining the stability and reliability of your web server environment. By the end, you will understand how to create backups of your IIS configurations, restore them when needed, and manage these backups effectively.

### Backup IIS Configuration
To create a manual backup of your IIS configuration, use the `Backup-WebConfiguration` cmdlet:
```powershell
$backupName = "IISBackup_" + (Get-Date).ToString("yyyyMMdd_HHmmss")
Backup-WebConfiguration -Name $backupName
```
This command creates a backup with a timestamped name in the default backup directory: `C:\Windows\System32\inetsrv\backup`
> More Information on the command: [Backup-WebConfiguration](https://learn.microsoft.com/en-us/powershell/module/webadministration/backup-webconfiguration?view=windowsserver2025-ps)

### List Backups
To get the list of backups, you can use the `Get-ChildItem` cmdlet to view the contents of the backup directory:
```powershell
Get-ChildItem -Path "C:\Windows\System32\inetsrv\backup"
```

Alternatively, you can use the `Get-WebConfigurationBackup` cmdlet.
```powershell
Get-WebConfigurationBackup
```
You may notice in the output there are names such as CFGHISTORY_0000000003 and other numbers. These are generated by IIS to keep track of configuration history.

### Restore IIS Configuration
To restore a backup, use the `Restore-WebConfiguration` cmdlet. In the example below, it restores a backup named `IISBackup`:
```powershell
Restore-WebConfiguration -Name "IISBackup"
```
Once the backup is restored, you will need to refresh or reload the IIS Manager Console to see the changes.

### Remove Backups
To remove a backup, use the `Remove-WebConfigurationBackup` cmdlet. In this example, it removes a backup named `IISBackup`:
```powershell
Remove-WebConfigurationBackup -Name "IISBackup"
```

To delete multiple backups, you can use a loop. For example, to delete all backups with names starting with `IISBackup`, you could use:
```powershell
Get-WebConfigurationBackup | Where-Object { $_.Name -like "IISBackup*" } | ForEach-Object { Remove-WebConfigurationBackup -Name $_.Name }
```

To delete backups older than 30 days, you could use:
```powershell
$thresholdDate = (Get-Date).AddDays(-30)
Get-WebConfigurationBackup | Where-Object { $_.CreationDate -lt $thresholdDate } | ForEach-Object { Remove-WebConfigurationBackup -Name $_.Name }
```



## 99 - IIS Checkpoint
This is the graded part of the lab, document the process and show the completion of the challenges. The scenario is in the assignment.


----


# Linux Apache
## 00 Getting Started with Apache


### Installation:
<pre><code class="language-bash">
sudo apt update
sudo apt install apache2
</code></pre>
The service should now be running and enabled on startup.
<pre><code class="language-bash">
sudo systemctl status apache2
</code></pre>

You can now verify that our Apache2 is working by doing a visual verification by visiting the server with a web browser. You should be greeted with the Apache2 welcome page


Run the setupUpWeb.sh script that you import into the Linux VM.
<pre><code class="language-bash">
sudo ./setupWeb.sh
</code></pre>

## 01 Our First Apache Site

Make the directory where our website content will be stored
<pre><code class="language-bash">
sudo mkdir -p /var/www/apache.local/html
</code></pre>
Let's create some content for our first Apache site's index.html file in our websites content directory /var/www/apache.local/html/ Create and edit a file called index.html with a text editor.
<pre><code class="language-bash">
sudo vim /var/www/apache.local/html/index.html
</code></pre>

Input the following HTML code into the file:
```
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8"/>
        <title>My First Apache Site</title>
    </head>
    <body>
        <h1>This is my first site hosted with Apache</h1>
    </body>
</html>
```


Change ownership to the directories so www-data is the owner.
<pre><code class="language-bash">
sudo chown www-data:www-data -R /var/www/apache.local
</code></pre>

Next we will need to setup our new virtual host under the Apache configuration. Let's copy the default default Virtual Host file under /etc/apache2/sites-available and use it as a template for our apache.local.conf file.
<pre><code class="language-bash">
sudo cp /etc/apache2/sites-available/000-default.conf /etc/apache2/sites-available/apache.local.conf
</code></pre>

You will now need to update the apache.local.conf file to look similar to the following:
```
<VirtualHost *:80>
        ServerName apache.local
        ServerAlias www.apache.local *.apache.local
        ServerAdmin webmaster@apache.local
        DocumentRoot /var/www/html/apache.local
        ErrorLog ${APACHE_LOG_DIR}/apache.local_error.log
        CustomLog logs/apache.local_access.log combined
</VirtualHost>
```


Verify our syntax is OK and has no errors with `apachectl -t`
<pre><code class="language-bash">
sudo apachectl -t
</code></pre>
Now reload the service and browse to the website.

## 02 Using Virtual Hosting

**Create Document Roots for Each Virtual Host:**    
```
sudo mkdir -p /var/www/Avhost1.local
sudo mkdir -p /var/www/Avhost2.local
```

**Set Permissions for Document Roots:**
```
sudo chown -R www-data:www-data /var/www/Avhost1.local
sudo chown -R www-data:www-data /var/www/Avhost2.local
```

**Create Virtual Host Configuration Files:**
- For Avhost1.local:
```
sudo nano /etc/apache2/sites-available/Avhost1.local.conf
```
**Add the following configuration:**
```
<VirtualHost *:80>
	ServerAdmin admin@Avhost1.local
	ServerName Avhost1.local
	DocumentRoot /var/www/Avhost1.local
	ErrorLog ${APACHE_LOG_DIR}/Avhost1.local_error.log
	CustomLog ${APACHE_LOG_DIR}/Avhost1.local_access.log combined
</VirtualHost>
```

- For Avhost2.local:
```
sudo nano /etc/apache2/sites-available/Avhost2.local.conf
```
**Add the following configuration:**
```
<VirtualHost *:80>
	ServerAdmin admin@Avhost2.local
	ServerName Avhost2.local
	DocumentRoot /var/www/Avhost2.local
	ErrorLog ${APACHE_LOG_DIR}/Avhost2.local_error.log
	CustomLog ${APACHE_LOG_DIR}/Avhost2.local_access.log combined
</VirtualHost>
```

**Enable the Virtual Hosts:**
```
sudo a2ensite Avhost1.local.conf
sudo a2ensite Avhost2.local.conf
sudo systemctl reload apache2
```


## 03 Securing Our Apache Site

### Implement HTTPS with Self-Signed Certificates

**Generate Self-Signed Certificates:**
Here is an example on how to generate a self-signed certificate and store the cert and key into a file named **Avhost1.local.key** and **Avhost1.local.crt**. When running the command you will need to input various fields (e.g., Country, State, City, Company, Section, FQDN, Email).
```
sudo openssl req -x509 -nodes -days 365 -newkey rsa:4096 -keyout /etc/ssl/private/Avhost1.local.key -out /etc/ssl/certs/Avhost1.local.crt
```

Example Input:
> Note: The Common Name is the most crucial part. It should be the Fully Qualified Domain Name (FQDN) of your server (e.g., Avhost1.local). This is what clients will use to identify your server and it should match the domain name of your website.
<pre>
Country Name (2 letter code) [AU]:US
State or Province Name (full name) [Some-State]:Indiana
Locality Name (eg, city) []:Indianapolis
Organization Name (eg, company) [Internet Widgits Pty Ltd]:CIT
Organizational Unit Name (eg, section) []:Education
Common Name (e.g. server FQDN or YOUR name) []:Avhost1.local
Email Address []:admin@Avhost1.local
</pre>

**Update Virtual Host Configurations for HTTPS:**
- For Avhost1.local:
**Add the following configuration to the Avhost1.local.conf file:**
```
<VirtualHost *:443>
	ServerAdmin admin@Avhost1.local
	ServerName Avhost1.local
	DocumentRoot /var/www/Avhost1.local
	
	SSLEngine on
	SSLCertificateFile /etc/ssl/certs/Avhost1.local.crt
	SSLCertificateKeyFile /etc/ssl/private/Avhost1.local.key
	
	ErrorLog ${APACHE_LOG_DIR}/Avhost1.local_error.log
	CustomLog ${APACHE_LOG_DIR}/Avhost1.local_access.log combined
</VirtualHost>
```
	
- For Avhost2.local:
**Add the following configuration for a 301 Redirect from HTTP to HTTPS:**
```
<VirtualHost *:80>
    ServerAdmin admin@Avhost2.local
    ServerName Avhost2.local
    Redirect 301 / https://Avhost2.local/
    ErrorLog ${APACHE_LOG_DIR}/Avhost2.local_error.log
    CustomLog ${APACHE_LOG_DIR}/Avhost2.local_access.log combined
</VirtualHost>
<VirtualHost *:443>
	ServerAdmin admin@Avhost2.local
	ServerName Avhost2.local
	DocumentRoot /var/www/Avhost2.local
	SSLEngine on
	SSLCertificateFile /etc/ssl/certs/Avhost2.local.crt
	SSLCertificateKeyFile /etc/ssl/private/Avhost2.local.key
	ErrorLog ${APACHE_LOG_DIR}/Avhost2.local_error.log
	CustomLog ${APACHE_LOG_DIR}/Avhost2.local_access.log combined
</VirtualHost>
```

**Enable SSL Module:**
```
sudo a2enmod ssl rewrite
sudo systemctl reload apache2
```

We should notice two different outcomes. One site will allow both HTTP and HTTPS sites to load. The other will send a 301 Redirect and redirect the site.

If you wish, you could modify the Avhost2.local configuration and swap out the Redirect line to the following to rewrite the URL instead.
<pre>
RewriteEngine On
RewriteRule ^(.*)$ https://Avhost2.local$1 [R=301,L]
</pre>


## 04 Troubleshooting Helicopters
- apachectl -S

## 05 - Finished
Since we use both Apache and Nginx there is potential for port conflict so we will disable the service from startup and stop the service. If wanting to use Apache, you'd need to start the service again. 
<pre><code class="language-bash">
sudo systemctl disable apache2
sudo systemctl stop apache2
</code></pre>
## 99 - Apache Checkpoint
This is the graded part of the lab, document the process and show the completion of the challenges. The scenario is in the assignment.



----


# Linux Nginx

NGINX
- /etc/nginx : default configuration root for NGINX
- /etc/nginx/nginx.conf : Main configuration file
- /etc/nginx/sites-available/ : Configuration files of individual websites that are available to be served
- (but not necessarily enabled)
- /etc/nginx/sites-enabled/ : Symbol links to individual website configuration sites (in sites-available) that
- are actively being served
- /var/log/nginx/ : default log location for NGINX

Nginx Commands:
- Shows the NGINX help menu: nginx -h
- Shows the NGINX version: nginx -v
- Tests the NGINX configuration: nginx -t
- Send a signal to NGINX master process (stop/quit/reload/reopen) : nginx -s signal

## 00 - Getting Started


### Installation:
<pre><code class="language-bash">
sudo apt update
sudo apt install nginx
</code></pre>
### Exploring basic configuration
<pre><code class="language-bash">
cat /etc/nginx.conf
</code></pre>

| Directive | Setting                           | Description                                                                                      |
| --------- | --------------------------------- | ------------------------------------------------------------------------------------------------ |
| user      | www-data                          | Defines the user used by the worker processes                                                    |
| include   | /etc/nginx/modules-enabled/*.conf | loads configurations for modules                                                                 |
| include   | /etc/nginx/mime.types             | allows Nginx to know various mime types for files and how to process them (jpg, css, html, ect.) |
| include   | /etc/nginx/conf.d/*.conf          | Virtual Host Configs loaded from this directory                                                  |
| include   | /etc/nginx/sites-enabled/*        | Virtual Host Configs loaded from this directory                                                  |

### Disable the default site 
We now want to disable the default NGINX site so it we can create our own. Start by unlinking the default page in /etc/nginx/sites-enabled/ directory. We can do this with the unlink command. This will keep the default configuration in the sites-available directory that can be used for reference if needed.
<pre><code class="language-bash">
sudo unlink /etc/nginx/sites-enabled/default
</code></pre>
Restart Nginx and check the status:
<pre><code class="language-bash">
sudo systemctl restart nginx
systemctl status nginx
</code></pre>


## 01 - Our First Nginx Site


Make the directory where our website content will be stored
<pre><code class="language-bash">
sudo mkdir -p /var/www/example.local/html
</code></pre>
Let's create some content for our first sites index.html file and a styles.css file in our websites content directory /var/www/example.local/html/ Create and edit a file called index.html with a text editor.
<pre><code class="language-bash">
sudo vim /var/www/example.local/html/index.html
</code></pre>

Input the following HTML code into the file:
```
<!DOCTYPE html>
<html>
    <head>
        <meta charset="UTF-8"/>
        <title>My First NGINX Site</title>
    </head>
    <body>
        <h1>This is my first site hosted with NGINX</h1>
    </body>
</html>
```

Create and edit a file called styles.css with a text editor.
<pre><code class="language-bash">
sudo vim /var/www/example.local/html/styles.css
</code></pre>

Input the following CSS code into the file:
<pre><code class="language-html">
h1 {
	background-color: green;
	color: white;
	text-align: center;
}
</code></pre>

Change ownership to the directories so www-data is the owner.
<pre><code class="language-bash">
sudo chown www-data:www-data -R /var/www/example.local
</code></pre>

Now create an empty example.local configuration file in the /etc/nginx/sites-available directory so we can modify it later
<pre><code class="language-bash">
sudo touch /etc/nginx/sites-available/example.local
</code></pre>

Edit the example.local file with a text editor of choice. In the example I am utilizing VIM.
<pre><code class="language-bash">
sudo vim /etc/nginx/sites-available/example.local
</code></pre>

The configuration should look like:
<pre><code class="language-text">
server {
    listen 80;
    server_name example.local www.example.local;
    root /var/www/example.local/html;
    
    location / {
        try_files $uri $uri/ =404;
    }
    access_log /var/log/nginx/example.local_access.log;
    error_log /var/log/nginx/example.local_error.log;
}
</code></pre>

Create a symbolic link from the sites-available directory to the sites-enabled directory.
<pre><code class="language-bash">
sudo ln -s /etc/nginx/sites-available/example.local /etc/nginx/sites-enabled/
</code></pre>

Check for syntax mistakes
<pre><code class="language-bash">
sudo nginx -t
</code></pre>

Expected output of `sudo nginx -t`:
<pre>
nginx: the configuration file /etc/nginx/nginx.conf syntax is ok
nginx: configuration file /etc/nginx/nginx.conf test is successful
</pre>

If there wasn't any errors in the configuration test we can safely reload Nginx.
<pre><code class="language-bash">
sudo nginx -s reload
</code></pre>

Nginx should now be serving our newly created site. You can verify that the newly created site is working doing by visiting the server with a web browser.

## 02 - Virtual Hosting
We will start exploring using Host-based Virtual Hosting for our Nginx web server to host multiple sites.

Make additional directories for the virtual domains in /var/www
```
sudo mkdir -p /var/www/Nvhost1.local/html
sudo mkdir -p /var/www/Nvhost2.local/html
```

Create the index.html files for the two virtual domains. We will copy the previous tasks html file and update the contents.
```
sudo cp /var/www/example.local/html/index.html /var/www/Nvhost1.local/html/index.html
sudo cp /var/www/example.local/html/index.html /var/www/Nvhost2.local/html/index.html
```

Update the index.html files for both the virtual hosts.

Let's also change the ownership to www-data again for our newly created directories and files so the owner isn't root.
```
sudo chown www-data:www-data -R /var/www/
```

Create empty configuration files in the /etc/nginx/sites-available directory
```
sudo touch /etc/nginx/sites-available/Nvhost1.local.conf /etc/nginx/sites-available/Nvhost2.local.conf
```

Edit the config files with a text editor of choice. In the example I am utilizing VIM.
```
sudo vim /etc/nginx/sites-available/Nvhost1.local
```

The configuration file should look something similar to this:
```
server {
    listen 80;
    root /var/www/Nvhost1.local/html;
    server_name Nvhost1.local www.Nvhost1.local;
    location / {
        try_files $uri $uri/ =404;
    }
}
```

Repeat the previous step but update the Nvhost2.local sites configuration file instead

Test if the configuration has any errors with the nginx -t command
```
sudo nginx -t
```

If the test was successful, reload nginx with the following command:
```
sudo nginx -s reload
```

Nginx should now be serving the additional newly created sites. You can verify that the newly created sites are working by doing by visiting the server with a web browser.

## 03 - Securing

<pre><code class="language-bash">
sudo openssl dhparam -out /etc/ssl/certs/dhparam.pem 2048
</code></pre>

Create a file **/etc/nginx/snippets/Nvhost1.local-selfsigned.conf** and add the following: 
<pre>
ssl_certificate /etc/ssl/certs/Nvhost1.local-selfsigned.crt;
ssl_certificate_key /etc/ssl/private/Nvhost1.local-selfsigned.key;
</pre>

Create a file **/etc/nginx/snippets/ssl-params.conf** and add the following:
<pre>
ssl_protocols TLSv1.2 TLSv1.3;
ssl_prefer_server_ciphers on;
ssl_dhparam /etc/ssl/certs/dhparam.pem;
ssl_ciphers 'ECDHE-ECDSA-AES256-GCM-SHA384:ECDHE-RSA-AES256-GCM-SHA384:ECDHE-ECDSA-CHACHA20-POLY1305:ECDHE-RSA-CHACHA20-POLY1305:ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256';
ssl_ecdh_curve secp384r1;
ssl_session_timeout 1d;
ssl_session_cache shared:SSL:10m;
add_header X-Content-Type-Options nosniff;
add_header X-Frame-Options DENY;
add_header X-XSS-Protection "1; mode=block";
</pre>

Generate a self-signed certificate
<pre><code class="language-bash">
sudo openssl req -x509 -nodes -days 365 -newkey rsa:4096 -keyout /etc/ssl/private/Nvhost1.local.key -out /etc/ssl/certs/Nvhost1.local.crt -subj "/C=US/ST=Indiana/L=Indianapolis/O=CIT/OU=Education/CN=Nvhost1.local/emailAddress=webmaster@Nvhost1.local" -addext "subjectAltName=DNS:Nvhost1.local,DNS:*.Nvhost1.local"
</code></pre>
Edit the Nvhost1.local.conf file in /etc/nginx/sites-available to look similar to the follow.
<pre>
server {
    listen 80;
    server_name Nvhost1.local www.Nvhost1.local;
    root /var/www/Nvhost1.local/html;
    #rewrite ^ https://$host$request_uri? permanent;
    #return 301 https://$host$request_uri;
	location / {
        try_files $uri $uri/ =404;
    }
}

server {
    listen 443 ssl;
    server_name Nvhost1.local www.Nvhost1.local;

    keepalive_timeout 70;

    root /var/www/Nvhost1.local/html;
    location / {
        try_files $uri $uri/ =404;
    }
    include snippets/Nvhost1.local-ssl.conf;
    include snippets/ssl-params.conf;
    access_log /var/log/nginx/Nvhost1.local_access.log;
    error_log /var/log/nginx/Nvhost1.local_error.log;
}
</pre>
Test the Configuration to see if it's OK

Then restart Nginx
<pre><code class="language-bash">
sudo systemctl restart nginx
</code></pre>

## 04 - Troubleshooting
Below are a few options when it comes to troubleshooting or using Nginx.

Check Service status with systemctl. You can also use systemctl to stop or start the service as well.
<pre><code class="language-bash">
sudo systemctl status nginx
</code></pre>

Send signal to master Nginx process (e.g., stop, quit, reopen, reload). The below command tells the master Nginx process to reload.
<pre><code class="language-bash">
sudo nginx -s reload
</code></pre>
Test the configuration and exit
<pre><code class="language-bash">
sudo nginx -t
</code></pre>
Testing configuration, dumps all content and exit
<pre><code class="language-bash">
sudo nginx -T
</code></pre>
Review the log file. In this example it's looking at the default error.log. The `tail` command views the last part of the file and the **-f** monitors the file as it grows.
<pre><code class="language-bash">
sudo tail -f /var/log/nginx/error.log
</code></pre>
## 05 - Finished
Since we use both Apache and Nginx there is potential for port conflict so we will disable the service from startup and stop the service. If wanting to use Nginx, you'd need to start the service again. 
<pre><code class="language-bash">
sudo systemctl disable nginx
sudo systemctl stop nginx
</code></pre>
## 99 - Nginx Checkpoint
This is the graded part of the lab, document the process and show the completion of the challenges. The scenario is in the assignment.

----
# Additional Resources
+ [MS PktMon](https://learn.microsoft.com/en-us/windows-server/networking/technologies/pktmon/pktmon)
+ [BIND 9 Administrator Reference Manual](https://bind9.readthedocs.io/en/latest/index.html)
+ [Apache Documentation](https://httpd.apache.org/docs/current/)

https://nginx.org/en/docs/
https://docs.nginx.com/nginx/admin-guide/web-server/
https://nginx.org/en/docs/index.html





----




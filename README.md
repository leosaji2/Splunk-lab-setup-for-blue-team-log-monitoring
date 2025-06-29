# Splunk-lab-setup-for-blue-team-log-monitoring
This project demonstrates the setup of a Splunk-based log monitoring system designed for Blue Team defensive operations. It includes a complete walkthrough of configuring Splunk Enterprise on a Windows system and deploying the Splunk Universal Forwarder on a Linux virtual machine to forward logs in real-time to the central Splunk server.
# Introduction to Splunk Enterprise 
In today’s data-driven world, organizations generate massive volumes of machine data from applications, servers, networks, and security systems. Splunk Enterprise is a powerful platform designed to harness this data, enabling real-time search, analysis, and visualization of machine-generated information. Splunk Enterprise collects data from diverse sources—such as websites, applications, sensors, and devices—and indexes it into searchable events. Users can then explore this data through intuitive dashboards, reports, and alerts, helping them monitor system health, detect anomalies, and make informed decisions. Whether it's IT operations, cybersecurity, or business analytics, Splunk Enterprise provides a unified solution for turning raw data into actionable insights. With its scalable architecture, customizable apps, and support for advanced analytics, Splunk Enterprise empowers organizations to gain visibility across their digital infrastructure and respond swiftly to emerging issues. 
# HOW TO SETUP SPLUNK ENTERPRISE IN WINDOWS 
Step 1: Download Splunk Enterprise from https://www.splunk.com/en_us/download.html after signing up to the account. 

![image](https://github.com/user-attachments/assets/d661315a-85b5-4ed7-88a8-2a47a212bb28)
Download Splunk enterprise for windows (64-bit windows 10 windows server 2019,2022) 


Step 2: To start the installer :
Double-click on the splunk.msi file. The installer runs and displays the Splunk Enterprise Installer panel. 

![image](https://github.com/user-attachments/assets/82215361-c617-4a27-8c68-75aca3e194d4)

Step 3: Accept the License Agreement 
To proceed with the installation, check the box labeled "Check this box to accept the License Agreement." This action enables the Customize Installation and Next buttons. If you wish to review the license terms before continuing, click View License Agreement. 

Step 4: Choose Installation Options 
The installer offers two modes: proceed with default settings or customize the installation parameters. Selecting the default option will: 

●	Install Splunk Enterprise in \Program Files\Splunk on the system drive 

●	Use default ports for management and web access

●	Configure the service to run under the Local System account 

●	Prompt you to set an administrator password before continuing 

●	Create a Start Menu shortcut for easy access 

To modify any of these defaults, click Customize Options and follow the corresponding instructions. Otherwise, click Next, enter the desired admin password when prompted, and the installation will begin. 

If you choose to customize: 

●	You'll be asked where to install Splunk. You can keep the default path or click Change… to select a different folder. 

●	You'll choose the Windows account Splunk will run as: 

Local System, or a domain user (must be entered as domain\username and have admin rights). 

Step 5: Enter Credentials  


![image](https://github.com/user-attachments/assets/de8c8830-d7f8-43a3-8d44-c886abceb49a)

Step 6: Review and Install 

● The installer will now show a summary of your chosen settings.

● Click Install to begin the installation. 

Once the installation completes, you're ready to launch Splunk  

![image](https://github.com/user-attachments/assets/e760f23e-2069-437a-a15e-9665efb105ec)

Use the Credentional when installation time you set.

![image](https://github.com/user-attachments/assets/bd88a9a1-68e8-49fc-b353-e6e8e51d48df)

Step 7:Enable Receiving Port (on Indexer or Receiver) 

To allow Splunk to receive data from forwarders: 

1.	Log in to Splunk Web as an admin.

2.  Go to Settings > Forwarding and receiving.
  
3.	Under Receive data, click Configure receiving.
  
4.	Click New Receiving Port.
   
5.	Enter the port number (commonly 9997) and click Save.

This sets up your Splunk instance (usually an indexer or heavy forwarder) to listen for incoming data on the specified port. 





# HOW TO SETUP UNIVERSAL FORWARDER IN KALI LINUX 

Step 1: Download the forwarder  

1.	Go to the Splunk Universal Forwarder download page.

2.  Choose Linux as your platform.

3.  Select the .deb package for 64-bit Linux.


Step 2: Extract and install the forwarder 

  1.	If the file is zipped (e.g., .zip), extract it using terminal
   
       unzip splunkforwarder-<version>.zip

    
  3.	Install the package using dpkg
   
       sudo dpkg -i splunkforwarder--linux-2.6-amd64.deb

Step 3: Start and Enable Splunk Forwarder 

  1.	cd /opt/splunkforwarder/bin- Path of splunk universal forwarder
     
  2.	Configure the forwarder to start automatically with the system using command

       sudo ./splunk enable boot start 


  ![image](https://github.com/user-attachments/assets/a93605a1-cea7-46f0-9058-86fa67e5d62e)


Step 4: Create an account for forwarder 

  Create the username and password for splunk universal forwarder 


![image](https://github.com/user-attachments/assets/bd1d1842-5085-44f8-a367-69bec08bf160)


Step 5: Enable Receiving input on the Index Server 

     sudo ./splunk enable listen 9997


  ![image](https://github.com/user-attachments/assets/3f025f4f-8192-45c9-9d7a-ea6afcac729d)


  
Step 5: Configure Forwarder connection to Index Server

      sudo ./splunk add forward-server <IP of the index server>:9997 

  (windows ip or ip of splunk enterprise deployed system)

Step 6: Test Forwarder connection: 

    sudo ./splunk list forward-server 

Step 7: Add Files/Directories to Monitor 

    sudo ./splunk add monitor /path/to/app/logs/ -index main -sourcetype %app% 


  Where /path/to/app/logs/ is the path to application logs on the host(/var/log/) that you want   to bring into Splunk, and %app% is the name you want to associate with that type of data 


![image](https://github.com/user-attachments/assets/32320047-7f1b-4d5f-b772-bac50c010a43)


Step 8:Check and Restart if Needed 

  To verify the connection between the forwarder and indexer: 

    sudo ./splunk list forward-server 

  Look for a status like: 

    Active forwards: 
 	  ip address indexer:9997 
    Configured but inactive forwards: 
  	(none) 
   
  If the forwarder is not showing “active”, try restarting it:

    sudo ./splunk restart 


![image](https://github.com/user-attachments/assets/2915729a-800c-44e0-9a7a-854a64cdf6e6)

Step 9: Verify Logs on the Indexer 

 Once everything is set up and the connection is active, head over to your Splunk Indexer (via   web interface) and run a quick search to check if logs from the forwarder are coming in:

 Where to Find It: 
 
 Go to the Search & Reporting app → Click on Data Summary 


 ![image](https://github.com/user-attachments/assets/109daefc-6e7d-47d5-bed0-8c4be0e4345b)


























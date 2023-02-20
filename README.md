
# Azure Sentinel Detection 'Honeypot' Lab

Project makes use of Azure Sentinel  used to log the information of various threat vectors attempting to brute force their way into our virtual environment. The main method in which I acheive this outcome is by creating a virtual machine within Microsfot Azure and have it act as a honeypot to intrusive IPs by leaving it vulnerable. I was able to construct this lab without the aid of any programming, just a few scripts using CLI. 


## Azure Environment

![Azure Overview](https://i.postimg.cc/7L9cqxb9/2023-02-19-2.png)
![Azure VM](https://i.postimg.cc/nzPncKM3/2023-02-19-4.png)

Here us a quick look at the enviornment for Microsofot Azure for those unfamiliar with it. Right now you can see the configurations for the virtual machine created as well as several of the resources I used. 

When configurating the networking settings for the VM, I gave it a public IP address and turned off the firewall making it incredibly exposed to the internet allowing pretty anyone from anywhere to ping (hence why its a honepot). 


## Setup
- [Microsoft Azure](https://azure.microsoft.com/en-us/free/) - You can signup and create free account.
- [IPGeolocation](https://ipgeolocation.io/) - Free IP Geolocation API and Accurate IP Lookup Database. Utilized this service to pull data for parameters such as longitude/latitude, country, and time zone and sync it with my dashboard.


## Simulating Attack
After remoting into my VM with RDP, I turned off all firewall settings allowing any IPs to connect to my VM. I then then performed several failed login attempts my actual workstation to gather some sample data to simulate breach attempts. You can see these attempts in Event Viewer in our VM where it will show you some basic info such as computer name and source network address.

![Event Viewer](https://i.postimg.cc/5ygCz4P0/2023-02-20-2.png)

Note the event ID: 4625. That is error code for failed login attempts.

I then initialized my Log Analytics Workspace in Azure which we will use later to map the results from our results from the exercise.

Next, I used a custom PowerShell script to export metadata from login attempts. Its a simple script that runs a loop scouting the security log in Event Viewer on our VM for event ID 4625 and grabs the IP addresses and geodata of machines that tried to access it and generates a new log file.


![PowerShell ISE](https://i.postimg.cc/6QwqwJ65/2023-02-20-5.png)

Since our vulnerable VM has been active for a while, there has been a lot of incoming IPs trying to gain access to our computer. The going trend seems to be that most of them are from overseas as relavent from PowerShell output. After this it was simply a case of mapping out where these IP addresses where attempting to gain access from in Azure Workspace Log Analytics and editing the parameters of the workbook.

![Log Analytics Overview](https://i.postimg.cc/KjPbz48K/2023-02-20-8.png)

Shows the number of security alerts encountered just over the span of a single day.

![Workbook](https://i.postimg.cc/MpkBNFhd/2023-02-20-10.png)

Workbook with condensed stats along with query I ran to plot geodata with constraints and labels.

![Map](https://i.postimg.cc/tgvhgtrc/2023-02-20-11.png)

Here is our map. It seems our VM is garnishing a lot of trafic from Middle-Eastern countries with our main hotspots being Russia and the Phillipines. 






## Acknowledgements


 - [Josh Madakor](https://www.youtube.com/@JoshMadakor)
-  [Cyberwox Academy](https://www.youtube.com/@CyberwoxAcademy)


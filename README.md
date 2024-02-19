# Building-a-Mini-SOC-Lab-with-Security-Onion

# Introduction:

## The Importance of a Home Lab
Having a home lab is crucial for cybersecurity professionals, especially those starting their journey in the field. It provides a safe and controlled environment to gain hands-on experience, experiment with various tools and techniques, and simulate real-world scenarios. A home lab allows us to practice and refine our skills, test different security solutions, and stay up to date with the latest threats and vulnerabilities. It also offers an opportunity to explore various aspects of cybersecurity, such as incident response, threat hunting, and network monitoring, in a practical setting.

## What is Security Onion?
SecurityOnion is an open-source tool that helps organizations build and monitor their security operations center (SOC). It provides a platform for network security monitoring (NSM) and log management, enabling the detection, analysis, and response to security incidents. Security Onion includes various components and tools, such as Suricata for intrusion detection, Zeek for network traffic analysis, and Elasticsearch for log storage and analysis. It offers a comprehensive set of features to assist in threat detection, incident investigation, and overall network security. By setting up a mini SOC lab with Security Onion, you can gain practical experience in using these tools and understand how they contribute to a robust security infrastructure.

## Objective 
This guide will help cybersecurity beginners like myself get hands-on experience in building and managing a mini SOC with Security Onion. We will set up a vulnerable machine and an attacking machine to simulate a real-world attack scenario. We’ll dive into monitoring network traffic and detecting suspicious activities. We will cover the installation process and simulate common security incidents to practice our detection and response skills.

## Hardware Minimum Requirements
- Storage: 300 GB
- Ram: 16 GB
- CPU: 6 cores
## Project Items needed 
- VirtualBox and extension pack [link](https://www.virtualbox.org/wiki/Downloads)
- Vulnhub [link](https://www.vulnhub.com/)
- 1x SecurityOnion iso [link](https://github.com/Security-Onion-Solutions/securityonion/blob/2.4/main/DOWNLOAD_AND_VERIFY_ISO.md)
- 1x Ubuntu Desktop [link](https://ubuntu.com/download/desktop)
- Kali Linux ISO file [link](https://www.kali.org/get-kali/#kali-platforms)
## VM Topology
To prepare for that we need to set up a network topology where the VMs can talk to each other. Like this:
![](https://github.com/Lattice23/Building-a-Mini-SOC-Lab-with-Security-Onion/assets/159420767/e2be11a7-b2b4-4a9d-beb7-a7a2484c8718)

In Oracle VM manager go to file>tools>network manager

Add a new NAT Network and a Host-only Network like this

# Kali VM setup
Just like the Windows VM add the ISO and modify the hardware
1. Add VM to NAT Network and add Host-only adapter 1
2. Once you turn on the VM select Graphical install
3. Choose your language, keyboard layout, and country
4. Make a hostname, username, and password (domain name is optional)
5. Select Use entire disk, then select all files in one position
6. Select yes and format the disk
7. Select any software you want installed with the OS and press continue(default is fine)
8. Lastly, Install the GRUB bootloader and select the boot device
   
## Security Onion VM setup
This is a long process so read carefully

1. just like the Windows setup make a username, password, and host
2. DO not check the guest additions box
3. SecurityOnion needs at least 200GB of storage, 12 GB of RAM, and 4 CPU cores
4. Select finish and don’t power up the VM
5. in network settings switch to NAT Network with a second adapter attached to Host-only adapter 1
6. Select Install Security Onion and type “yes” to proceed
7. Create a username and password to install (this will take a while)
8. Press enter to reboot and select the first CentOS Linux
9. Log in and select Yes > Install > EVAL> type “AGREE”
10. Enter a FQDN (”seconion” is fine)
11. select the first option as the management and use DHCP
12. select standard > direct
13. select the second option as your monitor interface
14. Set automatic for updates
15. Add your NAT network IP and Adapter 1 Host-only interface IP(you can find these at file> tools> network manage)
16. Remove any software you won’t be using(Default is fine)
17. For this project choose a fake email address to log into for later use (ex: bob@seconion.com)
18. Select IP address > NTP yes > OK > SO-Allow NO
19. Scroll down and select yes to install (this will take Long)
20. Take note of the web interface URL for later use and reboot

## Ubuntu VM setup
1. just like the Windows setup make a username, password, and host
2. Modify the hardware and finish (at least 2 cores is needed)
3. add the VM to the NAT network
4. start the machine
5. Press Install Ubuntu
6. you may need to enter your user and password to finish installing

## Now we are going to set the VM to work as an analyst for SecurityOnion
1. first, go to the terminal and type “sudo apt install net-tools” (if you are not in the sudoers file type “su -” > enter password > type “sudo adduser [your username] sudo”)
2. type “ip add” and take note of the IP address
3. now enter the SecurityOnion web URL in Firefox
4. now powerup SecurityOnion
5. in the terminal type “sudo so-allow”
6. select “a” for analyst
7. then enter the IP address of the Ubuntu VM
8. Now refresh the web URL in Ubuntu
9. Click Advanced > accept the risk > and sign in with the email you made for the Security Onion web interface

## Vulnhub (proof of concept)
Go to the vulnhub and download a vulnerable machine to check if SecurityOnion is working.

Here is an easy box: Basic Pentesting 1

1. Import the VM
2. Set the Network to Host-only adapter 1 (DO NOT CONNECT TO NAT)
3. Power the Vulnhub machine
4. Power up the kali machine
5. Make sure kali is connected to 2 network interfaces using the “ifconfig” command
6. Perform a Nmap scan for the Vulnhub IP ( Ex: nmap -sn 192.168.188.1–255)
7. Once you find the IP perform another Nmap scan (Ex: nmap -sV -sC -T4 [Vulnhub IP])
As we can see Security Onion has already picked up some activity from the Kali machine. You can explore the other tools like Kibana to check this too. This activity indicates the successful implementation of our network monitoring system, offering us valuable insights into potential security threats.

## Summary

That is the end of my mini SOC lab project. I hope you were able to learn something new and gain some hands-on knowledge about network monitoring and threat detection. With consistent practice and exploration, you can further improve your skills in this area and become adept at identifying and mitigating potential security threats in real time.

S/O to DayCyberwox for the inspiration and resources for this project.

# P.S :
If you want to continue attacking and monitoring the machine here is a well-done [guide](https://www.youtube.com/watch?v=aUH7G1JICVw) from InfoSec Pat to help root it

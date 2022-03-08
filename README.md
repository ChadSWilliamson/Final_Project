# Final_Project

# Attack, Defense and Analysis of a Vulnerable Network

# Red Team Test
  - Network Topology
  - Description of Targets
  - Exposed Services
  - Critical Vulnerabilities
  - Exploitation
---------------------------------------------------------------------------------------------------------------------------------
# Network Topology
![Finalprojecttopo](https://user-images.githubusercontent.com/89936268/156804939-b293e54f-26df-479f-9e38-0530780ad0d9.png)

This scan identifies the services below as potential points of entry:

![nmapScreenshot_2022-02-26_09-13-43](https://user-images.githubusercontent.com/89936268/156850973-78c92f04-acba-44d5-8368-8682d5c23a26.png)

![nmapScreenshot2_2022-02-26_09-14-53](https://user-images.githubusercontent.com/89936268/156851001-664cb2c0-3f43-495b-89ef-18676c71d4cd.png)


# Critical Vulnerabilities: Target 1
------------------------------------------------------------------------------------------------------------------------------------

# The following vulnerabilities were identified on each target:


|    Vulnerability     |     Description         |        Impact                  |
| -------------------- | ----------------------- | ------------------------------ |
| Weakpasswords        | The passwords are weak enough to be guessed or brute forced | Bad actors can easily access accounts that they should not have access to |
| MySQL DB breach      | Unauthorized access to the database using unprotected credentials stored in wp-config.php. | Bad actors can easily access database using credentials that are stored in unprotected files. |
| WordPress reveals usernames | Using WordPress scan, MIchael and Steven's user names were revealed | Ability to SSH into Target 1 and also access the MySQL database. |
| Privilege escalation through Python | Steven has sudo privileges when executing "Python" allowing him to escalate to root privileges. | Bad actors can gain access to the root shell if they are able to access Steven's account. |
| port 22 openssh 7.6p1 Debian 5+deb8u4 | https://cve.mitre.org/cgi-bin/cvekey.cgi?keyword=openssh |
| port 80 running apache httpd 2.4.10 (Debian) | https://cve.mitre.org/cgi-bin/cvekey.cgi?keyword=Apache%20HTTP%20server |
| Port 111 running rpcbind 2-4 ( RPC #100000) | https://cve.mitre.org/cgi-bin/cvekey.cgi?keyword=rpc | 
| Port 139 and 445 running netbios-ssn Samba smbd 3.X - 4.X (workgroup: WORKGROUP) | https://cve.mitre.org/cgi-bin/cvekey.cgi?keyword=samba+smbd+3.X | 

------------------------------------------------------------------------------------------------------------------------------------------------------------------
# Reconnaissance of open port 22 SSH and weak password

 - I used wpscan to find the users associated with the company and then guessed a number of passwords to SSH into the system. 
    - Passwords guessed (123456,password,admin,michael)
 - The exploit granted me user shell access for Michael's account. I explored to find Flags 1-2.  

![wpscan2Screenshot_2022-03-04_13-08-01](https://user-images.githubusercontent.com/89936268/156845528-467277ad-9ca9-4fff-8c6f-177a12191868.png)


# Users enumerated
<img width="790" alt="Screen Shot 2022-03-04 at 3 11 01 PM" src="https://user-images.githubusercontent.com/89936268/156845628-1cec7e5c-feaf-4cb4-b711-5d9d8ce4ac91.png">


# Flag 1
![flag1Screenshot_2022-02-26_10-45-31](https://user-images.githubusercontent.com/89936268/156847029-a5b2ac37-82f5-4f37-9c79-4a63ddc71408.png)


# Flag 2
![flag2Screenshot_2022-02-26_10-20-39](https://user-images.githubusercontent.com/89936268/156847054-bb24c91c-c63d-4a5b-9eda-1808d1ca50d7.png)


# Reconnaissance of WordPress Configuration & SQL Database

  - The username and password to access the SQL database were in plain text in the wp-config.php file and not hashed as is the best practice.
  - The exploit gave me **mysql access** and gave me **flags 3, and 4**.

 **nano wp-config.php**
 
![nanowp-config phpScreenshot_2022-02-28_14-54-27](https://user-images.githubusercontent.com/89936268/156850056-12e6c4c4-7213-444a-8764-d21723ed2382.png)

**mysql login**

![mysqlloginScreenshot_2022-02-28_14-04-18](https://user-images.githubusercontent.com/89936268/156850141-19bd2a2b-f054-4af9-b1ca-bac9479233a8.png)

**Michael,Steven's user names and hashed password**

![michael stevenhashScreenshot_2022-02-28_15-02-40](https://user-images.githubusercontent.com/89936268/156852161-68097d4e-22ad-4588-b762-89f3e272d88c.png)

**Where flags 3,4 are located**
![whereflag3,4Screenshot_2022-03-01_17-27-45](https://user-images.githubusercontent.com/89936268/156853890-79b0a7d3-8fd1-47e9-ae7e-1eb0faf739de.png)

**Flags 3, 4**

![flags3-4Screenshot_2022-02-28_14-15-55](https://user-images.githubusercontent.com/89936268/156852301-b4d8deea-1bc2-49f4-a701-d878a512af26.png)


# Privilege Escalation via Python

- I cracked Steven's password using John the ripper to access his account.
- A bad actor in Steven's account can learn that they can execute "Python" with sudo privileges, the execute this command to gain access to the root shell:
    **sudo python -c 'import pty;pty.spawn("/bin/bash")'**
- The exploit acheived root access and allowed to see flag 4 again. 

![johnripperScreenshot_2022-03-01_09-58-14](https://user-images.githubusercontent.com/89936268/156853290-ae454418-bfd4-45a6-9996-b3d0644efc99.png)

<img width="947" alt="Screen Shot 2022-03-04 at 3 12 07 PM" src="https://user-images.githubusercontent.com/89936268/156853313-c0a84b9f-b63f-463d-ad79-c4bd3baf55d2.png">


![stevenpythonScreenshot_2022-03-04_13-01-35](https://user-images.githubusercontent.com/89936268/156853384-d2bd9c4c-60f4-4a03-ba7e-a14fb964299b.png)

![Screen Shot 2022-03-04 at 5 02 01 PM](https://user-images.githubusercontent.com/89936268/156853743-afde40ce-031d-4ac9-84ba-e6b02a22bf16.png)


# Blue Team Operations
  - Monitoring Targets
  - Patterns of Traffic & Behavior

# Alert 1
  - Metric: Excessive HTTP Errors
  - Threshold: When the http response code is over 500 for more than 5 mintues
  - Vulnerability Mitigated: Availability
  - Reliability: High

![HTTPrequest](https://user-images.githubusercontent.com/89936268/157099743-5ae62b30-71b4-4a46-84ca-43bb74aaa050.png)

  - Metric: HTTP Request Size
  - Threshold: When total bytes of http requests are above 3500 for last minute
  - Vulnerability Mitigated: DDOS attack
  - Reliability: Medium

![HTTPerrors](https://user-images.githubusercontent.com/89936268/157100189-873485bc-819e-4e13-92a1-9a92427c647e.png)

# Alert 3
  - Metric: CPU Usage Monitor
  - Threshold: CPU process total percentage is above 0.5 for last 5 mintues
  - Vulnerability Mitigated: DDOS attack
  - Reliability: Low

![CPUusage](https://user-images.githubusercontent.com/89936268/157100284-d106304b-10ed-4f37-88f4-6bf3ed3346bc.png)

# Network Forensic Analysis Report
- Time Thieves
  -frank-n-ted.com
- 10.6.12.12
- GET /files/june11.dll HTTP/1.1

**Follow the TCP stream**
![malwarename](https://user-images.githubusercontent.com/89936268/157133908-e7a155c4-1b2a-4d73-aae5-3847e86232db.png)

**Finding the file in wireshark to export it to desktop**
![exportmalware](https://user-images.githubusercontent.com/89936268/157133955-bd6188f0-893a-4f8c-a98f-980369af99db.png)

**june11.dll file uploaded to VirusTotal.com for analysis**
![virustotal](https://user-images.githubusercontent.com/89936268/157134387-c28ecdd7-e21c-4abd-8022-a168f417414c.png)

**File classification in VirusTotal.com**
- Trojan

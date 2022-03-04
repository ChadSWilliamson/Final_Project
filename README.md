# Final_Project

# Attack, Defense and Analysis of a Vulnerable Network

# Red Team Test
  - Network Topology
  - Description of Target
  - Critical Vulnerabilities
---------------------------------------------------------------------------------------------------------------------------------
# Network Topology
![Finalprojecttopo](https://user-images.githubusercontent.com/89936268/156804939-b293e54f-26df-479f-9e38-0530780ad0d9.png)


![nmapScreenshot_2022-02-26_09-13-43](https://user-images.githubusercontent.com/89936268/156850973-78c92f04-acba-44d5-8368-8682d5c23a26.png)

![nmapScreenshot2_2022-02-26_09-14-53](https://user-images.githubusercontent.com/89936268/156851001-664cb2c0-3f43-495b-89ef-18676c71d4cd.png)


# Critical Vulnerabilities: Target 1
------------------------------------------------------------------------------------------------------------------------------------

# Assessment uncovered these critical vulnerabilities in Tagrget 1


|    Vulnerability     |     Description         |        Impact                  |
| -------------------- | ----------------------- | ------------------------------ |
| Weakpasswords        | The passwords are weak enough to be guessed or brute forced | Bad actors can easily access accounts that they should not have access to |
| MySQL DB breach      | Unauthorized access to the database using unprotected credentials stored in wp-config.php. | Bad actors can easily access database using credentials that are stored in unprotected files. |
| WordPress reveals usernames | Using WordPress scan, MIchael and Steven's user names were revealed | Ability to SSH into Target 1 and also access the MySQL database. |
| Privilege escalation through Python | Steven has sudo privileges when executing "Python" allowing him to escalate to root privileges. | Bad actors can gain access to the root shell if they are able to access Steven's account. |

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
- - A bad actor in Steven's account can learn that they can execute "Python" with sudo privileges, the execute this command to gain access to the root shell:
    **sudo python -c 'import pty;pty.spawn("/bin/bash")'**
- The exploit acheived root access and allowed to see flag 4 again. 

![johnripperScreenshot_2022-03-01_09-58-14](https://user-images.githubusercontent.com/89936268/156853290-ae454418-bfd4-45a6-9996-b3d0644efc99.png)

<img width="947" alt="Screen Shot 2022-03-04 at 3 12 07 PM" src="https://user-images.githubusercontent.com/89936268/156853313-c0a84b9f-b63f-463d-ad79-c4bd3baf55d2.png">


![stevenpythonScreenshot_2022-03-04_13-01-35](https://user-images.githubusercontent.com/89936268/156853384-d2bd9c4c-60f4-4a03-ba7e-a14fb964299b.png)

![Screen Shot 2022-03-04 at 5 02 01 PM](https://user-images.githubusercontent.com/89936268/156853743-afde40ce-031d-4ac9-84ba-e6b02a22bf16.png)

# Wazuh File Integrity Monitoring Lab

## Objective

The Wazuh File Integrity Monitoring Lab is aimed to detect 

### Skills Learned
[Bullet Points - Remove this afterwards]

- Advanced understanding of SIEM concepts and practical application.
- Proficiency in analyzing and interpreting network logs.
- Ability to generate and recognize attack signatures and patterns.
- Enhanced knowledge of network protocols and security vulnerabilities.
- Development of critical thinking and problem-solving skills in cybersecurity.

### Tools Used
[Bullet Points - Remove this afterwards]

- Security Information and Event Management (SIEM) system for log ingestion and analysis.
- Network analysis tools (such as Wireshark) for capturing and examining network traffic.
- Telemetry generation tools to create realistic network traffic and attack scenarios.

## Steps

*Ref 1: Network Diagram*

1. For this Lab I will be using two virtual machines. One Machine will contain a pre-built virtual machine image in an Open Virtual Appliance(OVA) format containing the following components: Wazuh's manager, indexer, and dashboard. I will use this Wazuh OVA image with Oracle VM Virtual Box. The other virtual machine will be a Windows Server 2022 as my target machine

     ![Wazuh virtual box](https://github.com/user-attachments/assets/a1a0d240-cab6-421d-9bad-6ae4ab06e32f)

2. After getting both the virtual machines up and running my next step is to get the Wazuh agent installed on the Windows 2022 server. From the official Wazuh documentation website, I can download the Wazuh agent and it will look like this when complete: 
![download](https://github.com/user-attachments/assets/69024763-cb23-40e9-8020-79f9b57da560)

3. Now I need to get the Wazuh server IP adress and the authentication key to link the Wazuh server to the Wazuh Agent

4. To go the IP address I will run the "ip a" command:
 ![wazuh ip](https://github.com/user-attachments/assets/bfb8e0f2-4cfb-48e3-8072-76b7cfecc7d9)

5. Next I will get the authenticaion key by accessing /var/ossec/bin/manage_agents on the Wazuh Manger host
![Wazuh agetn key](https://github.com/user-attachments/assets/c75a97c1-206d-4954-8927-31e465b636f6)

6. Turning back to the Windows Server 2022 when can import our findings for the IP adress and the authentication key

    ![Wazuh windows key](https://github.com/user-attachments/assets/4ca85ed1-32bd-41fa-955c-c3db8195b87c)

7. Now to confirm we have set Wazuh up correctly I will try and access the Wazuh dashboard using the IP address of the Wazuh host which is 192.168.100.131 in Firefox

   ![wazuh dashboard](https://github.com/user-attachments/assets/be0a8705-c5b6-44d0-b66e-b4fbd51e2c77)

8. After I signed in I can see that the agent is active

      ![Wazuh agent](https://github.com/user-attachments/assets/bb8cbb5c-0135-4796-bfcc-0d6340f51ea7)

9. I can now start to configure File integrity monitoring on the Windows 2022 server machine. First, I will have to navigate to This PC > Local Disk(C:) > Program Files(x86) > ossec-agent and then I will edit the ossec.conf file. Before I make changes, as best practice I will copy the ossec.conf file and rename the copy as ossec-backup.conf just incase I mess up the configuration file later on so I have a clean backup.
    
    ![Wazuh backup](https://github.com/user-attachments/assets/a05aa920-4006-4b85-8a0c-7e2d40c380fc)

10. Since the Public and Temp directories are common places for attackers to upload their tool sets. I will select one for this lab, which will be the Public directory.


11. I will open up the ossec.conf file and will start to configure the File Ingtegrity Monitoring to monitor the Public directory. I will copy and paste one of the lines under the "Default files to be monitored" as I will make changes to the line to fit my needs.

	- The first edit will be to add the path of the dirctory that I want to be monitored which is C:\Users\Public

	- Next, I will remove the recursion level section, leaving recursion in it will only monitor that directory and no further. To make sure the monitoring can go through multiple levels it needs to be removed and then it will monitor everything in the public directory. In my case, I do not know how many levels I will need to monitor and this will fulfill my needs.

	- The default setting for File Integrity Monitoring (FIM) is to check for changes every 12 hours, I have added realtime="yes" which makes it so FIM checks for changes in realtime.

![Wazuh Realtime](https://github.com/user-attachments/assets/6ba1502a-2fec-4344-be91-530ddbe1716c)

12. Now FIM is set up and let's simulate an event. I will create a file called evil-malware in the Public directory. As you can see in the Wazuh dashboard, we have an event which has identified a file that was ADDED under the C:\users\public\pictures directories as "evil-malware".

 ![image](https://github.com/user-attachments/assets/47c45d18-0937-4895-a835-c5cb7b2cb141)


 ![Wazuh evil-malware as Event](https://github.com/user-attachments/assets/e6bcda90-d32a-44f5-9308-e2ee292bb0cc)
 
  Clicking the side arrow we can look at more specific information relation to this event. We can see the agent ID, IP, and name. We can also see the path and file that was added.
   
   ![Wazuh new fields](https://github.com/user-attachments/assets/203bf0e7-4072-405e-a6c8-e9feccafd2ee)

   
   
   You are also supplied with the MD5, SHA1 and SHA256 hash values of the file which is really interesting as you can compare those hashes using open-source intelligence like VirusTotal to help investigate the suspected file

![Wazuh new hash](https://github.com/user-attachments/assets/8ca2de42-9a46-440b-8341-7e5138f890f4)

In this next simulation, I am going to show a different scenario where FIM will be very useful. There are Bank records that are NOT to be modified ever. This will show how FIM can detect files that have been modified.

13. I have created a file in the public\documents directory called "classified bank records.txt" and have added text as "200.00" and then saved the file. I then opened the file back up and changed the text to "20000.00" and saved it.

 As show in the picture we can clearly see that this classified file never to be modified has been modified, We are now alerted and can begin an investigation into this incident.

 ![Wazuh Bank record modified](https://github.com/user-attachments/assets/7822954a-0896-475f-a2a7-481c9a472649)
 
Now I want to show another use for FIM. We can set up FIM to monitor registry keys which are commonly used for persistence. To simulate this I will create a registry key under the HKey local machine run key which will be named "UnusualFile-evil"


 
![Wazuh REgkey](https://github.com/user-attachments/assets/83736ffa-18e7-4ca2-bb49-d25af9ac1516)

 

 





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

9. I can now start to configure File integrity monitoring on the Windows 2022 server machine. First, I will have to navigate to This PC > Local Disk(C:) > Program Files(x86) > ossec-agent and then I will edit the ossec.conf file. Before I make changes, as best practice I will copy the ossec.conf file and rename the copy as ossec-backup.conf just incase I mess up the configuration file later on so I have a clean backup
    
    ![Wazuh backup](https://github.com/user-attachments/assets/a05aa920-4006-4b85-8a0c-7e2d40c380fc)


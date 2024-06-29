# SIEM-Lab
In this guide, I’ll walk you through steps on how to set up a home lab for Elastic Stack Security Information and Event Management (SIEM) using the Elastic Web portal and a Kali Linux VM. You will also learn how to generate security events on the Kali VM, set up an agent to forward data to the SIEM, and query and analyze the logs in the SIEM. This is a great project that you can add to your resume and talk about in interviews.

## Prerequisites
Before we get started, make sure you have the following:

1. VirtualBox or VMware
2. Basic knowledge of Linux and virtualization software.
## Overview of the tasks
1. Set up a free Elastic account.
2. Install the Kali VM.
3. Configure the Elastic Agent on the Linux VM to collect the logs and forward it to the SIEM.
4. Generate security events on the Kali VM.
5. Query to find the security events in the Elastic SIEM.
6. Create a Dashboard to visualize security events.
7. Create alerts for security events.
Please note that with updates to the Elastic web console, the look and layout of the console may change. The instructions provided here are accurate as of the date they were written, but may need to be updated for future versions of the Elastic web console. If you notice any discrepancies or changes to the web console that affect the accuracy of these instructions, please let me know in the comments so that I can update the instructions accordingly.

## Task 1: Set up an Elastic Account
Before we get started, we need to create a free account to set up a cloud Elastic instance that we can run the SIEM on. To do that, follow these steps:

1. Sign up for a free trial to use Elastic Cloud at https://cloud.elastic.co/registration
2. Once you have an Elastic account, log in to the Elastic Cloud console at https://cloud.elastic.co.
3. Click on “Start your free trial.”
4. Click on the “Create Deployment” button and select “Elasticsearch” as the deployment type.
5. Choose a region and deployment size that fits your needs and click on “Create Deployment.”
6. Wait for the configuration to complete.
7. Once the deployment is ready, click “continue.”
## Task 2: Setting up the Linux VM
Next, we need to set up the Linux VM. You can use any Linux OS and virtualization software for this, but I’ll be using Kali Linux and Oracle VirtualBox.

### To set it up, follow these steps:

1. Download the Kali Linux VM from the official Kali website at https://www.kali.org/get-kali/#kali-virtual-machines.
2. Create a new VM with the Kali VM file in your preferred virtualization platform, such as VirtualBox or VMware.
3. Start the VM and follow the on-screen prompts to install Kali.
4. Once the installation is complete, log in to the Kali VM using the credentials “kali” for both the username and password.
Note: If you encounter any difficulties with this particular task, you can search for something like this on YouTube: “How to create a virtual machine using VirtualBox/VMware with a Kali VM file.”

## Task 3: Setting up the Agent to Collect Logs
An agent is a software program that is installed on a device, such as a server or endpoint, to collect and send data to a centralized system for analysis and monitoring. In the context of Elastic SIEM, an agent is used to collect and forward security-related events from your endpoints to your Elastic SIEM instance.

### To set up the agent to collect logs from your Kali VM and forward them to your Elastic SIEM instance, follow these steps:

1. Log in to your Elastic SIEM instance and navigate to the Integrations page by: clicking on the Kibana main menu bar at the top left, then selecting “Integrations” at the bottom.

2. Search for “Elastic Defend” and click on it to open the integration page.


3. Click on “Install Elastic Defend” and follow the instructions provided on the integration page to install the agent on your Kali VM.

Make sure “Linux” is selected and then copy that command to your clipboard.

4. Paste that command into the Kali terminal (command line).

5. Once the agent is installed, which can take a few minutes, you’ll see a message that says “Elastic Agent has been successfully installed.” It will automatically start collecting and forwarding logs to your Elastic SIEM instance, although it might take a few minutes for the logs to appear in the SIEM.


You can verify that the agent has been installed correctly by running this command: sudo systemctl status elastic-agent.service


If you get an error installing the agent, make sure that your Kali is connected to the internet before proceeding by pinging google.com.

## Task 4: Generating Security Events on the Kali VM
1. To verify that the agent is working correctly, you can generate some security-related events on your Kali VM. To do this, we can use a tool like Nmap. Nmap (Network Mapper) is a free and open-source utility used for network exploration, management, and security auditing. It is designed to discover hosts and services on a computer network, thus creating a “map” of the network. Nmap can be used to scan hosts for open ports, determine the operating system and software running on the target system, and gather other information about the network.

### To run an Nmap scan, follow these steps:

1. Install Nmap on the Linux VM if you’re not using Kali, Nmap already comes preinstalled in Kali. Open a new Terminal and run this command to install it: sudo apt-get install nmap.
2. Run a scan on Kali machine by running the command: sudo nmap <vm-ip>. You can also run a scan of your host machine if you place your Kali VM on a “bridged” network.

An Nmap scan of my host machine.
3. This scan generates several security events, such as the detection of open ports and the identification of services running on those ports. Run a few more Nmap scans (“nmap -sS <ip address>”, “nmap -sT <ip address>”, “nmap -p- <ip address>”etc..”


## Task 5: Querying for Security Events in the Elastic SIEM
Now that we have forwarded data from the Kali VM to the SIEM, we can start querying and analyzing the logs in the SIEM.

### To do this, follow these steps:

Inside your Elastic Deployment, click on the menu icon at the top-left with the three horizontal lines and then click on the “Logs” tab under “Observability” to view the logs from the Kali VM.

2. In the search bar, enter a search query to filter the logs. For example, to search for all logs related to Nmap scans, enter the query: event.action:
“nmap_scan” or process.args: “sudo”.

3. Click on the “Search” button to execute the search query.

But please note that it can sometimes take a while for the events to populate and show up on the SIEM, so this query might not work right away.

4. The results of the search query will be displayed in the table below. You can click on the three dots next to each event to view more details.



By generating and analyzing different types of security events in Elastic SIEM like the one above, or generating authentication failures by typing in the wrong password for a user or attempting SSH logins an incorrect password, you can gain a better understanding of how security incidents are detected, investigated, and responded to in real-world environments.

## Task 6: Create a Dashboard to Visualize the Events
You can also use the visualizations and dashboards in the SIEM app to analyze the logs and identify patterns or anomalies in the data. For example, you can create a simple dashboard that shows a count of security events over time.

### Here’s how you can do that:

1. Navigate to the Elastic web portal at https://cloud.elastic.co/.
2. Click on the menu icon on the top-left, then under “Analytics,” click on “Dashboards.”

3. Click on the “Create dashboard” button on the top right to create a new dashboard.

4. Click on the “Create Visualization” button to add a new visualization to the dashboard.

4. Select “Area” or “Line” as the visualization type, depending on your preference. This will create a chart that shows the count of events over time.


6. In the “Metrics” section of the visualization editor on the right, select “Count” as the vertical field type and “Timestamp” for the horizontal field. This will show the count of events over time.



Click “close” once you’re done.

Click “close” once you’re done.
7. Click on the “Save” button to save the visualization and then complete the rest of the settings.


## Task 7: Create an Alert
In a SIEM, alerts are a crucial feature for detecting security incidents and responding to them in a timely manner. Alerts are created based on predefined rules or custom queries, and can be configured to trigger specific actions when certain conditions are met. In this task, we will walk through the steps of creating an alert in the Elastic SIEM instance to detect Nmap scans. By following these steps, you can create an alert that will monitor your logs for Nmap scan events and then notify you when they are detected.

### Here are the steps:

1. Click on the menu icon on the top-left, then under “Security,” click on “Alerts.”
2. Click on “Manage rules” at the top right.

3. Click on the “Create new rule” button at the top right.

4. Under the “Define rule” section, select the “Custom query” option from the dropdown menu.

5. Under “Custom query,” set the conditions for the rule. You can use the following query to detect Nmap scan events.


This query will match all events with the action “nmap_scan.” Then click “Continue.”

6. Under the “About rule” section, give your rule a name and a description (Nmap Scan Detection).

7. Set the severity level for the alert, which can help you prioritize alerts based on their importance. Keep all the other default settings under “Schedule rule” and click “Continue.”


8. In the “Actions” section, select the action you want to take when the rule is triggered. You can choose to send an email notification, create a Slack message, or trigger a custom webhook.

9. Finally, click the “Create and enable rule” button to create the alert.


Once you’ve created the alert, it will monitor your logs for Nmap scan events. If an Nmap scan event is detected, the alert will be triggered and the selected action will be taken. You can view and manage your alerts on the “Alerts” section under “Security.”

## Conclusion
- In this guide, we have set up a home lab using Elastic SIEM and a Kali VM. We forwarded data from the Kali VM to the SIEM using the Elastic Beats agent, generated security events on the Kali VM using Nmap, and queried and analyzed the logs in the SIEM using the Elastic web interface. We also created a dashboard to visualize security events and then created an alert to detect security events.

- This home lab provides a valuable environment for learning and practicing the skills necessary for effective security monitoring and incident response using Elastic SIEM. By following these steps, you can gain hands-on experience with using a SIEM and improve your security monitoring skills to help you become a successful security analyst or engineer.

#### Next Steps
- Try generating different types of security events on your Kali VM and then querying for them in the Elastic SIEM.
Test the alert that you created by generating nmap scans on the Kali VM.
Learn how to use the different analysis and visualization tools provided by Elastic SIEM to better understand and analyze your security logs. The more familiar you are with these tools, the more effective you’ll be at detecting and responding to security threats.
- Explore different types of integrations and data sources available for Elastic SIEM, such as integrating with cloud providers like AWS or Azure, and collecting data from different log sources like Windows event logs or syslog. This will help you expand your knowledge of Elastic SIEM and make it more effective for your specific use case.
Thank you for taking the time to read this repository post. I hope you found it informative and gained some valuable insights that you can apply.  

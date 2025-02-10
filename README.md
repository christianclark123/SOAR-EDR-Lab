# SOAR-EDR-Lab

## Objective

This SOAR & EDR Automation Lab focuses on integrating LimaCharlie as an Endpoint Detection and Response (EDR) solution and Tines as a Security Orchestration, Automation, and Response (SOAR) platform. The lab involves detecting a password recovery tool on a virtual Windows machine using LimaCharlie and triggering an automated response via Tines. The response includes sending notifications via email and Slack, and asking the user whether to isolate the machine. If the user confirms, the machine will be automatically isolated. This lab showcases advanced incident detection and automated response workflows in a modern security environment.

### Skills Learned

- Proficiency in configuring LimaCharlie to detect specific malicious activity.
- Creation and fine-tuning of automated Tines playbooks (stories) for incident response.
- Integration of EDR detections with SOAR platforms for real-time automation.
- Implementation of multi-channel notifications (email, Slack).
- Hands-on experience with endpoint isolation workflows.
- Enhanced knowledge of incident detection and automated response processes.
- Understanding the collaboration between EDR and SOAR tools in cybersecurity operations.

### Tools Used

- Tines – Creating automated playbooks (stories) for incident response.
- LimaCharlie – Detecting endpoint activity and responding to threats.
- Virtual Windows Machine – Simulated environment for testing detections and responses.
- Email and Slack Integrations – For incident notifications and user interaction.
- REST APIs – For communication between Tines and LimaCharlie.
- PowerShell and CMD – For testing and generating simulated detections.
- Password Recovery Tool – Used as a simulated threat for testing detections.

## Steps

Create a Playbook Workflow using <a href="https://app.diagrams.net/">Draw.io</a>

Playbook Objectives **NOTE** Changes *can* and *will* happen
-   Send a Slack Message
-   Send an Email (Containing information about the detection)
-   Generate a user prompt (Isolate the machine? Yes/No) If YES - Isolate

The Playbook (Story) will look like the following that are following the objectives we set out for this lab.

![image](https://github.com/user-attachments/assets/33d15155-8eeb-4355-b1ea-b58ef46b9ff2)

Save your workflow diagram to your computer for reference throughout the lab.

For this lab, we will be utilizing a <a href="https://github.com/christianclark123/Active-Directory/tree/main">Windows Server 2022</a> VM that we previously configured with Active Directory. 

Sign up for an account on <a href="https://app.limacharlie.io/login">LimaCharlie</a>

Once you've signed up Create an Organization 

![image](https://github.com/user-attachments/assets/3323b87e-450b-4288-9d59-acac2080e057)

On the main screen on the left tab go to Installation Keys and Create Installation Key

![image](https://github.com/user-attachments/assets/e3e2d395-49ea-4a57-ade0-8fbc1d72023a)
![image](https://github.com/user-attachments/assets/0758c476-4249-4376-8ab9-cc37cd84ef3d)

Delete the other Installation Keys that are already on LimaCharlie.

![image](https://github.com/user-attachments/assets/bd501ab9-66ba-4e5c-95b3-fa22328272fa)

Log onto your Windows 2022 Server and on the Installation keys page scroll down to Sensor Downloads and download Windows 64 bit.

![image](https://github.com/user-attachments/assets/985b3366-7dd6-49ae-bcb4-57f2cde60bea)

For Installation on Windows EXE it requires your installation key so copy your sensor key.

![image](https://github.com/user-attachments/assets/84f06593-3239-4f05-8a8a-c85858b12e8b)

Open a  powershell with Administrator rights (Should already be logged on as Admin)

Chane your directory to downloads "cd Downloads".

Type "dir" to and you should sere the hcp file downloaded. 

![image](https://github.com/user-attachments/assets/a51cf9cf-2846-412f-9900-89d0ff8b4243)

Type in hcp and tab to auto complete -i and paste your sensor key and hit enter.

![image](https://github.com/user-attachments/assets/a040ee21-837e-4627-9747-1273a730756f)

![image](https://github.com/user-attachments/assets/02878295-5581-4fde-82f1-9933ca373bb3)

Your LimaCharlie Agent should successfully install.

![image](https://github.com/user-attachments/assets/d7d2227f-adc4-4d8d-ae8e-bb638d1c21bd)

To verify the service is running type Services into the Windows search bar and look for LimaCharlie.

![image](https://github.com/user-attachments/assets/44de820a-b760-48f1-b95a-25d301d8ee9b)

Minimize your VM and head back to the LimaCharlie website and under Sensors List you should see your VM computer under Hostname.

![image](https://github.com/user-attachments/assets/b997207e-6859-4423-806e-1d9f58c881d5)

Clicking on it you can see Sensor details for your VM.

![image](https://github.com/user-attachments/assets/94d22084-7de5-4f31-a5da-18afaddb20d6)

Event collection gives a list of event collection rules that are collected from your server. 

![image](https://github.com/user-attachments/assets/5506669e-8e09-40ee-a129-78ff54a3e00f)

LimaCharlie has the ability to access the file system of the sensor server remotely. It gives various options such as Inspect File Hash, Copy File Path, Move File, Delete File, and Download the File. 

![image](https://github.com/user-attachments/assets/ec2d1d37-739a-4dfa-a552-0d131cb94a9c)

LimaCharlie has a Live Feed where each event that is generated is sent over for review.

![image](https://github.com/user-attachments/assets/4058401b-515e-4fca-b77b-ccac92cb6a83)

In the Timeline section, you can see all events for the server sensor. For this server, I can see I'm getting DNS requests from different domains and IP addresses. In a real-world scenario you can click on each event and investigate. 

![image](https://github.com/user-attachments/assets/1020a705-f9c4-4c83-860b-0cd792d1d436)

Go to <a href="https://github.com/AlessandroZ/LaZagne/releases">LaZagne github releases</a> choose the latest release. 

Download the LaZagne.exe

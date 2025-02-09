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


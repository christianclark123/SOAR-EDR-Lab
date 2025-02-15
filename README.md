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

In the Timeline section, you can see all events for the server sensor. I can see I'm getting DNS requests from different domains and IP addresses for this server. In a real-world scenario, you can click on each event and investigate. 

![image](https://github.com/user-attachments/assets/1020a705-f9c4-4c83-860b-0cd792d1d436)

Go to <a href="https://github.com/AlessandroZ/LaZagne/releases">LaZagne GitHub releases</a> and choose the latest release. 

**NOTE** We will be using Lazagne to generate telemetry. Lazagne is an open-source password recovery tool for local computers. 

Download the LaZagne.exe

You might receive a notification that the downloaded was blocked.

![image](https://github.com/user-attachments/assets/a4e168e0-60d1-42a2-b13d-e171168652a3)

Search Windows Security in the Windows search.

Select Virus & Threat Protection. Under the protection settings click Manage settings and turn Real-time protection OFF.

![image](https://github.com/user-attachments/assets/e5ee3510-9d97-470b-ae69-396b551b4615)

Click on the LaZagne.exe download three dots.

![image](https://github.com/user-attachments/assets/f5f5ca4d-72f6-4772-9c67-f7fec06ca39a)

Press Keep.

![image](https://github.com/user-attachments/assets/c420cbbd-120f-40aa-9435-2c12abfa97c5)

A popup will appear saying, "This app is unsafe." Click Show More and press Keep Anyway. 

![image](https://github.com/user-attachments/assets/f4314dcb-eebf-454b-97b3-99d020ea52c6)

You should now see the download in your Downloads folder.

![image](https://github.com/user-attachments/assets/7b2dd43c-f9e9-4066-94ca-cbd2c71f0d25)

In the Downloads window of your File Explorer Shift+Right Click and Open PowerShell window here.

Type .\ and tab to complete the .\LaZagne.exe and press enter.

Go to your LimaCharlie -> Sensors -> Timeline -> Search Lazagne. You should see that LimaCharlie has picked up events pertaining to LaZagne. Clicking on each event gives you details such as is the file signed, the hash, parent process ID, process ID, and the username that the event was executed on.

![image](https://github.com/user-attachments/assets/7b3dcf86-d1d7-42c0-894f-5d13c321c65f)


![image](https://github.com/user-attachments/assets/0f6ee5af-0848-4efc-878f-2eb72e69a00c)

In Powershell type in ".\LaZagne.exe all" command. This will create new event data that we will construct our rule on.

![image](https://github.com/user-attachments/assets/cc665c8d-718c-4ff2-b7a0-92f49cf8699c)

![image](https://github.com/user-attachments/assets/07d3cfbd-d27c-4a07-a4cf-2f331cf0df3c)

Open up a new tab in LimaCharlie and go to your organization. Select Automation -> D&R Rules (Detection & Response Rules). 

![image](https://github.com/user-attachments/assets/25ac20b4-92d8-49ba-bd47-013543bc5dd3)

Our new detection rule will have the following parameters:
  
  - The Event type will be New_Process or Existing Process
  
  - FILE_PATH ends with LaZagne.exe
  
  - COMMAND_LINE contains LaZagne
 
  - HASH is the hash created from the all event

<a href="https://raw.githubusercontent.com/christianclark123/Detection-Rule-/refs/heads/main/README.md">Here</a> is the created Detect and Respond rule template that follows the parameters above. Be sure to change the hash to your created events hash and the Respond portion. 

On the Detect and Respone Rules hit the + New Rule at the top. Paste in the information from the template link above.

![image](https://github.com/user-attachments/assets/363bacd7-be68-4d85-8342-f51b3b754327)

![image](https://github.com/user-attachments/assets/eab5f7d4-a2ef-40c7-ad8a-5f021642faa2)

When you put in the information for your Detect & Respond name your rule and press Create.

![image](https://github.com/user-attachments/assets/6d8e9632-a2cc-4cd6-9f31-42a21d82753c)

LimaCharlie has the capability to test the new rule. Head to your New_Process that was created when you executed the command and copy the whole event.

![image](https://github.com/user-attachments/assets/69cbd9ae-989d-4c0c-b80d-2d1dff0ed317)

On your new rule scroll down under Advanced click "Target Event" paste your whole event and press "Test Event".

![image](https://github.com/user-attachments/assets/d1eb69bb-9b26-4de3-9d95-e684f3a04848)

You should see a green "Match" where it goes through and tests each parameter to see if it is met. 

![image](https://github.com/user-attachments/assets/f5accfbd-6c56-4998-a592-8976e9957548)

Head back to your dashboard and go to the Detections tab. There should be no detections yet, because we have not ran the command again since creating the new rule.

![image](https://github.com/user-attachments/assets/0431e1cf-e6c9-49b2-81d5-2a8c9b6a67d0)

Back to your VM server, clear your PowerShell and run the command ".\LaZagne.exe all" again.

Refresh your Detections page on LimaCharlie and you should see the new detection rule that we implemented. 

![image](https://github.com/user-attachments/assets/3048656a-566c-4fdb-aac2-a854c0198d08)

Clicking on the detection gives more information. It includes all the parameters we listed in the rule above. LimaCharlie gives you the option to View the Event Timeline and mark it as a False Positive which is good for eliminating false positives from your detection making your detections stronger and more precise. 

![image](https://github.com/user-attachments/assets/6648d87d-3bcb-43a9-86f1-c6ddab032f6b)

Next we will set up <a href="https://slack.com/">Slack</a>. Sign up for a free account and make sure to use a valid email as they will send a confirmation code.

Create a Workspace, name it what you would like. 

![image](https://github.com/user-attachments/assets/505e118e-ccc7-4b52-a1f1-e3057755ed97)

![image](https://github.com/user-attachments/assets/696574e9-7dd6-4134-a213-d508ca6c3b4d)

Go through the creation process. Start with Limited Free Version

![image](https://github.com/user-attachments/assets/b132dae6-1280-41ce-99ca-9a547d69415f)

Add a new channel and name it alerts. Make sure it is public. Going back to our Playbook Workflow chart this will receieve an alert from Tines in that alert channel of a detection found from LimaCharlie.

![image](https://github.com/user-attachments/assets/fad29f41-a899-4d74-b822-4be374c8181f)

Go to <a href="https://www.tines.com/">Tines</a> and sign up for free account. Use a valid email Tines will send a sign in link.

![image](https://github.com/user-attachments/assets/ecef5fa1-72f5-42a4-93fa-f5919e145b2b)

We will establish the link between LimaCharlie and Tines. Delete the Weather and Email from your storyboard to make a blank story.

![image](https://github.com/user-attachments/assets/0084eeab-22c6-4301-896f-daaaf6e1d4b3)

Drag the Webhook action to your storyboard, name it Retreieve Detections and add a description. Copy the Webhook URL.

![image](https://github.com/user-attachments/assets/449dcabc-3cda-4cbc-842d-208f66aa7b8e)

Go back to LimaCharlie -> Click your organization -> Outputs

![image](https://github.com/user-attachments/assets/f89feb4f-6ee4-4b5a-ab49-d5a1b3fd6165)

Click Add Output -> Detections -> Tines (If there was no application for Tines you could select the Webhook application)

![image](https://github.com/user-attachments/assets/e347c9ae-3fed-4890-94fe-deb21fb698ce)

![image](https://github.com/user-attachments/assets/3665e825-5af9-426d-be35-bd7202848593)

Name the output and past in your Webhook URL that you copied from Tines and click Save Output.

![image](https://github.com/user-attachments/assets/342c2418-566b-42fa-b1bf-aa16de3cb0c3)

Once the configuration saves, it will say it couldn't detect any recent samples moving through this output because we have not ran the command.

On your VM run the command again ".\LaZagne.exe". That should generate a detection event. Click the Refresh Samples on your output configuration

![image](https://github.com/user-attachments/assets/bde955cc-c3c5-4402-ad2e-26178d2d6434)

An event should appear. 

![image](https://github.com/user-attachments/assets/bab2eef4-4b3e-47cb-a7a7-f996063345fe)

Back on Tines click on your Webhook and an event should have generated. Expand each ellipses and it should be a copy of the detection event.

![image](https://github.com/user-attachments/assets/68f34ade-cbb8-4574-ab87-a3ff0567de39)


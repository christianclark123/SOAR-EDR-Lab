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

Open a  PowerShell with Administrator rights (Should already be logged on as Admin)

Change your directory to downloads "cd Downloads".

Type "dir" to and you should see the hcp file downloaded. 

![image](https://github.com/user-attachments/assets/a51cf9cf-2846-412f-9900-89d0ff8b4243)

Type in hcp and tab to auto-complete -i and paste your sensor key and hit enter.

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

LimaCharlie can access the file system of the sensor server remotely. It gives various options such as Inspect File Hash, Copy File Path, Move File, Delete File, and Download the File. 

![image](https://github.com/user-attachments/assets/ec2d1d37-739a-4dfa-a552-0d131cb94a9c)

LimaCharlie has a Live Feed where each event that is generated is sent over for review.

![image](https://github.com/user-attachments/assets/4058401b-515e-4fca-b77b-ccac92cb6a83)

In the Timeline section, you can see all events for the server sensor. I can see I'm getting DNS requests from different domains and IP addresses for this server. In a real-world scenario, you can click on each event and investigate. 

![image](https://github.com/user-attachments/assets/1020a705-f9c4-4c83-860b-0cd792d1d436)

Go to <a href="https://github.com/AlessandroZ/LaZagne/releases">LaZagne GitHub releases</a> and choose the latest release. 

**NOTE** We will be using Lazagne to generate telemetry. Lazagne is an open-source password recovery tool for local computers. 

Download the LaZagne.exe

You might receive a notification that the download was blocked.

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

On the Detect and Response Rules hit the + New Rule at the top. Paste in the information from the template link above.

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

Head back to your dashboard and go to the Detections tab. There should be no detections yet because we have not ran the command again since creating the new rule.

![image](https://github.com/user-attachments/assets/0431e1cf-e6c9-49b2-81d5-2a8c9b6a67d0)

Back to your VM server, clear your PowerShell, and run the command ".\LaZagne.exe all" again.

Refresh your Detections page on LimaCharlie and you should see the new detection rule that we implemented. 

![image](https://github.com/user-attachments/assets/3048656a-566c-4fdb-aac2-a854c0198d08)

Clicking on the detection gives more information. It includes all the parameters we listed in the rule above. LimaCharlie gives you the option to View the Event Timeline and mark it as a False Positive which is good for eliminating false positives from your detection making your detections stronger and more precise. 

![image](https://github.com/user-attachments/assets/6648d87d-3bcb-43a9-86f1-c6ddab032f6b)

Next we will set up <a href="https://slack.com/">Slack</a>. Sign up for a free account and make sure to use a valid email as they will send a confirmation code.

Create a Workspace, name it what you would like. 

![image](https://github.com/user-attachments/assets/505e118e-ccc7-4b52-a1f1-e3057755ed97)

![image](https://github.com/user-attachments/assets/696574e9-7dd6-4134-a213-d508ca6c3b4d)

Go through the creation process. Start with the Limited Free Version

![image](https://github.com/user-attachments/assets/b132dae6-1280-41ce-99ca-9a547d69415f)

Add a new channel and name it alerts. Make sure it is public. Going back to our Playbook Workflow chart this will receive an alert from Tines in that alert channel of a detection found from LimaCharlie.

![image](https://github.com/user-attachments/assets/fad29f41-a899-4d74-b822-4be374c8181f)

Go to <a href="https://www.tines.com/">Tines</a> and sign up for a free account. Use a valid email Tines will send a sign-in link.

![image](https://github.com/user-attachments/assets/ecef5fa1-72f5-42a4-93fa-f5919e145b2b)

We will establish the link between LimaCharlie and Tines. Delete the Weather and Email from your storyboard to make a blank story.

![image](https://github.com/user-attachments/assets/0084eeab-22c6-4301-896f-daaaf6e1d4b3)

Drag the Webhook action to your storyboard, name it Retreieve Detections, and add a description. Copy the Webhook URL.

![image](https://github.com/user-attachments/assets/449dcabc-3cda-4cbc-842d-208f66aa7b8e)

Go back to LimaCharlie -> Click your organization -> Outputs

![image](https://github.com/user-attachments/assets/f89feb4f-6ee4-4b5a-ab49-d5a1b3fd6165)

Click Add Output -> Detections -> Tines (If there was no application for Tines you could select the Webhook application)

![image](https://github.com/user-attachments/assets/e347c9ae-3fed-4890-94fe-deb21fb698ce)

![image](https://github.com/user-attachments/assets/3665e825-5af9-426d-be35-bd7202848593)

Name the output and paste in your Webhook URL that you copied from Tines and click Save Output.

![image](https://github.com/user-attachments/assets/342c2418-566b-42fa-b1bf-aa16de3cb0c3)

Once the configuration saves, it will say it couldn't detect any recent samples moving through this output because we have not run the command.

On your VM run the command again ".\LaZagne.exe". That should generate a detection event. Click the Refresh Samples on your output configuration

![image](https://github.com/user-attachments/assets/bde955cc-c3c5-4402-ad2e-26178d2d6434)

An event should appear. 

![image](https://github.com/user-attachments/assets/bab2eef4-4b3e-47cb-a7a7-f996063345fe)

Back on Tines click on your Webhook and an event should have generated. Expand each ellipse and it should be a copy of the detection event.

![image](https://github.com/user-attachments/assets/68f34ade-cbb8-4574-ab87-a3ff0567de39)

Now we are going to connect Slack and Tines. Head to your <a href="slack.com">Slack</a> and click "...More" -> Automations -> Apps -> Search Tines. Press Add and it will redirect you to a new webpage where you should hit the "Add to Slack" button. 

![image](https://github.com/user-attachments/assets/22369992-60f2-40a9-9c4a-824315c197f9)

![image](https://github.com/user-attachments/assets/b4cc4404-e16a-432b-a3ec-9858bde90395)

![image](https://github.com/user-attachments/assets/58ce8630-8c43-417c-9ade-15d937281660)

Follow the steps for installing the app on Tines. 

![image](https://github.com/user-attachments/assets/8e6985ae-2eaa-4973-bc30-cee3deace8d9)

Open up a new dashboard for Tines, in the left-hand corner open up the dropdown menu for Personal and click Credentials. Press the +

![image](https://github.com/user-attachments/assets/2946a129-6073-43ae-b453-9b03daca9ace)

![image](https://github.com/user-attachments/assets/2d2cd459-ef80-4860-af24-6117f02c659e)

Choose Slack -> Use Tine's app for Slack -> Click Allow -> This will create a new Slack credential. This establishes the link between Tines and Slack.

![image](https://github.com/user-attachments/assets/06e5291c-2434-4ad5-a9d1-de5706e7b137)

![image](https://github.com/user-attachments/assets/88d2b97a-3ca3-4f46-bb5d-de8702eda9e1)

Back on your Story, add the Slack template to your StoryBoard. 

![image](https://github.com/user-attachments/assets/0873038d-9499-4a56-9c81-4eed442232bf)

![image](https://github.com/user-attachments/assets/e0803874-5b79-4679-b5a2-25dcbf4d2137)

Click on your story pin for Slack and search in the right hand menu "Send a Message". We want to send the messages to our Slack #alert channel we created earlier. 

On Slack right-click your #alert and click View channel details. At the bottom copy your Channel ID.

![image](https://github.com/user-attachments/assets/f2757455-c02b-4724-9fe7-cbe77b9ea514)

On Tines paste that Channel ID to the Channel / User ID section. Write a temporary message so we can check to see if it works. On the Slack story post on your board click Test and you should receive a message in your #alert channel of Slack. 

![image](https://github.com/user-attachments/assets/a7f0dc28-7c12-4520-86e8-3cdf87feb946)

![image](https://github.com/user-attachments/assets/7f008335-e1cb-46d4-a3e3-0a35e94f6b39)

Back on Tines, connect the event Webhook to Slack.

![image](https://github.com/user-attachments/assets/8374068e-af60-40fa-bbf0-fb59fd4af880)

On our Playbook workflow we created we are also going to send an email. To do that on the left menu of Tines there is a "Send Email" story pin. Drag that to your Storyboard. Connect your Webhook to the email story pin. 

![image](https://github.com/user-attachments/assets/599b51b9-0cd2-49f6-930f-895aa05f76ef)

![image](https://github.com/user-attachments/assets/d5ffe1b0-b486-4e4e-88d5-5376dd99d57e)

You can use your email but I will be utilizing a <a href="https://public.sqrx.com/web/">Disposable Email</a>. Copy the email into the Tines email Recipients and Reply to section. 

![image](https://github.com/user-attachments/assets/ac5c402c-969d-4181-a0fa-7f67bf0899b3)

Change the Sender name to Alerts and the Subject to Test for now. 

![image](https://github.com/user-attachments/assets/e3df460e-36ba-4f25-ad2b-aeea98b6bcbb)

Test the email by clicking on Test from the Send Email Pin. Choose an event and click Test

![image](https://github.com/user-attachments/assets/ac7d7074-4ea5-4fbd-b114-d9a6c1d74a69)

![image](https://github.com/user-attachments/assets/4ac14fd2-1155-42e5-889a-ebb2f9fbf214)

You should receive an email like the pictures below. 

![image](https://github.com/user-attachments/assets/57123d58-2add-49fd-a111-8faf7a404b83)

![image](https://github.com/user-attachments/assets/b3dc72be-1d45-4957-99f3-b403dfaf7f7f)

Now that we have set up email and Slack notifications we will set up the User Prompt from our Playbook.

On Tines in the left menu click Tools and add a new page to your storyboard. 

![image](https://github.com/user-attachments/assets/a960ca23-a7af-4ab2-a260-8db5f082d9e9)

Name -> User Prompt. Description -> Isolate Computer (Yes/No). Access Control -> Members of this Tines tenant.

![image](https://github.com/user-attachments/assets/a49b3671-0eb0-4756-80fa-3a854d967a73)

Anonymize -> Off. Page behavior -> Show success message. Success message -> Thank you, you can now close this window... 

![image](https://github.com/user-attachments/assets/67d3e737-0dab-4aa3-a831-7bfd19d0f0ac)

Link the Webhook to your User Prompt. 

![image](https://github.com/user-attachments/assets/bf1a8aeb-86e7-4c51-86cc-e7b6ab615a29)

Click your User prompt and hit Edit Page. 

![image](https://github.com/user-attachments/assets/345726b4-8315-4b46-8146-3ded8cadab68)

Add a heading with your project name, rich text that asks "Do you want to isolate the machine?", and a boolean that reads "Isolate?" We will be replacing the rich text with the message we created in our Playbook. 

![image](https://github.com/user-attachments/assets/1be1799c-a3a3-493e-93ce-ea4719faae49)

Head to your events on the Webhook, we will use the names of the events for our message.

![image](https://github.com/user-attachments/assets/1e20ffbf-ba0c-4cc0-a916-02adfea97bc5)

Open up a notepad to record what each part will be. Copy from the actual event, it will give the source path of the event making it easier for the message. 

![image](https://github.com/user-attachments/assets/acadb688-b479-443b-a4c7-f50c25ae4fd8)

Copy the contents and go to your Slack Message story pin and paste your variables in the Message portion. 

![image](https://github.com/user-attachments/assets/8917590f-d320-45b1-9ff2-e4a23e9f3371)

Run a test on the Slack story pin, and head to Slack to see if it worked.

![image](https://github.com/user-attachments/assets/0492948c-3aaa-445b-83f5-d93323974bba)

In order to make it more user friendly we will add descriptions to each value. Add it to your Notepad and remove the values from the message on Tines and paste in the new values with descriptions.

![image](https://github.com/user-attachments/assets/e304e052-7bcc-48af-a24a-89caae23ea65)

Run another test to make sure it is updated. 

![image](https://github.com/user-attachments/assets/ecb116a5-cda4-438d-9e17-d0593a5c20cc)

If you click on the detection link it brings you right to your LimaCharlie event Timeline.

![image](https://github.com/user-attachments/assets/cf5a06db-b3f7-4ca1-bff0-c79c065e55dc)

Paste the same values with the description into the body portion of your email story pin. Run a test and check your email. 

![image](https://github.com/user-attachments/assets/133ba4a7-0a44-4c8d-8329-84733cd9dfa2)

It works but the format for email is off. To fix that we will open the body in the HTML editor. 

![image](https://github.com/user-attachments/assets/5bb37aab-f66f-4dbb-b9ed-862079625273)

We will use <br> for a break in the text to make the email format read better. 

![image](https://github.com/user-attachments/assets/0edbe193-448a-4532-a5fc-07394c7a58aa)

Run the test again for email and verify the changes. 

![image](https://github.com/user-attachments/assets/10644e9a-2fe7-4e0c-a10c-3bf5ebd0f574)

Back to the User Prompt story pin. Click Edit at the bottom and paste in your values. 

![image](https://github.com/user-attachments/assets/eadb791d-455d-4ba9-974c-895962f2f4e5)

In order to view the page click the arrow at the bottom of your prompt that says Visit Page

![image](https://github.com/user-attachments/assets/3511e9c1-051b-4a2e-847e-9ebadb0de2a3)

![image](https://github.com/user-attachments/assets/b3485e22-e2d3-4813-a795-fdebc8d85c3b)

Now we will build out the "No" portion of our Playbook. Add a trigger from the left-hand menu to the story. Link the user prompt to the trigger. 

Open the settings for the trigger, and name it "No". For the Rules, type in user_prompt.body.isolate and the result should be false.

![image](https://github.com/user-attachments/assets/43bf5cf6-0e45-490c-abf3-1d3e5dc573d5)

![image](https://github.com/user-attachments/assets/70edd866-7e63-4d9a-9109-d117260f0b59)

![image](https://github.com/user-attachments/assets/15829804-0aeb-4d3e-8015-e4aac918b231)

Copy and paste another Slack pin to your storyboard. Remove everything except the Computer: and change the message line to "The computer: (your variable for your hostname) was not isolated, please investigate.

![image](https://github.com/user-attachments/assets/7113bb51-b457-471b-8ff2-eea0e497c521)

Connect the No trigger to the new Slack message pin. 

![image](https://github.com/user-attachments/assets/6f0dd23f-9da0-4fac-afab-684e49c70719)

In order to test this, click on your Webhook events and hit the Re-emit to emit the event again.

![image](https://github.com/user-attachments/assets/d8a2da11-63f5-453e-aaf8-73ee205386d9)

Now on the User Prompt pin click on Visit page and click No and submit. You should get a Slack message telling you the hostname was not isolated and needs investigation. 

![image](https://github.com/user-attachments/assets/52649c3e-3d4c-4dc0-8483-21fced4586ee)

![image](https://github.com/user-attachments/assets/fbb1f279-089c-4161-83b0-b9316c312365)

Re-emit another event and on the User Prompt click Yes. This will give us the field value for "Yes"

Copy the Trigger for No to the storyboard. 

Connect the new trigger pin to the User Prompt. Change the name to Yes, under Rules add a value for user_prompt.body.isolate the value should be true.

![image](https://github.com/user-attachments/assets/aac10165-136e-48ed-bd83-4daca539aa40)

When the user presses Yes, we want to send them to LimaCharlie so choose LimaCharlie from the template. Put it on your story under the Yes trigger. 

Under Build search for Isolate and choose "Isolate Sensor"

![image](https://github.com/user-attachments/assets/9fc8038c-84d2-4915-ae77-64a4b8e817c3)

Connect the Yes trigger to the LimaCharlie template.

Click on the LimaCharlie Isolate Sensor and under URL remove the sid. 

![image](https://github.com/user-attachments/assets/9ea59de1-edef-4068-a716-7e1c810b3b30)

Paste in the value tag you used for your detection routing "<<retrieve_detections.body.detect.routing.sid>>"

![image](https://github.com/user-attachments/assets/f9c8169e-fcc1-40c5-a88b-0b5ae18de1d4)

Re-emit the event so when we change the URL it can detect the sid. You should have a Result:.

![image](https://github.com/user-attachments/assets/458dbc37-0148-4431-ab7f-9e89e2cb51bd)

Even though it received the event it does not have the credentials. Open up a new Tines tab and under Personal click Credentials. 

![image](https://github.com/user-attachments/assets/bfa8eacc-b669-49eb-b033-283ca6cc2b7e)

To get the API for your organization go to LimaCharlie -> Access Management -> Rest API.

![image](https://github.com/user-attachments/assets/7a753084-9852-40b6-aea5-229d55e0881c)

Copy the "Org JWT" API. 

![image](https://github.com/user-attachments/assets/0690754a-3f79-497f-b825-2a9e6e953f3a)

Back on Tines click the +New Credential -> Text

![image](https://github.com/user-attachments/assets/74e2bb93-8cd1-4a43-8bcf-d2f09b3bdd75)

Name it LimaCharlie, for the description put LimaCharlie API, paste in your ORG JWT API key, for the URLS and Domains type in "*.limacharlie.io". 

That gives the credential access to only limacharlie.io and its subdomains. 

Hit Save. 

![image](https://github.com/user-attachments/assets/ae29ae79-53cd-47cd-8d27-c95f560f3cb3)

![image](https://github.com/user-attachments/assets/9a919b20-1d36-4682-ba3e-c6fa99b05b37)

![image](https://github.com/user-attachments/assets/ec92718b-e853-4191-9a9f-7d5ebbb425b9)

Refresh your Tines storyboard webpage, on the right side Connect your lima_charlie credential.

![image](https://github.com/user-attachments/assets/e73b10ab-ad0f-4cc3-a08f-fd5c46d78732)

Now the Isolate Sensor should have the proper credentials to isolate the host.

Right now my host is Allowed Network Access.

![image](https://github.com/user-attachments/assets/609be1e2-db26-4e1f-8416-e3ef13a92763)

Re-emit the Yes prompt event on your User Prompt. It should trigger the Isolation of your host computer. 

![image](https://github.com/user-attachments/assets/a4e0742f-a9db-4a43-bb1d-baad32025834)

![image](https://github.com/user-attachments/assets/06f5af83-2d7f-40c3-9bde-882b9148b972)

![image](https://github.com/user-attachments/assets/650844ad-42e6-4b2d-a78f-164f51180eab)

Refresh your LimaCharlie and it should say the host is Isolated. 

![image](https://github.com/user-attachments/assets/f8fcc33c-8ec3-4b60-af17-70ead7dff733)

On the host VM try to ping google.com and it should fail because it is isolated from the network. 

![image](https://github.com/user-attachments/assets/58112021-0969-4e44-b828-26a09496166b)

On LimaCharlie rejoin the host to the network. It should be able to ping. 

![image](https://github.com/user-attachments/assets/25b159cc-4eac-4b13-875f-575411324b6c)

Now that the automation is completed we will configure a message to send to Slack of the isolation status and that the host is isolated.

Put a LimaCharlie template under the Isolate Sensor and search for Get Isolation Status.

![image](https://github.com/user-attachments/assets/7709a42e-79f1-4775-afb2-d2f7a4d3d8a4)

Copy the sid to your URL as we did for the Isolate sensor.

![image](https://github.com/user-attachments/assets/1b7a535f-eaf1-43c2-b627-62be044ff1a1)

Connect the new lima_charlie credential and select your LimaCharlie credential.

![image](https://github.com/user-attachments/assets/903ca3e2-57a6-4252-b550-a9cea8c96e29)

Copy the Slack message from the No trigger. Connect the LimaCharlie Isolate Sensor to the new Slack message.

![image](https://github.com/user-attachments/assets/f6e67881-63cf-4648-96c6-09a8b875d566)

Change the message to read "Isolation Status:<<get_isolation_status>>The computer: <<retrieve_detections.body.detect.routing.hostname>>has been isolated."

Re-emit the playbook from the User Prompt to give results to the new Get Isolation Status process.

![image](https://github.com/user-attachments/assets/2ca278ad-70b7-4ddb-9fb7-964ddeb65f2c)

The message for the isolation status was a bit to convoluted. In the Slack message change the value to "get_isolation_status.body.is_isolated"

![image](https://github.com/user-attachments/assets/7c7ecd26-8ce1-4f46-a3f2-4b31b9cb3da9)

In order to add a Isolation User Prompt link to email and Slack connect both the first email and Slack to the User Prompt.

![image](https://github.com/user-attachments/assets/40f37124-8e9b-43bd-935a-c1a4488c1650)

Add a value under the messages for both Slack and Email that reads Isolation Prompt: PAGE.user_prompt. For the email make sure to include the <br> breaks. 

![image](https://github.com/user-attachments/assets/e2704d80-76d6-463f-803b-d553dfa275b2)

![image](https://github.com/user-attachments/assets/5266131f-2309-461f-9049-2ffb90b0eb7f)

Now to test the whole Playbook. Re-emit the event from the Webhook. You should receive a Slack message and an Email letting you know the details of the event. When you go to the User Prompt and hit yes you should receive a message that the host isolation status is true and that it has been isolated.

"Yes" User Prompt:

![image](https://github.com/user-attachments/assets/7383bef0-7943-4e97-9e13-9412e84e74b3)

![image](https://github.com/user-attachments/assets/3454333f-b7f3-4942-ac39-3bb714cba65e)

![image](https://github.com/user-attachments/assets/ce84c8b1-2f49-4cdf-b506-107f99be2790)

![image](https://github.com/user-attachments/assets/8e8e7646-3234-41ac-9e4a-3c1aa355cfd2)

"No" User Prompt:

![image](https://github.com/user-attachments/assets/96fa662a-58d3-4f9b-a948-68457602e03b)

![image](https://github.com/user-attachments/assets/9f013fc4-99d2-442f-bd19-2db9d50749ae)

END OF PROJECT

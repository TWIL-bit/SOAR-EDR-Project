<h1>SOAR / EDR with Automation: Detecting Malware & Automating Alerts for Reveiw</h1>


<h2>Description</h2>
Today I completed a SOAR / EDR project. This is my first time getting direct experience with both system types and I learned a lot of valuable information. The EDR that I will be working with is LimaCharlie and I will be using the SOAR system Tines. Going through this project has allowed me to get direct experience with creating detection and response rules, as well as creating a playbook for automation.
<br />


<h2>Languages and Utilities Used</h2>

- <b>Virtual Box</b>
- <b>Lima Charlie</b>
- <b>Slack</b> 
- <b>Tines</b> 


<h2>Environments Used </h2>

- <b>Windows 10 Pro</b>




<h2>Project walk-through:</h2>

<p>
I started by creating an account with LimaCharlie and installing the agent on my windows 10 machine. LimaCharlie is an open source SecOps platform that provides Security Operations capabilities as a service, and it can be used to detect malicious activity on your network based on defined rules.
<br> </br>
  <img width="950" height="709" alt="image" src="https://github.com/user-attachments/assets/6689828f-c7e3-4138-99eb-2dc435e72d2c" />

 
Now it’s time to generate some telemetry and create a detection and response rule. I am using LaZagne for this project. LaZagne is an open-source recovery tool, and it is used to extract passwords from software and operating systems. I downloaded the file from LaZagne’s GitHub, and it was initially blocked by Windows Defender. Since LaZagne is known to be a Trojan by many antivirus vendors, it will be blocked when trying to download it. For the sake of this project, I disabled the real-time detection in Windows Defender on my VM so I can download the file. I then executed the file in PowerShell to ensure that it was working, and the telemetry is being tracked by LimaCharlie.
 
<img width="975" height="369" alt="image" src="https://github.com/user-attachments/assets/856ad3ae-f90e-4a28-99ca-5af6f97d67ed" />
<img width="975" height="314" alt="image" src="https://github.com/user-attachments/assets/6dfc8352-56ef-4d43-8647-df6ba421f130" />
 <br> </br>

After verifying telemetry, I created a detection and response rule to detect new and existing processes related to LaZagne. I also defined the rules within it to make the detection more specific like ensuring the process is a windows process and contains the LaZagne value in the file path. This is my first time creating a D&R rule and I now understand that the more information you provide to the rule, the more specific it will be when detecting actions in the telemetry. This can reduce false positives. I was able to test the rule afterwards by copying one of the LaZagne events from the timeline in LimaCharlie into the target event under my created rule and it is working properly.

 <img width="794" height="453" alt="image" src="https://github.com/user-attachments/assets/7eefb0bd-ee27-4a9b-a3a9-90b6661b1384" />
 <img width="827" height="297" alt="image" src="https://github.com/user-attachments/assets/70fd7824-b51a-41d2-86a4-8bc0ba8e9fad" />
 <img width="975" height="299" alt="image" src="https://github.com/user-attachments/assets/1e8dab43-a308-4b0d-b8ad-7c81d0113e6c" />
<br> </br>

Now I can go to the detections tab in LimaCharlie and see some events being detected after creating the rule and running the executable again in PowerShell.

 <img width="975" height="277" alt="image" src="https://github.com/user-attachments/assets/727b1344-318f-4cd7-a8b5-e92d910df445" />
<br> </br>

Since I have telemetry and a D&R rule setup in LimaCharlie, it’s time to set up Tines to enable workflow and automation of alerts related to LaZagne. I created a free Tines account for this project and connected it to my LimaCharlie account by creating an output in LimaCharlie and adding my webhook URL from Tines to the output. 
 <br> </br>

 <img width="975" height="529" alt="image" src="https://github.com/user-attachments/assets/9f6fd41d-43c8-4034-ac8f-cb135a0a551b" />
 <img width="938" height="269" alt="image" src="https://github.com/user-attachments/assets/ddfcb98c-6ba4-4570-bb8e-b0ef82f8bcdd" />
<br> </br>

With Tines and LimaCharlie connected, It’s time to automate the process of sending a Slack message and an email to the Analyst containing information about what was detected. Slack is a collaboration platform that allows you to create channels to share with your team. In order to send a message to my Slack channel, I must first link my Tines account to Slack. I can do this by adding the Slack app to my story/workflow in Tines. Then I created a Slack credential in my Tines story by adding the channel ID of my alerts channel. Now that the two are connected, I sent a test message to make sure that it was received in Slack and it worked properly.
 
 <img width="928" height="624" alt="image" src="https://github.com/user-attachments/assets/8daed32f-2347-4561-9a1a-7b4279b0e462" />
 <img width="613" height="705" alt="image" src="https://github.com/user-attachments/assets/14627fde-1436-4e1c-b612-c446267568c3" />
 <img width="975" height="503" alt="image" src="https://github.com/user-attachments/assets/4eddc31e-0ee9-4ddf-8026-57d8e6ccdc56" />
<br> </br>

Along with receiving a message in Slack, I also configured my story to send an email as well and I listed my own email for the sake of this project. This is my first experience with automation and receiving the first email after configuring the send email action was really cool to see. It came to my personal email with all the information I populated in Tines.

 <img width="975" height="571" alt="image" src="https://github.com/user-attachments/assets/092e1871-6793-4abd-8065-3fe7eb77444e" />
 <img width="975" height="221" alt="image" src="https://github.com/user-attachments/assets/473a9ed6-6e1e-4cb9-8970-4d257d64b0e7" />
 <br> </br>

I want to configure my Slack messages to contain information about the event for the Analyst to see. I was able to do that by copying the path to specific elements in the event based on what information I want to include. A few of the things I included are the file path, host name and source IP.

 <img width="298" height="352" alt="image" src="https://github.com/user-attachments/assets/a12b14c3-e4b6-4554-83c4-a95567aef078" />
 <img width="975" height="509" alt="image" src="https://github.com/user-attachments/assets/e39581e6-73b4-4eb0-af39-7a6c4fe95413" />
<br> </br>

The Slack and email messages are now configured in Tines. The next thing I did was configure a user prompt that will ask the user if they want to isolate the machine. The user prompt page includes information about the alert for the Analyst to see. I set up a Yes and No trigger that will serve distinct functions based on the input from the Analyst. If the user chooses No, a message will be sent to slack stating that the machine was not isolated and that they should investigate. If the user chooses Yes, the machine will be isolated from the network.
 
 <img width="794" height="877" alt="image" src="https://github.com/user-attachments/assets/be0aaba9-6312-4aa0-8ceb-b2b7ccc7753c" />
 <img width="645" height="119" alt="image" src="https://github.com/user-attachments/assets/1bcfaaab-dde0-4493-a329-932f25ab4979" />
<br> </br>

Now it’s time to test the automation workflow. I started from the webhook and re-emitted the event so the process can start from the beginning. After re-emission, I received the email and slack message like I was supposed to.
 
 <img width="877" height="655" alt="image" src="https://github.com/user-attachments/assets/0866d881-32ad-43a8-961e-2783e52abf64" />
 <img width="975" height="264" alt="image" src="https://github.com/user-attachments/assets/c7e5b9bc-34be-4f73-9c2c-567cd400c14d" />
 <img width="917" height="282" alt="image" src="https://github.com/user-attachments/assets/be708a22-423c-457f-8fb2-b1b6ca304928" />
<br> </br>

I then went to the user prompt and chose yes so I can see the process of getting the machine isolated.

 <img width="975" height="708" alt="image" src="https://github.com/user-attachments/assets/7c8f85ff-9b75-45a7-af96-f5e6c42391c2" />
 <img width="533" height="470" alt="image" src="https://github.com/user-attachments/assets/e6cc61e5-2779-42ba-9762-bbc824d7e3b4" />
<br> </br>

Once I chose yes, my VM immediately went offline. When this happened for the first time, I could not figure out why my VM wasn’t connecting to the internet and couldn’t finish my project. I spent hours researching “Windows 10 VM not connecting to the internet all of a sudden” trying to see what was wrong and I did a bunch of troubleshooting. It wasn’t until the next day and doing the project again that I realized what happened. I actually did the project and tested it on the same machine which was the VM. So, once I isolated it, I didn’t have any internet access. To get the VM back online, I had to login into my LimaCharlie account again on my host machine, go to the sensor and rejoin the VM back to the network. After doing that, I refreshed my VM and everything loaded perfectly. I couldn’t believe it LOL.

(VM)

<img width="808" height="450" alt="image" src="https://github.com/user-attachments/assets/46c8b690-969b-42c0-9825-f8a7aa2ba0de" />


(Host Machine)
 
 
<img width="827" height="318" alt="image" src="https://github.com/user-attachments/assets/bae1fe7d-855c-48a6-b355-66facfa6e006" />


But once I got all that sorted out, I now understood what happened and can move on. Once Yes is selected in the user prompt, the machine gets isolated, and I automated a message in Tines to alert my Slack inbox about the isolation.

 <img width="814" height="248" alt="image" src="https://github.com/user-attachments/assets/624d3b3c-3e36-460c-8289-ab6627390f5c" />
<br> </br>

This wraps up the project about SOAR/EDR and Automation and I have learned a lot. I learned about LimaCharlie, Tines and Slack; configured my first Detection and Response rule, and automated messages to my Slack and Email inboxes about the events that were occurring. This project turned out to be really fun after understanding the process and I plan to sharpen my skills even more as I continue my journey into IT.


</p>

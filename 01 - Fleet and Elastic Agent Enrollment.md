# Lab 01 – Fleet and Elastic Agent Enrollment

## Personal Reflection

The first step in the detection engineering lab was enrolling a Fleet server (Windows VM) and installing an Elastic agent on that server to create an agent policy. The agent would provide visibility into my endpoint data flowing into Elastic.

---

## Goals

- Create my Windows VM in Azure  
- Set up Fleet  
- Create an agent policy and enroll an agent  
- Deploy the Elastic Agent to the VM  
- Install the Windows integration  

---

## Step-by-Step Process

### 1. Create Windows VM
I created a Windows VM in Azure with 2 vCPUs and 8 GB of RAM (Standard D2s v3)

<img width="1197" height="422" alt="image" src="https://github.com/user-attachments/assets/ea280806-5eab-488b-a005-eb21c028bdf9" />

---

### 2. Creating Elastic Fleet
On my **host machine**, I logged into my Elastic instance using the **Elastic Cloud** option at https://cloud.elastic.co/  
From there, I navigated to **Assets → Fleet → Agents**.  

<img width="502" height="571" alt="image" src="https://github.com/user-attachments/assets/0c66a49e-8fd3-4874-9115-d5759e58434c" />

---

### 3. Creating a New Agent Policy
I switched to the **Agent policies** tab and created a new policy for my Windows VM.  
- Named the policy, 'Windows Agent Policy' and clicked 'Create policy'  

<img width="800" height="672" alt="image" src="https://github.com/user-attachments/assets/0f5efb4d-d8c6-4bfb-ac8c-f7d30fd62137" />

“Enroll in Fleet?” – Leave this as recommended
“Install Elastic Agent on your host” – this is the command you’ll need to run on the host to set up the Elastic Agent.
Elastic showed me the command needed to install and enroll the agent. This command handles everything: download, install, enroll, and start.
Copy the Windows command and return to your VM and continue to Step 4

---

### 4. Installing the Elastic Agent on Windows
Back in the VM, I opened **PowerShell as Administrator** and ran the provided command (Windows section).  

<img width="708" height="225" alt="image" src="https://github.com/user-attachments/assets/c60f8651-2b4c-43e7-a00f-5ce8dca2a2a1" />

The command essentially did the following:
- **Downloaded** the Elastic Agent ZIP  
- **Unzipped** it into the current directory  
- **Changed directory** into the extracted folder  
- **Installed & enrolled** the agent using the cluster URL and enrollment token  

When prompted, I typed `Y` to install the Elastic Agent as a service.

<img width="998" height="272" alt="image" src="https://github.com/user-attachments/assets/49031128-db3d-4d0b-a142-48bd29b98434" />

A few useful PowerShell commands I ran afterward to generate some activity:
```powershell
whoami
net user
```

---

### 5. Confirming Enrollment
Two confirmations stood out:  
1. PowerShell output confirming the installation  
2. Elastic dashboard showing my agent connected and data flowing  

<img width="768" height="377" alt="image" src="https://github.com/user-attachments/assets/ef0e0832-aa36-4f8b-be6d-ae5e6319f23c" />

Fleet machine should be listed

<img width="1343" height="461" alt="image" src="https://github.com/user-attachments/assets/d200d9a9-a32e-44da-830a-c09deba80cb4" />

---

### 6. Adding the Windows Integration
By default, the **System integration** was applied, but it only captured basic logs. I installed Sysmon on the Windows VM and wanted to capture its logs.  

Steps:  
- Opened my agent policy  
- Clicked **Add integration**
  <img width="1347" height="316" alt="image" src="https://github.com/user-attachments/assets/2e2cd4ab-0764-405a-ae0b-6682c55369c3" />
 
- Searched for **Windows**
  <img width="927" height="273" alt="image" src="https://github.com/user-attachments/assets/eaf779a2-590d-4e78-952d-fb72aadfc46e" />

- Selected **Windows Integration** → **Add Windows**
  <img width="956" height="880" alt="image" src="https://github.com/user-attachments/assets/1457059b-c961-436c-99ee-49106a31110e" />
 
- Left defaults as they were, saved, and deployed  

Now my agent has more complete logs (including Sysmon events) that sets me up for deeper analysis for later in this course.

---

## Key Takeaways

- This lab is an introduction to Fleet and agent management — it showed me how seamless Elastic has made endpoint enrollment.  
- Seeing my Windows VM logs flow into Elastic was a nice step into detection engineering.  
- Adding the Windows integration reminded me how important it is to verify *what* logs are being collected, not just that the agent is online.  

---

**End Result:** My Windows VM was successfully enrolled in Fleet, with the Elastic Agent installed and the Windows integration enabled. Data was flowing, and I was ready for the next stage of detection engineering.

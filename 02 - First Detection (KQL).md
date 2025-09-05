# Part 2 – Baby’s First Detection (KQL)

## Personal Reflection

On my second part of the detection engineering class, I documented my steps into building a custom detection in Elastic using **Kibana Query Language (KQL)**. This was a transition from collecting data with the Elastic Agent on my Windows VM from part 1 to writing a query and turning it into a detection rule.  

The goal was to detect when someone runs the `net user` command on a Windows machine. This is mapped to **MITRE ATT&CK T1087.001 – Account Discovery: Local Account**, a common technique attackers use to enumerate local accounts.

---

## Goals of the Lab

- Write a KQL query to detect `net user` execution  
- Optimize the query to reduce cluster load  
- Create a custom detection rule in Elastic  
- Trigger the alert by running `net user` in the VM  

---

## Generating an Event

Since we are looking at creating a detection to look for the MITRE technique T1087.001, which is Account Discover: Local Account. I ran the 'net user' command in PowerShell on my Windows VM.

<img width="847" height="368" alt="image" src="https://github.com/user-attachments/assets/4325e9f0-06fd-4fc9-94c0-731d1acbeba0" />


Going back to Elastic and searching for the event resulted in a successful logging of the 'net user' command usage.

<img width="1553" height="468" alt="image" src="https://github.com/user-attachments/assets/75d5f2dc-dadf-46a9-bcbd-8d2e775d1724" />

<img width="531" height="721" alt="image" src="https://github.com/user-attachments/assets/2bbbeff7-96cf-47fa-84aa-e35c10d47136" />

---

## Creating the Detection Rule

Now that we have a query, we can move forward with creating the alert itself.

1. **Navigate to Security > Rules > Detection rules > Create new rule**
     <img width="507" height="237" alt="image" src="https://github.com/user-attachments/assets/de20c70f-c04e-4d5d-8632-fafc04b5fe18" />
     <img width="720" height="77" alt="image" src="https://github.com/user-attachments/assets/55757a2c-7aff-4d33-b78d-d85dcde07a85" />
     
   - Selected **Create new rule** → **Custom query**
     <img width="1078" height="635" alt="image" src="https://github.com/user-attachments/assets/307dafdc-86df-409c-86df-b3480ee3fd05" />

   - For “Source” leave that as the Index patterns
  
    For 'Custom Query', **Insert the query**:  
     ```kql
     process.name: net.exe and process.args: "user"
     ```

   **Suppression settings**:  
   - Suppressed by `host.name` and `user.name`  
   - Prevented duplicate alerts if `net user` was run multiple times in a short window  

    <img width="950" height="770" alt="image" src="https://github.com/user-attachments/assets/c749934d-ba13-413f-b290-7fe5c1c152dd" />


  **Preview and validate**:  
   - Hit “Rule preview” at the top with a time range applied to see what comes back  
    <img width="1042" height="125" alt="image" src="https://github.com/user-attachments/assets/bc9a1852-989f-45e2-bdf9-20d2516676e7" />
    <img width="502" height="597" alt="image" src="https://github.com/user-attachments/assets/a5c52533-a42a-471b-b8fe-fd9ef3f2191c" />

6. Hit continue at the bottom if the results look good
    **Metadata**:  
   - **Rule Name:** Net User Discovery  
   - **Description:** Adversaries may attempt to get a listing of local system accounts. This information can help adversaries determine which local accounts exist on a system to aid in follow-on behavior. 
Commands such as net user and net localgroup of the Net utility and id and groupson macOS and Linux can list local users and groups. On Linux, local users can also be enumerated through the use of the /etc/passwd file. On macOS the dscl . list /Users command can be used to enumerate local accounts.

<img width="977" height="784" alt="image" src="https://github.com/user-attachments/assets/34219fe8-cd42-4d71-a211-b875047c3fbc" />

7. Open up the Advanced settings
   - **Reference URLs**, I included the Atomic Red Team atomic link. - https://github.com/redcanaryco/atomic-red-team/blob/master/atomics/T1087.001/T1087.001.md
	 - **False positive examples** I put “A technician performing maintenance on a host” as an example.
	 - **MITRE ATT&CK threats** section, I navigate down the list to add T1087.001

<img width="952" height="512" alt="image" src="https://github.com/user-attachments/assets/5ddf60d4-1a04-45e9-8e08-fdfc1f606ff5" />


8. **Investigation Guide**:  
   	1. Investigate the alerting event to determine what parent process spawned it
		2. Questions to ask:
			a. Was this an automated action performed by Windows? Or does this seem to have been triggered by a user intentionally?
			b. Investigating the process id and the parent process id, was there any follow-on activity or preceding activity that seems abnormal?
		3. Make an assessment based on your investigation
			a. If the activity is performed automated by Windows, that is not an alert of note
			b. If the activity is performed and looks to be a part of routine maintenance performed by a technician, that is also not an alert of note
		4. If the above leads you to believe that the activity may be abnormal or suspicious, consider escalating the alert.

<img width="932" height="616" alt="image" src="https://github.com/user-attachments/assets/f13855a2-6bc6-40e3-a05c-78d9bc135052" />

9. Click Continue, “Schedule rule” leave everything at the default settings and Continue, click 'Create & enable rule'
<img width="427" height="72" alt="image" src="https://github.com/user-attachments/assets/4cdbf4b5-6e6d-418c-920c-271f1b55c1eb" />
<img width="1853" height="873" alt="image" src="https://github.com/user-attachments/assets/cb28533e-e7f6-45ad-a888-c8cadf537431" />

---

## Triggering the Alert

Back in my Windows VM, I ran:

```powershell
net user
```

Within a few minutes, Elastic generated an alert tied to my new rule. It was rewarding to see the full lifecycle: query → rule → alert.

<img width="733" height="207" alt="image" src="https://github.com/user-attachments/assets/69565b72-80a3-469b-92f7-f569a730b26a" />
<img width="1872" height="529" alt="image" src="https://github.com/user-attachments/assets/7f3f9cd1-29ca-44b2-8e37-61b57b1d3fb6" />

---

## Key Takeaways

- **Context is critical.** Not every `net user` is malicious — severity/risk scoring and investigation guides help SOC analysts filter noise.  
- **Hands-on detection building** made me realize how much goes into turning raw telemetry into actionable security alerts.  

---

**End Result:** I successfully built my first custom detection in Elastic, mapped to MITRE ATT&CK T1087.001, and validated it by generating a real alert from my lab VM.  

Next up: testing detections with Atomic Red Team!

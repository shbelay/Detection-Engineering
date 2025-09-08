# SOC Detection Engineering Crash Course – Lab Journey

This repository documents my hands-on journey through the **SOC Detection Engineering Crash Course with Hayden Covington**.  

This workshop provided me with a structured process to build, test, and refine detections inside a fully functional SIEM. By the end of these labs, I gained not only a strong foundation in detection engineering theory but also practical skills in building effective, high-fidelity detections.

---

## Completed Parts

### Part 1 – Fleet and Elastic Agent Enrollment
- Booted a Windows VM and deployed the Elastic Agent.  
- Enrolled the host into Fleet for centralized management.  
- Added the **Windows integration** to capture richer telemetry (including Sysmon logs).  
- Confirmed data was flowing into Elastic, establishing the foundation for detection work.  

📄 [Part 01 - Fleet and Elastic Agent Enrollment.md](https://github.com/shbelay/Detection-Engineering/blob/main/01%20-%20Fleet%20and%20Elastic%20Agent%20Enrollment.md)

---

### Part 2 – Baby’s First Detection (KQL)
- Wrote my first custom detection using **Kibana Query Language (KQL)**.  
- Targeted MITRE ATT&CK **T1087.001 – Account Discovery (Local Account)** via the `net user` command.  
- Optimized the query using `process.args` to reduce noise and cluster load.  
- Created a detection rule with metadata, MITRE mappings, and false positive examples.  
- Successfully generated alerts by running `net user` on my VM.  

📄 [Part 02 - First Detection (KQL).md](https://github.com/shbelay/Detection-Engineering/blob/main/02%20-%20First%20Detection%20(KQL).md)

---

### Part 3 – Testing Detections with Atomic Red Team
- Learned the importance of validating detections against realistic attack simulations.  
- Installed and configured **Atomic Red Team (ART)** on Windows.  
- Ran the **T1087.001-9 Atomic** test to simulate account discovery.  
- Fixed setup issues by manually creating the `atomics` directory and adding YAML files.  
- Successfully triggered my Elastic detection rule with ART.  

📄 [Part 03 - Testing Detections with Atomic Red Team.md
](https://github.com/shbelay/Detection-Engineering/blob/main/03%20-%20Testing%20Detections%20with%20Atomic%20Red%20Team.md)

---

### Part 4 – Tuning Detections in Elastic
- Learned that real SOC work requires constant **tuning** to balance fidelity and noise.  
- Simulated an IT helpdesk scenario by creating a “Helpdesk” user and running `net user`.  
- Added a **rule exception** in Elastic to filter benign alerts tied to Helpdesk activity.  
- Documented the exception with notes and leveraged the option to close matching alerts.  
- Improved detection fidelity by reducing unnecessary noise without losing visibility.  

📄 [Part 04 - Tuning Detections.md](https://github.com/shbelay/Detection-Engineering/blob/main/04%20-%20Tuning%20Detections.md)

---

## Key Takeaways
- **Hands-on practice matters:** Writing queries is only the beginning; testing and tuning complete the lifecycle.  
- **Context is everything:** Not every event is malicious; exceptions and risk scoring bring realism.  
- **Tools like ART are powerful:** They provide repeatable, scalable ways to validate detection rules.  
- **SOC work is iterative:** Each step builds on the last, making detections stronger and more reliable.  

---

Certificate of Completion

<img width="1113" height="747" alt="image" src="https://github.com/user-attachments/assets/a5933ad2-1bd6-410a-97df-949f526e8c21" />


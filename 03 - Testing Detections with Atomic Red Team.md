# Part 3 – Testing Detections with Atomic Red Team

## Personal Reflection

On the third part of my detection engineering course, I moved from **building detections** to **validating them** using Atomic Red Team (ART). Testing is a critical step in detection, as it verifies that alerts appear under realistic conditions..

The detection I built on Day 2 (for `net user` execution) maps to MITRE ATT&CK **T1087.001: Account Discovery – Local Account**. ART has a matching atomic test for this technique, which allowed me to validate my detection.

---

## Goals of the Lab

- Run an Atomic Red Team test for **T1087.001**  
- Ensure the detection in Elastic fires an alert  
- Learn to troubleshoot ART setup issues  
- Understand the value of automated detection testing  

---

## Running the Atomic Test

I imported the ART module and executed the atomic:

```powershell
Install-Module -Name invoke-atomicredteam,powershell-yaml -Scope CurrentUser
```
<img width="1106" height="316" alt="image" src="https://github.com/user-attachments/assets/89ebb5c2-c3d3-4f77-8c40-c54020b1a92b" />

Install the ART execution framework:

```powershell
IEX (IWR 'https://raw.githubusercontent.com/redcanaryco/invoke-atomicredteam/master/install-atomicredteam.ps1' -UseBasicParsing);
Install-AtomicRedTeam
```

<img width="1101" height="126" alt="image" src="https://github.com/user-attachments/assets/1565ea2f-338b-4880-b486-c83d5f379f13" />

This command specifically runs the atomic tied to the **net user discovery technique**.

---

## Setup Challenges

I attempted to run: Invoke-AtomicTest T1087.001-9 or Invoke-AtomicTest T1087.001 -TestNumbers 9, I got the following error:

<img width="1088" height="161" alt="image" src="https://github.com/user-attachments/assets/f8492960-02ab-45b1-8993-d919a338eaf7" />

Initially, I ran into an error because the `atomics` directory wasn’t created properly during setup. Here’s what I had to do:

1. Manually created the `C:\AtomicRedTeam\atomics` directory.  
2. Downloaded the full repo ZIP:  
   [Atomic Red Team GitHub](https://github.com/redcanaryco/atomic-red-team/archive/refs/heads/master.zip)  
3. Extracted and copied all contents from:  
   ```
   C:\Users\shbelay\Downloads\atomic-red-team-master\atomic-red-team-master\atomics
   ```
   into:  
   ```
   C:\AtomicRedTeam\atomics
   ```

After fixing this, I reran the script and it executed successfully. The output provided an incredible amount of detail about what was happening on the system — far more than running `net user` manually.

<img width="728" height="425" alt="image" src="https://github.com/user-attachments/assets/237cfce2-d827-49be-954b-6774e5e3cb3f" />
<img width="955" height="991" alt="image" src="https://github.com/user-attachments/assets/770f64fd-a51b-4e5c-a014-1e6aa98b8654" />



---

## Results

As expected, running the ART test **triggered my Elastic detection**. I saw the alert appear in Elastic Security, confirming that my KQL detection was correctly aligned with the MITRE technique.

<img width="1690" height="662" alt="image" src="https://github.com/user-attachments/assets/ff9f13a2-a0d1-423d-8aae-40c4c797363c" />

---

## Why Atomic Red Team Matters

- **Repeatability:** Instead of running ad-hoc commands, I now have a standardized way to validate detections.  
- **Scalability:** ART allows me to build a catalog of atomics and test multiple detections in one go.  
- **Confidence:** Seeing my custom detection fire from a validated atomic test gave me assurance that the rule works as intended.  

---

**End Result:** I successfully validated my detection for MITRE T1087.001 using Atomic Red Team. This gave me confidence that the rule is effective against a real-world attack simulation.  

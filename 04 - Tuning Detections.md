# Part 4 – Tuning Detections in Elastic

## Personal Reflection

For part 4, I began to tune and refine detections from the previous parts. A rule that generates too much noise can quickly overwhelm analysts, so the ability to filter out benign activity is critical for building fidelity into alerts.  

In this lab, I tuned the **Net User Discovery detection** I created earlier so that it wouldn’t constantly trigger on legitimate activity from IT helpdesk accounts.

---

## Goals of the Lab

- Filter out noise or expected activity from detections  
- Add a rule exception in Elastic  
- Understand how tuning increases alert fidelity  

---

## Lab Steps

### 1. Create a Standard User
On my Windows VM, I created a new user account named **Helpdesk**:

```powershell
net user Helpdesk <password> /add
```
<img width="518" height="93" alt="image" src="https://github.com/user-attachments/assets/a8efacbe-44b9-4136-a51c-650f300cf77a" />

This simulated a real-world IT support account that might legitimately run the `net user` command for troubleshooting.

---

### 2. Run Detection as Helpdesk
Still in PowerShell, I switched to the Helpdesk account and ran:

```powershell
Runas /user:Helpdesk "net user"
```
<img width="577" height="66" alt="image" src="https://github.com/user-attachments/assets/8e799cff-55a1-4df1-ab1e-95354c86573e" />

Back in Elastic, I confirmed that the detection rule fired and generated alerts.
<img width="1702" height="890" alt="image" src="https://github.com/user-attachments/assets/3a563530-e268-47f1-a3f6-69ccffcd750e" />

---

### 3. Add a Rule Exception
Since this account represents a trusted IT helpdesk user, I didn’t want to flood Elastic with alerts for expected behavior. To tune the detection:

- Navigated to:  
  **Elastic → Security → Rules → Detection Rules (SIEM) → Net User Discovery**
  <img width="453" height="297" alt="image" src="https://github.com/user-attachments/assets/14b95006-6ad8-4726-9420-e73c111785de" />
  <img width="1695" height="596" alt="image" src="https://github.com/user-attachments/assets/41f4d7c4-282a-4131-a9d5-34a8a6ccfc6c" />

- Selected the **Rule Exceptions** tab

   <img width="333" height="397" alt="image" src="https://github.com/user-attachments/assets/d183d876-31d5-4d1d-8b70-20807aa4293f" />

- Clicked **Add rule exception**  
  <img width="563" height="292" alt="image" src="https://github.com/user-attachments/assets/59ee61da-43ef-4e3f-b5b4-ece97f84a58d" />

I named the exception, added the **Helpdesk username** to the condition, and included a comment for documentation.  

I also enabled the option under **Alert actions**:  
> “Close all alert actions that match this exception.”  
<img width="801" height="775" alt="image" src="https://github.com/user-attachments/assets/c172439b-d4c5-4315-bbaf-cf4b6d15b5ca" />

This cleared existing alerts tied to that account and ensured future activity from Helpdesk wouldn’t generate noise.

Below image is prior to enabling the rule exception. There were 5 alerts.
<img width="1636" height="241" alt="image" src="https://github.com/user-attachments/assets/be705c5c-b052-49cc-ab5e-c357f4582126" />

After enabling the rule exception, there are 4 alerts. It removed the alert that was triggered from the first step of this part of the course.
<img width="1637" height="208" alt="image" src="https://github.com/user-attachments/assets/0d946a64-1007-4a72-bfa2-2c8a30b65ff9" />

---

## Key Takeaways

- **Tuning is essential.** Detections without tuning often create more noise than value.  
- **Exceptions should be documented.** Adding a name and comment helps others understand why the filter exists.  

---
 **End Result:** I successfully tuned my Net User Discovery rule by adding an exception for the Helpdesk account, reducing noise while maintaining detection fidelity.  

# Tenable Basic Agent Scan: Windows 11 VM

<img width="711" height="347" alt="Screenshot 2026-05-06 181102" src="https://github.com/user-attachments/assets/a441e683-9378-418b-b0fb-85c4d6ae9cfe" />




## Overview

The purpose of this lab was to complete a ***Basic Agent Scan*** . In this lab, I provisioned a Windows 11 virtual machine, installed a Tenable Nessus Agent, linked the agent to Tenable Vulnerability Management, created a Basic Agent Scan, triggered the scan using a local trigger file, reviewed the vulnerability results, and performed cleanup.

This lab demonstrates how Nessus Agents can be used to scan endpoints remotely and send vulnerability data back to the Tenable cloud platform.

---

## Lab Objectives

By the end of this lab, I was able to:

- Provision a Windows 11 virtual machine
- Create a Nessus Agent Group in Tenable
- Create a Basic Agent Scan
- Install and link a Nessus Agent on a Windows endpoint
- Trigger a local agent scan using a trigger file
- Verify the agent connection in Tenable
- Review vulnerability results
- Clean up the scan, agent group, agent, and virtual machine

---

## Tools Used

- Windows 11 Virtual Machine
- PowerShell
- Tenable Vulnerability Management
- Nessus Agent
- Triggered Agent Scan

---

## Lab Environment

| Component | Description |
|---|---|
| Endpoint | Windows 11 Virtual Machine |
| Scanner Type | Nessus Agent |
| Platform | Tenable Vulnerability Management |
| Scan Type | Basic Agent Scan |
| Trigger File | `start.txt` |

---

# Lab Steps

---

## Step 1: Provision a Windows 11 Virtual Machine

The first step was to provision a Windows 11 virtual machine that would be used as the endpoint for the Nessus Agent scan.

The VM acts as the target machine where the Nessus Agent is installed.

<img width="1918" height="981" alt="Lab1" src="https://github.com/user-attachments/assets/867a476c-3275-4820-a22b-e1878e4290fa" />

## Step 2: Create a Nessus Agent Group

Next, I created a Nessus Agent Group in Tenable called **Windows 11 Agent Group 1**

An Agent Group is used to organize Nessus Agents and assign them to specific scans. This helps separate endpoints by lab, business unit, operating system, department, or testing purpose.

Navigation path:

```text
Settings -> Sensors -> Nessus Agents -> Agent Groups -> + Add Agent Group
```

### Why This Matters?

The Agent Group tells Tenable which endpoints should be included in the agent scan. When the Nessus Agent is installed on the Windows 11 VM, the agent will be linked to this group.

<img width="1918" height="978" alt="Lab3" src="https://github.com/user-attachments/assets/86b67fa5-bfd1-454d-a443-5b4e8f202981" />

## Step 3: Create a Basic Agent Scan

After creating the Nessus Agent Group, I created a new scan using the **Basic Agent Scan** template.

Navigation path:

```text
Scans -> Create Scan -> Agent Scan -> Basic Agent Scan
```

### Why This Matters?

This step connects the scan to the Agent Group created earlier. Once the Windows 11 VM is linked to that group, Tenable knows which endpoint should be included in the scan.

<img width="1918" height="977" alt="Lab2" src="https://github.com/user-attachments/assets/301214ab-c0db-4577-9c5a-7cd218f1887c" />

## Step 4: Configure the Scan as a Triggered Scan

Next, I configured the Basic Agent Scan to run as a **Triggered Scan**.

A triggered scan starts when a specific file is created on the endpoint. In this lab, the trigger file was named:

```text
start.txt
```

### Why This Matters?

Using a triggered scan gives control over when the Nessus Agent scan begins. Instead of waiting for a scheduled scan time, the scan starts when the trigger file is created on the Windows 11 VM.

Once the Nessus Agent detects the trigger file, it begins the local scan and removes the file.


## Step 5: Install the Nessus Agent on the Windows 11 VM

After configuring the Basic Agent Scan, I installed the Nessus Agent on the Windows 11 virtual machine.

On the Windows 11 VM, I opened **PowerShell as Administrator** and ran the Nessus Agent installation command.

### Nessus Agent Installation Command

```powershell
Invoke-WebRequest -Uri "https://sensor.cloud.tenable.com/install/agent/installer/ms-install-script.ps1" -OutFile "./ms-install-script.ps1"; 
& "./ms-install-script.ps1" -key "<TENABLE-LINKING-KEY-OMMITED-FOR-SECURITY>" -type "agent" -name "<Windows-11-Agent-1>" -groups "<Windows-11-Agent-Group-1>"; 
Remove-Item -Path "./ms-install-script.ps1"
```

### What This Command Does

Downloads the Nessus Agent installation script
Runs the installation script
Links the agent to Tenable using a linking key
Assigns the agent to the selected Agent Group
Removes the installation script after it runs

<img width="1918" height="978" alt="Lab4" src="https://github.com/user-attachments/assets/54224349-01c4-4db4-80b2-15575c2a1f47" />

<img width="1567" height="1000" alt="Lab5" src="https://github.com/user-attachments/assets/9acf4b38-ee0e-4649-a6e0-dda3721fa02f" />


## Step 6: Verify the Nessus Agent Service

After installing the Nessus Agent on the Windows 11 VM, I verified that the Nessus Agent service was installed and running.

This step confirms that the agent is active on the endpoint before checking for it in the Tenable portal.

### PowerShell Command

Open PowerShell as Administrator and run:

```powershell
Get-Service -Name "Tenable Nessus Agent"
```

<img width="1572" height="996" alt="lab7" src="https://github.com/user-attachments/assets/bfdde413-ca09-431a-abe8-2cb9a1f031ca" />

## Step 7: Verify the Nessus Agent in Tenable

After confirming that the Nessus Agent service was running on the Windows 11 VM, I went back to the Tenable portal to verify that the agent appeared successfully.

Navigation path:

```text
Settings -> Sensors -> Nessus Agents
```

### Why This Matters?

This step confirms that the Windows 11 VM successfully linked to Tenable. If the agent does not appear in the Nessus Agents section, the scan will not be able to target the endpoint.

<img width="1918" height="968" alt="lab8" src="https://github.com/user-attachments/assets/90ca1883-e4bb-4f8e-99a1-91d46ecb111d" />

<img width="1918" height="947" alt="lab9" src="https://github.com/user-attachments/assets/56764c60-83ff-4ac0-8b93-21ad066af45f" />

## Step 8: Trigger the Agent Scan

After verifying that the Nessus Agent appeared in Tenable, I triggered the scan from the Windows 11 VM by creating a file called start.txt.

The scan was configured earlier as a **Triggered Scan** using the trigger file name:

```text
start.txt
```
<img width="1571" height="997" alt="Lab6" src="https://github.com/user-attachments/assets/833d0a1f-ca3b-44aa-8b67-f7a7d2f3a074" />


## Step 9: Oberseve the Scan Results/Vulnerabilities

After the scan was completed the results showed the following results:

| Severity | Vulnerability |
|---|---|
| HIGH | 5 |
| MEDIUM | 1 |
| LOW | 1 |


<img width="1918" height="980" alt="Lab1" src="https://github.com/user-attachments/assets/4320eb59-d468-4197-83ba-6515b02e0ecc" />

---

After reviweing the results a cleanup was complete and the VM was deleted.

---

## Key Takeaways

This lab demonstrated how Tenable Nessus Agents can be used to scan endpoints remotely.

**Key lessons learned:**

Nessus Agents are installed directly on endpoints.
Agent Groups help organize endpoints based on business or lab needs.
Basic Agent Scans allow Tenable to collect vulnerability data from linked endpoints.
Triggered scans can be started by creating a specific trigger file.
Vulnerability results may take time to populate after the agent begins scanning.
Cleanup is important to avoid unused cloud resources and stale Tenable assets.

## Conclusion

In this lab, I successfully configured a Nessus Agent-based vulnerability scan against a Windows 11 virtual machine.

I created an Agent Group, configured a Basic Agent Scan, installed and linked the Nessus Agent, triggered the scan using start.txt, reviewed the vulnerability results in Tenable, and cleaned up the lab environment.

This lab helped reinforce how agent-based vulnerability scanning works and how endpoint data is collected and reported back to Tenable.

---

```text
Author        : Manuel Aguirre
LinkedIn      : linkedin.com/in/mannyaguirre/
GitHub        : github.com/mannyaguirre
Date Created  : May 5, 2026
Last Modified : May 5, 2026
Version       : 1.0

# AWS Systems Manager Patch Manager – Linux and Windows Patching

## Overview

In this lab, I used **AWS Systems Manager Patch Manager** to manage operating system updates across Linux and Windows EC2 instances.

I first used the default Amazon Linux patch baseline to scan and install patches on three Linux instances. I then created a custom Windows patch baseline focused on security updates, associated it with a Windows patch group, and used Patch Manager to patch three Windows instances.

Finally, I reviewed the Systems Manager compliance information to verify that all six EC2 instances were compliant.

### Environment

The lab environment contained:

* 3 Linux EC2 instances
* 3 Windows EC2 instances
* AWS Systems Manager
* Systems Manager Fleet Manager
* Systems Manager Patch Manager
* Run Command
* Default Amazon Linux patch baseline
* Custom Windows patch baseline

---

# Task 1: Patching Linux Instances Using the Default Baseline

## Objective

The first part of the lab involved using the default AWS patch baseline to scan and install updates on the Linux EC2 instances.

## What I Did

I opened **AWS Systems Manager** and used **Fleet Manager** to review the managed EC2 instances.

I selected the `Linux-1` instance and reviewed its management information, including the operating system, platform type, node type, and IAM role.

This confirmed that the Linux instances were registered with Systems Manager and could be managed through Patch Manager.

I then opened **Patch Manager** and selected the **Patch now** option.

For the patching operation, I configured Patch Manager to:

| Configuration      | Value                                |
| ------------------ | ------------------------------------ |
| Patching operation | Scan and install                     |
| Reboot option      | Reboot if needed                     |
| Instance selection | Target instances using tags          |
| Tag key            | `Patch Group`                        |
| Tag value          | `LinuxProd`                          |
| Patch baseline     | AWS-AmazonLinux2DefaultPatchBaseline |

The `LinuxProd` tag was already configured on the Linux instances as part of the lab environment.

## Patching Results

I started the patching operation and monitored the **AWS-PatchNowAssociation** execution.

The operation identified the three Linux instances associated with the `LinuxProd` patch group and began scanning and installing the required patches.

I monitored the operation until the patching process completed.

### Evidence

![Linux Patch Manager](images/task1-linux-patching.png)

> **Screenshot:** Patch Manager showing the Linux patching operation and the targeted Linux instances.

![Linux Patch Results](images/task1-linux-patch-results.png)

> **Screenshot:** Patch operation results showing the completed Linux instance patching.

---

# Task 2: Creating a Custom Windows Patch Baseline

## Objective

For the Windows instances, I created a custom patch baseline instead of relying solely on the default Windows baseline.

The purpose of the custom baseline was to control which Windows security updates would be approved and how quickly they would become approved for installation.

## Creating the Patch Baseline

I opened **Systems Manager → Patch Manager → Patch baselines** and created a new patch baseline named:

```text
WindowsServerSecurityUpdates
```

I used the following configuration:

| Configuration          | Value                           |
| ---------------------- | ------------------------------- |
| Name                   | `WindowsServerSecurityUpdates`  |
| Description            | Windows security baseline patch |
| Operating system       | Windows                         |
| Default patch baseline | Not selected                    |

## Approval Rules

I configured two approval rules for Windows Server 2019 security updates.

### Rule 1 – Critical Security Updates

| Setting              | Configuration     |
| -------------------- | ----------------- |
| Product              | WindowsServer2019 |
| Severity             | Critical          |
| Classification       | SecurityUpdates   |
| Auto-approval        | 3 days            |
| Compliance reporting | Critical          |

### Rule 2 – Important Security Updates

| Setting              | Configuration     |
| -------------------- | ----------------- |
| Product              | WindowsServer2019 |
| Severity             | Important         |
| Classification       | SecurityUpdates   |
| Auto-approval        | 3 days            |
| Compliance reporting | High              |

I then created the custom patch baseline.

### Evidence

![Windows Custom Patch Baseline](images/task2-windows-patch-baseline.png)

> **Screenshot:** Custom `WindowsServerSecurityUpdates` patch baseline and its configuration.

---

# Task 2.1: Associating the Patch Baseline with a Patch Group

After creating the baseline, I associated it with the Windows patch group.

I modified the patch groups for the `WindowsServerSecurityUpdates` baseline and added:

```text
Patch Group: WindowsProd
```

This association allows Patch Manager to apply the custom Windows baseline to instances that have the corresponding `WindowsProd` tag.

### Evidence

![Windows Patch Group](images/task2-windows-patch-group.png)

> **Screenshot:** The custom Windows baseline associated with the `WindowsProd` patch group.

---

# Task 3: Patching the Windows Instances

## Objective

The next stage involved applying the custom Windows patch baseline to the three Windows EC2 instances.

## Task 3.1: Tagging the Windows Instances

I opened the **EC2 console** and added the following tag to each Windows instance:

```text
Key:   Patch Group
Value: WindowsProd
```

I applied the tag to:

* `Windows-1`
* `Windows-2`
* `Windows-3`

This allowed Systems Manager Patch Manager to identify the Windows instances that should receive the custom patch baseline.

### Evidence

![Windows Instance Tag](images/task3-windows-tag.png)

> **Screenshot:** The `Patch Group = WindowsProd` tag applied to a Windows instance.

---

# Task 3.2: Patching the Windows Instances

After tagging the Windows instances, I returned to **Systems Manager → Patch Manager** and started a new patching operation.

I used the following configuration:

| Configuration      | Value                       |
| ------------------ | --------------------------- |
| Patching operation | Scan and install            |
| Reboot option      | Reboot if needed            |
| Instance selection | Target instances using tags |
| Tag key            | `Patch Group`               |
| Tag value          | `WindowsProd`               |

I started the patching operation and monitored the execution.

The operation targeted the three Windows instances associated with the `WindowsProd` patch group.

## Reviewing the Execution

Once the execution started, I opened the **Execution ID** to monitor the patching process.

I then reviewed the output for one of the managed Windows instances.

The output showed the Systems Manager patching activity being performed on the instance.

I also reviewed the output for the `PatchGroup: WindowsProd` information.

This demonstrated how Patch Manager uses **Run Command** and the Systems Manager patching documents to perform the patching operation on the managed instances.

### Evidence

![Windows Patch Execution](images/task3-windows-patch-execution.png)

> **Screenshot:** Windows patching execution showing the targeted instances.

![Windows Run Command Output](images/task3-run-command-output.png)

> **Screenshot:** Run Command output showing the patching operation and `WindowsProd` patch group information.

---

# Task 4: Verifying Patch Compliance

## Objective

After completing the Linux and Windows patching operations, I used the Systems Manager compliance reports to verify that the instances were successfully patched.

## Compliance Dashboard

I opened:

**Systems Manager → Patch Manager → Dashboard**

Under **Compliance summary**, I verified that the environment showed:

```text
Compliant: 6
```

This indicated that all six EC2 instances were compliant.

The six compliant instances consisted of:

* 3 Linux instances
* 3 Windows instances

### Evidence

![Patch Compliance Dashboard](images/task4-compliance-dashboard.png)

> **Screenshot:** Patch Manager compliance dashboard showing all six instances as compliant.

---

# Task 4.1: Compliance Reporting

I then opened the **Compliance reporting** tab to review the individual instance compliance status.

The Linux and Windows instances were listed with a **Compliant** status.

I reviewed the node patching details, including:

* Critical noncompliant count
* Security noncompliant count
* Other noncompliant count
* Last operation date
* Baseline ID

This allowed me to confirm not only the overall compliance status but also the patching information associated with each managed node.

### Evidence

![Compliance Reporting](images/task4-compliance-reporting.png)

> **Screenshot:** Compliance reporting showing the Linux and Windows instances as compliant.

---

# Task 4.2: Reviewing Installed Patches

I selected one of the Windows node IDs to view its detailed information.

Within the node details, I opened the **Patch** tab and reviewed the patches that had been applied to the instance.

I also reviewed the **Installed Time** information to confirm when the patches were installed.

### Evidence

![Installed Windows Patches](images/task4-installed-patches.png)

> **Screenshot:** Windows node Patch tab showing installed patches and installation times.

---

# Results

The patching activities were completed successfully.

### Linux Instances

I successfully:

* Reviewed the managed Linux EC2 instances.
* Used the default Amazon Linux patch baseline.
* Targeted the instances using the `LinuxProd` patch group.
* Performed a **Scan and install** operation.
* Allowed instances to reboot if required.
* Monitored the patching operation until completion.

### Windows Instances

I successfully:

* Created a custom Windows patch baseline.
* Configured security update approval rules.
* Created the `WindowsServerSecurityUpdates` baseline.
* Associated the baseline with the `WindowsProd` patch group.
* Added the `WindowsProd` tag to the Windows EC2 instances.
* Used Patch Manager to scan and install Windows updates.
* Reviewed the Run Command output generated by the patching operation.

### Compliance Verification

After patching both operating system types, I verified the overall compliance status.

The final compliance summary showed:

```text
Compliant: 6
```

All three Linux and all three Windows EC2 instances were reported as **Compliant**.

---

# Key Learning

This lab gave me practical experience using **AWS Systems Manager Patch Manager** to manage operating system updates across both Linux and Windows EC2 instances.

I learned how default patch baselines can be used for standard patching requirements, while custom patch baselines provide more control over which updates are approved.

I also learned how **patch groups and EC2 tags** can be used to target specific instances and associate them with the appropriate patch baseline.

Finally, I used Systems Manager compliance reporting to verify the results and confirm that all six managed instances were compliant.

---

# Technologies Used

* **AWS Systems Manager**
* **Systems Manager Patch Manager**
* **Systems Manager Fleet Manager**
* **AWS Systems Manager Run Command**
* **Amazon EC2**
* **Linux**
* **Windows Server 2019**
* **Patch Baselines**
* **Patch Groups**
* **EC2 Tags**
* **Systems Manager Compliance Reporting**

---

# Screenshots

The screenshots below provide evidence of the configuration and work completed during the lab.

| Task   | Screenshot                      |
| ------ | ------------------------------- |
| Task 1 | Linux patching operation        |
| Task 1 | Linux patch results             |
| Task 2 | Custom Windows patch baseline   |
| Task 2 | Windows patch group association |
| Task 3 | Windows `Patch Group` tag       |
| Task 3 | Windows patch execution         |
| Task 3 | Run Command output              |
| Task 4 | Compliance dashboard            |
| Task 4 | Compliance reporting            |
| Task 4 | Installed Windows patches       |


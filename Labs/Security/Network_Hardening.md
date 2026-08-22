# Amazon Inspector Lab

## Task 1: Activate Amazon Inspector

### Objective

Activate **Amazon Inspector** to continuously scan the Lambda functions for security vulnerabilities.

### Steps

1. Open the **AWS Management Console**.

2. Search for **Inspector** and choose **Amazon Inspector**.

3. In the left navigation pane, choose **Activate Inspector**.

4. Choose **Activate Inspector** again.

5. Wait for the message:

   > Welcome to Inspector. Your first scan is underway.

6. If the **Feedback for Amazon Inspector** survey appears, choose **Cancel**.

7. Close any banner messages.

8. Refresh the page periodically.

### Verification

Navigate to:

**Dashboard → Summary → Environment coverage → Lambda functions**

Confirm that **Lambda functions** shows:

```text
100%
```

### Result

Amazon Inspector is activated and scanning the Lambda functions.

---

## Task 2: Review the Inspected Resources

### Objective

Review the vulnerabilities detected by Amazon Inspector in the Lambda functions.

### Task 2.1: Review Lambda Functions

1. In Amazon Inspector, choose **Findings**.
2. Select **All findings**.
3. Review the three vulnerability findings.

The findings should include the following information:

| Field             | Description                      |
| ----------------- | -------------------------------- |
| Severity          | Vulnerability severity           |
| Impacted resource | Affected Lambda function         |
| Title             | Description of the vulnerability |

### Vulnerability Reviewed

Select:

```text
CVE-2023-32681 - requests
```

4. Open the vulnerability details.
5. Under **Vulnerability details**, choose the external link next to **Vulnerability ID**.
6. Review the vulnerability information provided by the **National Vulnerability Database (NVD)**.
7. Review the **Remediation** section.

### Finding

The vulnerability is related to an outdated version of the Python **requests** package.

The recommended remediation is to upgrade the package.

### Result

The vulnerable Lambda package was identified and the recommended remediation was reviewed.

---

## Task 3: Remediate the Vulnerability

### Objective

Update the Lambda function to remove the vulnerable version of the `requests` package and verify that Amazon Inspector closes the finding.

### Task 3.1: Update the Lambda Function

1. Open the **AWS Management Console**.
2. Search for **Lambda**.
3. Choose **Lambda**.
4. Select the **get-request** function.
5. In the code editor, open:

```text
requirements.txt
```

6. Locate:

```text
requests==2.20.0
```

7. Change it to:

```text
requests
```

8. Choose **Deploy**.

### Expected Deployment Message

```text
Successfully updated the function get-request
```

---

## Task 3.2: Verify the Remediation

1. Return to **Amazon Inspector**.
2. In the left navigation pane, choose **Findings → All findings**.
3. Change **Finding status** from **Active** to **Closed**.
4. Locate:

```text
CVE-2023-32681 - requests
```

### Expected Result

The vulnerability appears under **Closed** findings.

This confirms that the vulnerable package was successfully remediated.

> **Note:** Amazon Inspector may take several minutes to complete the new scan. Refresh the page periodically.

---

## Task 3.3: Verify the Lambda Scan

1. In Amazon Inspector, choose **Resources coverage**.
2. Select **Lambda functions**.
3. Locate the **Last scanned** column.
4. Expand the column if necessary to view the complete timestamp.

### Expected Result

The **get-request** Lambda function shows a recently updated **Last scanned** timestamp.

This confirms that Amazon Inspector performed a new scan after the Lambda function was updated.

---

## Screenshots

### Amazon Inspector Activated

*Add screenshot here.*

### Lambda Findings

*Add screenshot showing the `CVE-2023-32681 - requests` finding here.*

### Updated `requirements.txt`

*Add screenshot showing:*

```text
requests
```

### Closed Finding

*Add screenshot showing `CVE-2023-32681 - requests` under **Closed** findings.*

### Lambda Last Scanned

*Add screenshot showing the updated **Last scanned** timestamp.*

---

## Summary

In this lab, Amazon Inspector was activated to scan AWS Lambda functions for vulnerabilities. The `CVE-2023-32681 - requests` vulnerability was identified and remediated by updating the `requests` package. The finding was then verified as **Closed**, confirming successful remediation.


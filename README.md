Here is a Markdown-formatted **Admin Guide** for using `Get-OfficeLicenses.ps1` and `Collect-OfficeLicenses.ps1`, ready to be included in a GitHub README:

---

# 🛠️ Admin Guide: Get Office Activation Toolkit

This guide explains how to use the two PowerShell scripts in this toolkit to gather and consolidate Microsoft Office license data across multiple Windows devices.

## 📄 Script Overview

| Script                       | Purpose                                                                                     |
| ---------------------------- | ------------------------------------------------------------------------------------------- |
| `Get-OfficeLicenses.ps1`     | Extracts local Office license details (user/device) from a single device                    |
| `Collect-OfficeLicenses.ps1` | Remotely collects license data from multiple devices and combines results into a single CSV |

---

## ⚙️ Prerequisites

* PowerShell Remoting enabled on all target devices (`Enable-PSRemoting -Force`)
* Network share for storing results (e.g. `\\server\share\OfficeLicReports`)
* Necessary firewall exceptions and permissions for remote script execution
* Admin access to target machines
* Scripts copied to admin workstation or central management system

---

## 🚀 Usage Instructions

### 1. `Get-OfficeLicenses.ps1`

This script runs on **an individual machine** to extract Office license data.

#### 🔹 Use Locally

```powershell
.\Get-OfficeLicenses.ps1
```

#### 🔸 What it does:

* Scans for user-mode and device-mode Office licenses.
* Parses license details including:

  * License state (Licensed, Grace, RFM)
  * Product ID, Tenant ID, Expiration
  * Email address (if available)
* Returns the data as PowerShell objects (used by the controller script).

---

### 2. `Collect-OfficeLicenses.ps1`

This script runs on your **admin workstation** to remotely query multiple machines.

#### 🔹 Modify Target List

Edit the `$computers` array to match your environment:

```powershell
$computers = @("PC1", "PC2", "PC3")
```

#### 🔹 Define Output Location

Ensure `$networkOutputPath` points to a valid, writable network location:

```powershell
$networkOutputPath = "\\server\share\OfficeLicReports"
```

#### 🔹 Run Script

```powershell
.\Collect-OfficeLicenses.ps1
```

#### 🔸 What it does:

* Uses `Invoke-Command` to run license extraction logic remotely on each computer.
* Combines all returned license data into a single CSV file:

  * Named format: `Combined_OfficeLicenses_<hostname>_<timestamp>.csv`
  * Output path: your specified network share

---

## 📦 Example Output

| ComputerName | Type   | Product | LicenseState      | Email    | TenantId                                    |                                      |
| ------------ | ------ | ------- | ----------------- | -------- | ------------------------------------------- | ------------------------------------ |
| PC1          | NUL    | User    | ProPlus2021Volume | Licensed | [user@example.com](mailto:user@example.com) | 11111111-2222-3333-4444-555555555555 |
| PC2          | Device | MAK     | ProPlus2019Retail | Grace    | (blank)                                     | 66666666-7777-8888-9999-000000000000 |

---

## 🧼 Cleanup & Notes

* You may delete intermediate logs or schedule this to run via Task Scheduler or Intune.
* The toolkit does **not modify or remove any licenses**—it's read-only.
* Use `Remove-Item` manually or through custom logic if cleanup is required.

---

## 🔐 Security Tip

Ensure only trusted admins have access to:

* These scripts
* The remote execution environment
* The CSV output path (as it may contain email addresses or tenant info)

---

Let me know if you'd like a `README.md` file generated or guidance on GitHub publishing.

---

# 📘 Admin Guide: `vNextDiag2.ps1` — Get Office Activation Tool

## Overview
It is designed to:
* Display Office license modes per product release
* Show Shared Computer Licensing status
* Reveal special policy configurations (Viewer Mode, Extended Offline Access, Unattended/RPA)
* List detailed licensing information per mode
* Remove specific licenses by ID

> ⚙️ **Version**: 1.1.0
> 🖥️ **Supported Platforms**: Windows 7 and above (Office 2016 or later)

---

## 🛠️ Requirements

* Must be run with **appropriate permissions** (admin for machine-level registry access).
* PowerShell 5.1+ recommended.
* Microsoft 365 Apps installed (Click-to-Run supported).

---

## 🚀 Usage

```powershell
.\vNextDiag.ps1 [-action <list|remove>] [-licenseId <string>]
```

### Parameters

| Parameter    | Description                               | Required                 |
| ------------ | ----------------------------------------- | ------------------------ |
| `-action`    | `list` (default) or `remove`              | No                       |
| `-licenseId` | License ID to remove (used with `remove`) | Yes (if `action=remove`) |

---

## 📋 Actions

### 🔍 1. List License Info

```powershell
.\vNextDiag.ps1 -action list
```

**Displays:**

* **Mode per ProductReleaseId:** Legacy, Device, or vNext
* **Shared Computer Licensing:** Enabled/Disabled with token details
* **Additional Licensing Settings:**

  * Viewer Mode
  * Extended Offline Access
  * Unattended/RPA
* **Licenses:**

  * Both User-based (NUL) and Device-based
  * Includes fields like Product, ACID, Email, Expiration, License State, etc.

### ❌ 2. Remove License

```powershell
.\vNextDiag.ps1 -action remove -licenseId "<LicenseId>"
```

Removes a license from:

* `%LOCALAPPDATA%\Microsoft\Office\Licenses`
* `%PROGRAMDATA%\Microsoft\Office\Licenses`

> 🔒 **Requires correct `LicenseId`**; otherwise, the script will report it as not found.

---

## 🧩 Additional Licensing Settings

These are registry-based policies checked by the script:

| Setting Name                | Location                                                        | Type    |
| --------------------------- | --------------------------------------------------------------- | ------- |
| **Viewer Mode**             | `HKLM\SOFTWARE\Policies\Microsoft\Office\16.0\Common\Licensing` | Machine |
| **Extended Offline Access** | Same as above                                                   | Machine |
| **Unattended/RPA**          | `HKCU\Software\Microsoft\Office\Common\Licensing`               | User    |

Values:

* `1` = Enabled
* `0` = Disabled
* *Missing* = Not Configured

---

## 🧪 Output Example

```text
========== Mode per ProductReleaseId ==========
ProPlusRetail = vNext

========== Shared Computer Licensing ==========
SharedComputerLicensing = Enabled
{
  "ACID": "...",
  "User": "...",
  "NotBefore": "...",
  "NotAfter": "..."
}

========== Additional Licensing Settings ==========
ViewerMode = 1
ExtendedOfflineAccess = Not Configured
UnattendedRPA = 0

========== vNext licenses found ==========
...

========== Device licenses found ==========
...
```

---

## 📎 Notes

* For full registry access, **run PowerShell as Administrator**.
* This script does **not activate** Office; it **diagnoses and cleans** licensing data only.
* Useful for imaging environments, automation audits, or troubleshooting grace mode issues.

---

## 📂 Related Paths

| Purpose                     | Path                                             |
| --------------------------- | ------------------------------------------------ |
| User Licenses               | `%LOCALAPPDATA%\Microsoft\Office\Licenses`       |
| Device Licenses             | `%PROGRAMDATA%\Microsoft\Office\Licenses`        |
| Tokens for Shared Licensing | `%LOCALAPPDATA%\Microsoft\Office\16.0\Licensing` |

---

Let me know if you'd like this guide exported as a `.md` file or need branding/customization for deployment.

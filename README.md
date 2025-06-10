Here's a **Markdown-formatted user guide** for using the `vNextDiag.ps1` PowerShell script:

---

# 📘 vNextDiag.ps1 User Guide

**Version:** 1.0.0
**Purpose:** This script provides diagnostics and management capabilities for Microsoft Office vNext licensing. It can list current license information or remove specific licenses.

---

## 🛠️ Usage

### 🔹 Basic Syntax

```powershell
.\vNextDiag.ps1 [-action <list|remove>] [-licenseId <LicenseId>]
```

* **`-action`**:
  Defines the operation to perform.

  * `list` (default): Displays license info and Shared Computer Licensing status.
  * `remove`: Deletes a license with the given `LicenseId`.

* **`-licenseId`**:
  Required only if `-action remove` is specified. Specifies the license to delete.

---

## 📄 Actions

### ✅ `list` (Default)

Displays the following information:

#### 1. **Mode per ProductReleaseId**

* Reads registry values from:

  ```
  HKCU:\SOFTWARE\Microsoft\Office\16.0\Common\Licensing\LicensingNext
  ```
* Shows mode:

  * `vNext` (2)
  * `Device` (3)
  * `Legacy` (default)

#### 2. **Shared Computer Licensing**

* Checks registry for `SharedComputerLicensing` in:

  * `HKLM:\SOFTWARE\Microsoft\Office\ClickToRun\Configuration`
  * `HKLM:\SOFTWARE\Microsoft\Office\16.0\Common\Licensing`
  * `HKLM:\SOFTWARE\Policies\Microsoft\Office\16.0\Common\Licensing`

* Reports whether SCL is **Enabled** or **Disabled**.

* Attempts to display token files containing:

  * ACID
  * User
  * Validity period

#### 3. **License Information**

Shows license files stored in:

* For **User (NUL)**:

  ```
  %LOCALAPPDATA%\Microsoft\Office\Licenses
  ```

* For **Device**:

  ```
  %PROGRAMDATA%\Microsoft\Office\Licenses
  ```

Displays details:

* License type, product, state (Licensed/Grace/RFM)
* User or Device ID
* Entitlement status and expiration
* Associated email (if available)
* LicenseId

---

### 🗑️ `remove`

Removes a license file based on its `LicenseId`.

#### Example:

```powershell
.\vNextDiag.ps1 -action remove -licenseId abc123xyz
```

* Searches through user and device license directories.
* Deletes the file if a matching `LicenseId` is found.
* Outputs success or failure message.

---

## 🧪 Sample Commands

### List all licenses and configurations:

```powershell
.\vNextDiag.ps1
```

### Remove a license by ID:

```powershell
.\vNextDiag.ps1 -action remove -licenseId 1234abcd-5678efgh
```

---

## 📝 Notes

* Requires PowerShell with permission to read registry and file system.
* Use elevated permissions if required (especially for reading from `HKLM`).
* Ensure you have backups or snapshots before removing licenses.

---

## 📚 Version History

* **1.0.0** — Initial release

---

Let me know if you'd like this published as a `.md` file or need formatting adjusted for a specific platform (like GitHub or internal documentation).

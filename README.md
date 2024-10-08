# Implementing a Password Manager Across Client Devices (Remote Support)

This project aims to deploy a **password manager** across user devices, enhancing security and simplifying the login process. *(Completed during my voluntary experience, providing remote tech-support for community members).*

![Diagram](images/IMG_PWManager-Diagram.png)

## 🛠 Tools & Technologies:

- **System:** `Windows 10/11`, `Android OS`
- **Virtualization:** `VMware`
- **Scripting:** `PowerShell`
- **Remote Desktop Support:** `TeamViewer`
- **Video Conferencing:** `Zoom`
- **Documentation:** `AI Tutorial Builder`/`Video Editor` *(VegasPro/CapCut)*
- **Security:** Password Manager `(Bitwarden)` - *256-bit AES encryption*

---

# Introduction

The user expressed a constant frustration of <ins>having to remember multiple passwords</ins> for their online accounts and how long it would take to input their login details due to their <ins>slow typing.</ins> 

They had locked themselves out of their account multiple times, due to <ins>mis-entering their password</ins>. 

They were also concerned about the <ins>security risks</ins> of leaving <ins>password hints on note files</ins> across different devices, including their smartphones and laptop.

### 👤 User Background:
- The client is an elderly person with limited technical experience, running an independent paralegal business.
- They're looking to integrate digital tools into their existing workflow, with password management being a key concern.
- Frustrations include remembering multiple passwords and security risks of storing them insecurely.

<img src="images/IMG_AccountLockOut.png" width="40%" height="40%" />

---

## ⛔ Problem Statement:
Challenges the user faced in managing passwords included:
- ❌ Difficulty in remembering multiple passwords
- ❌ Time-consuming and error-prone, manual entering of passwords
- ❌ Security risk of storing password hints on note files, to get leaked/stolen

## 💡 Solution:
After researching and reviewing several options I chose `BitWarden`, a password manager offering:
- ✅ Centralized password storage
  - *(no need to remember passwords)*
- ✅ Auto-fill functionality
  - *(no more slow-typing/mis-entering passwords)* 
- ✅ Cross-platform compatibility
  - *(for web browser/smartphone/laptop - no longer needs to store hints across devices)*   

<img src="images/IMG_Bitwarden-Solution.png" width="40%" height="40%" />

---

# 💻 Implementation:

### 1️⃣ Assessment/Preparation:
- Simulated the user's desktop environment by creating a virtual machine instance using `VMware`. 

- Conducted a trial run of installing the Password Manager, to make sure all of its features were functioning before deploying it for the client.

> <ins>**User's** desktop environment</ins>

<details><summary>VM checklist</summary>

  - [x] **OS**: `Windows 10` 
  - [x] **Hardware Specs**: CPU: *2 cores* | RAM: *8GB (8192)* | HDD1: *256GB*
  - [x] **Web Browser**: Firefox
  - [x] **Accounts**: AOL Mail, Outlook, Gmail, PayPal, Metro Bank

</details>

> <ins> **Adjust Bitwarden Settings** for each Platform</ins> 

<details><summary>Desktop Settings - checklist</summary>

  `Windows 10`
  
  SECURITY
  - [x] **Vault timeout**: "On restart" *(Settings > Security)*
  - [x] **Vault timeout action**: "Lock" *(Settings > Security)*
  - [x] **Enable** "Unlock with PIN" *(Settings > Security)*
  - [x] **Unlock with Master PW when app is restarted**: "No" *(Settings > Security)*

  PREFERENCES
  - [x] **Clear clipboard**: "20 seconds" *(Settings > Preferences)*

  APP SETTINGS
  - [x] **Theme**: "Dark" *(Settings > App Settings)*  

</details>

<details><summary>Web Browser Settings - checklist</summary>

  `Firefox`
  
  MANAGE
  - [x] **Show auto-fill menu on form fields**: "When field is selected on focus" *(Settings > Auto-fill)* 
  - [x] **Enable** "Auto-fill on page load" *(Settings > Auto-fill)*
  - [x] **Default URI match detection**: Base domain *(Settings > Auto-fill)* 

  SECURITY
  - [x] **Vault timeout**: "On restart" *(Settings > Security)*
  - [x] **Vault timeout action**: "Lock" *(Settings > Security)*
  - [x] **Enable** "Unlock with PIN code" *(Settings > Security)*
  - [x] **Disable** "Lock with Master PW when app is restarted *(Settings > Security)*

  OTHER
  - [x] **Clear clipboard**: "20 seconds" *(Settings > General)*
  - [x] **Theme**: "Dark" *(Settings > Display)*  

</details>

<details><summary>Android Mobile Settings - checklist</summary>

  `ANDROID OS`
  
  UNLOCK OPTIONS
  - [x] **Enable** "unlock with PIN code" *(Settings > Account Security)*
  - [x] **Unlock with Master PW when app is restarted**: "No" *(Settings > Account Security)*

  SESSION TIMEOUT
  - [x] **Session timeout**: "1 min" *(Settings > Account Security)*
  - [x] **Session timeout action**: "Lock" *(Settings > Account Security)*

  AUTO-FILL
  - [x] **Enable** "auto-fill services" *(Settings > Autofill)*
  - [x] **Enable** "use inline autofill" *(Settings > Autofill)*
  - [x] **Enable** "use accessibility" *(Settings > Autofill)*
  - [x] **Enable** "use draw over" *(Settings > Autofill)*

  APPEARANCE
  - [x] **Theme**: "Dark" *(Settings > Appearance)*

</details>

---

> [!IMPORTANT]
> **Drop-down sections above** (^) - expand for more info.

---

### 2️⃣ Installation/Configuration:
- Performed the installation remotely, using `TeamViewer` and `Zoom`.

- This allowed for full control of the user's laptop and `Android` smartphone, configuring settings for a more user-friendly experience.

- Because TeamViewer lacked a built-in, voice calling feature, Zoom was used instead to communicate with the client. Providing timely responses to their requests and questions.

`PowerShell` -script

Installing **Bitwarden** with <ins>*Windows Package Manager*</ins>: `winget`

```powershell
# Check if winget is installed
$wingetInstalled = Get-Command winget -ErrorAction SilentlyContinue

# Install winget if it's not installed
if (-not $wingetInstalled) {
    $latestWingetMsixBundleUri = $(Invoke-RestMethod https://api.github.com/repos/microsoft/winget-cli/releases/latest).assets.browser_download_url | Where-Object {$_.EndsWith(".msixbundle")}
    $latestWingetMsixBundle = $latestWingetMsixBundleUri.Split("/")[-1]

    # Download Visual C++ Runtime and Winget packages
    Write-Information "Downloading Winget to artifacts directory..."
    Invoke-WebRequest -Uri $latestWingetMsixBundleUri -OutFile "$env:TEMP\$latestWingetMsixBundle"
    Invoke-WebRequest -Uri https://aka.ms/Microsoft.VCLibs.x64.14.00.Desktop.appx -OutFile "$env:TEMP\Microsoft.VCLibs.x64.14.00.Desktop.appx"

    # Install Visual C++ Runtime and Winget packages
    Add-AppxPackage -Path "$env:TEMP\Microsoft.VCLibs.x64.14.00.Desktop.appx"
    Add-AppxPackage -Path "$env:TEMP\$latestWingetMsixBundle"
    Write-Host "winget installed successfully."
}

# Install Bitwarden using winget
Write-Host "Installing Bitwarden..."
winget install Bitwarden.Bitwarden -e
Write-Host "Bitwarden installed successfully."

```
> [!NOTE]
> This script automates the downloading and silent installation of Bitwarden, reducing the manual effort and time needed to search for and run the installer.

---

### 3️⃣ Training:
During the training session, the user was concerned they wouldn't remember how to use the Password Manager, so I offered to create a video tutorial to serve as a reference, in case they ever forgot the correct action steps and needed reminding.  

To produce the Tutorial, I used an `AI Step Recorder` to capture my mouse movements and automate the screen recording and annotation process, demonstrating how to use the Password Manager. The final video was produced using a `Video Editor` , to include AI-voice narration, subtitles and timestamps.

https://github.com/SimCmngs/PasswordManager-Implementation/assets/166608651/6bce557b-d351-4856-8047-1574502c86fe

---

## 📊 Results:
Benefits for the end-user:
- 🚀 **Enhanced workflow and ease-of-use**
    - *(The password manager's autofill feature, will streamline the login process, <ins>saving the user time</ins> and <ins>reducing frustration</ins>)*.
    
- 🔒 **Improved security**
    - *(A centralised and synchronized password vault, will make it <ins>safer to access passwords across different devices</ins>)*.
    
- 🧠 **Increased self-reliance**
    - *(A video tutorial, will allow for the user to troubleshoot issues on their own, <ins>reducing the reliance on external support</ins> and <ins>minimizing disruptions to their work</ins>)*.

---

## 🎉 Conclusion:
The user expressed their satisfaction with the Password Manager solution, appreciating its ease of use and positive impact on their daily work.

---

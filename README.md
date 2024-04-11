# Implementing a Password Management System - Across Client's Devices

This was a project I completed during my voluntary experience providing tech support for elderly users. This project aimed to install a password manager across a client's devices, enhancing their security and 
streamlining their login process.

![Logo](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/th5xamgrr6se0x5ro4g6.png)

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

When I met with the client, they expressed a constant frustration of <ins>having to remember multiple passwords</ins> for their online accounts and how long it would take to input their login details due to their <ins>slow typing.</ins> 

They had locked themselves out of their account multiple times, due to <ins>mis-entering their password</ins>. 

The client was also concerned about the <ins>security risks</ins> of leaving <ins>password hints on note files</ins> across different devices, including their smartphones and laptop.

### 👤 Client Background:
- The client is an elderly person with limited technical experience, running an independent paralegal business.
- They're looking to integrate digital tools into their existing workflow, with password management being a key concern.
- Frustrations include remembering multiple passwords and security risks of storing them insecurely.

![App Screenshot](https://via.placeholder.com/468x300?text=App+Screenshot+Here)


## ⛔ Problem Statement:
Challenges the client faced in managing passwords included:
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

![App Screenshot](https://via.placeholder.com/468x300?text=App+Screenshot+Here)

---

# 💻 Implementation:

### 1️⃣ Assessment/Preparation:
I simulated the client's desktop environment by creating a virtual machine instance using `VMware`. This allowed me to conduct a trial run of installing the Password Manager, to make sure all of its features were functioning before deploying it for the client.

<details><summary>VM checklist (mimic client's desktop environment)</summary>

  - [x] **OS**: `Windows 10` 
  - [x] **Hardware Specs**: CPU: *2 cores* | RAM: *8GB (8192)* | HDD1: *256GB*
  - [x] **Web Browser**: Firefox
  - [x] **Accounts**: AOL Mail, Outlook, Gmail, PayPal, Metro Bank

</details>

<ins>**Adjust Settings for each Platform**</ins>

<details><summary>Desktop Settings - checklist</summary>

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

  MANAGE
  - [x] **Show auto-fill menu on form fields**: "When field is selected on focus" *(Settings > Auto-fill)* 
  - [x] **Enable** "Auto-fill on page load" *(Settings > Auto-fill)*
  - [x] **Default URI match detecion**: Base domain *(Settings > Auto-fill)* 

  SECURITY
  - [x] **Vault timeout**: "On restart" *(Settings > Security)*
  - [x] **Vault timeout action**: "Lock" *(Settings > Security)*
  - [x] **Enabl**e "Unlock with PIN code" *(Settings > Security)*
  - [x] **Disbale** "Lock with Master PW when app is restarted *(Settings > Security)*

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
  - [x] **Theme**: "Dark" *(Settings > Apperance)*

</details>

---

### 2️⃣ Installation/Configuration:
I then performed the installation remotely using both `TeamViewer` and `Zoom`. With TeamViewer, I was able to control the client's laptop and `Android` smartphone remotely, configuring settings for a more user-friendly experience.

Because TeamViewer lacked a built-in, voice calling feature, I used Zoom instead to communicate with the client. Providing timely responses to all of their requests and questions.

`PowerShell` 

<ins>Installing **Bitwarden** with *Windows Package Manager*</ins>: `winget`

```powershell
# Check if winget is installed
$wingetInstalled = Get-Command winget -ErrorAction SilentlyContinue

# Install winget if it's not installed
if (-not $wingetInstalled) {
    Write-Host "Installing winget..."
    Invoke-WebRequest -Uri "https://github.com/microsoft/winget-cli/releases/latest/download/Microsoft.DesktopAppInstaller_8wekyb3d8bbwe.appxbundle" -OutFile "$env:TEMP\winget.appxbundle"
    Add-AppxPackage -Path "$env:TEMP\winget.appxbundle"
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
During the training session, the client was concerned they wouldn't remember how to use the Password Manager, so I offered to create a video tutorial to serve as a reference, in case they ever forgot the correct action steps and needed reminding.  

To produce the Video Tutorial, I used an `AI Step Recorder` to capture my mouse movements and automate the screen recording and annotation process, demonstrating how to use the Password Manager. I produced the final video, using a `Video Editor` to include voice narration, subtitles and timestamps.

![App Screenshot](https://via.placeholder.com/468x300?text=App+Screenshot+Here)

---

## 📊 Results:
Benefits for the client:
- 🚀 **Enhanced workflow and ease-of-use**
    - *(The password manager's autofill feature, will streamline the login process, saving the client time and reducing frustration)*.
    
- 🔒 **Improved security**
    - *(A centralised and synchronized password vault, will make it safer to access passwords accross different devices)*.
    
- 🧠 **Increased self-reliance**
    - *(A video tutorial, will enable the client to troubleshoot issues on their own, reducing the reliance on external support and minimizing disruptions to their work)*.

---

## 🎉 Conclusion:
The client expressed their satisfaction with the Password Manager solution, appreciating its ease of use and positive impact on their daily work.

---

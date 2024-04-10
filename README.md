# Implementing a Password Management System Across All Devices - (Voluntary Exp.)

### 👤 Client Background:
- The client is an elderly person with limited technical experience, running an independent paralegal business.
- They're looking to integrate digital tools into their existing workflow, with password management being a key concern.
- Frustrations include remembering multiple passwords and security risks of storing them insecurely.

## Introduction

1. When I met with the client, they expressed a constant frustration of <ins>having to remember multiple passwords</ins> for their online accounts and how long it would take to input their login details due to their <ins>slow typing.</ins> 

2. They had locked themselves out of their accounts multiple times, due to <ins>mis-entering their password</ins>. 

3. The client was also concerned about the <ins>security risks</ins> of leaving <ins>password hints on note files</ins> across different devices, including their smartphones and laptop.

![App Screenshot](https://via.placeholder.com/468x300?text=App+Screenshot+Here)

---

## ⛔ Problem Statement:
Challenges the client faced in managing passwords included:
- ❌ Difficulty in remembering multiple passwords
- ❌ Time-consuming and error-prone manual entering of passwords
- ❌ Security risk of storing password hints on note files to get leaked/stolen

## 💡 Solution:
After evaluating several options I chose `BitWarden`, a password manager offering:
- ✅ Centralized password storage
  - *(no need to remember passwords)*
- ✅ Auto-fill functionality
  - *(no more slow-typing/mis-entering passwords)* 
- ✅ Cross-platform compatibility
  - *(for web browser/smartphone/laptop - no longer needs to store hints across devices)*   

![App Screenshot](https://via.placeholder.com/468x300?text=App+Screenshot+Here)

---

## 💻 Implementation:

### 📋 Assessment/Preparation:
Simulated the client's desktop environment by creating a virtual machine instance using `VMware`. This allowed me to conduct a trial run of installing the Password Manager, to make sure all of its features were functioning before deploying it for the client.

### 🔧 Installation/Configuration:
Performed the installation remotely using both `TeamViewer` and `Zoom`. With TeamViewer, I was able to control the client's laptop and smartphone remotely, configuring settings for a more user-friendly interface.

Because TeamViewer lacked a built-in, voice calling feature, I used Zoom instead to communcicate with the client. Providing timely responses to all of their requests and questions.

`PowerShell` <ins>installing Bitwarden with Windows Package Manager `(winget)`:</ins>

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

### 🎓 Training:
- Created a video tutorial using AI Step Recorder.
- Included voice narration, subtitles, and timestamps.

![App Screenshot](https://via.placeholder.com/468x300?text=App+Screenshot+Here)

---

## 📊 Results:
Benefits for the client:
- 👍 Enhanced workflow and ease-of-use 
- 👍 Improved security 
- 👍 Increased self-reliance 

---

## 🎉 Conclusion:
The client expressed satisfaction with the Password Manager solution, appreciating its ease of use and positive impact on their daily work.


---

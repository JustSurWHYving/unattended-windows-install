## Overview

Microsoft gives you the ability to add [Answer Files](https://learn.microsoft.com/en-us/windows-hardware/manufacture/desktop/update-windows-settings-and-scripts-create-your-own-answer-file-sxs?view=windows-11) (or Unattend files) to Windows Installation Media, which can be used to modify Windows Settings and Packages in the Windows Image (ISO) during the Windows Setup process.

If you’ve ever used Rufus to create Windows Installation Media, you might have seen options like “Remove Windows 11 Hardware Requirements” and “Disable Privacy Questions”. When you select those options, Rufus creates an answer file with your selected options and includes it on the installation media.

I've created a few of these answer files that I use to automate and streamline my Windows installs just the way I like them, and that’s what I’m sharing with you in this project. 
  - The answer files work on any edition (Home/Pro) of Windows 10 22H2 and Windows 11 23H2.
  - *Windows 11 24H2 will be tested once it's fully released.*
<br/>

## Why should I use an Answer File?

### Security:
  - You can see all of the changes that will be made to the Windows Image by inspecting the answer file.
  - It runs on the Official Windows 10 or 11 ISO downloaded directly from Microsoft, no need to download edited ISO files from Unofficial sources.
  - It's not dependent on any third party tools and is an Official Microsoft Feature used to make Mass Windows Deployments easier.
### Automation:
  - When installing Windows on multiple computers, there's no need to manually configure settings and run scripts on each machine, which saves a lot of time (and effort).
<br/>

## What do answer files do?

### <ins>Choose one of the following versions:</ins>
### LTSC-Like
### *Recommended for most people*
   - Includes most of the same Windows Packages as LTSC
     - (Windows Security, Edge, Notepad, Snipping Tool, Calculator, Paint, Legacy Windows Media Player) with added Microsoft Store.
   - Only Security updates are installed, others are delayed for 2 years (max period)
   - Includes better privacy settings and various other tweaks.
   - UAC is Disabled by Default to ensure the `currentuser.cmd` script executes correctly at first logon. If you use UAC, please enable it in Control Panel once you're in Windows.
<br/>

### Standard
### *This acts as a "Blank Canvas" where you can start from scratch and only install the software you want).*
   - ALL Windows Packages are removed except for Windows Security. Microsoft Edge (NOT on Win 10 after updates) and Microsoft Store are both removed.
   - Only Security updates are installed, others are delayed for 2 years (max period)
   - Includes better privacy settings and various other tweaks.
   - UAC is Disabled by Default to ensure the `currentuser.cmd` script executes correctly at first logon. If you use UAC, please enable it in Control Panel once you're in Windows.
<br/>

> [!IMPORTANT]
> If you want to install and use Adobe Creative Cloud then you need to reinstall Microsoft Edge or else the Adobe Installer won't run.
> 
> Open Windows Powershell as admin and run the following command to reinstall Microsoft Edge (or use the Chris Titus Windows Utility)
>```
>winget install -e --id Microsoft.Edge
>```
<br/>

> [!NOTE]
> Due to the removal of Microsoft Edge, I also include a Powershell Script on the Desktop called "LAUNCH-CTT-WINUTIL.ps1" 
    Make sure you are connected to the internet, then right click on this file and select Run with Powershell. It will launch the Chris Titus Tech Windows Utility and you can use that to install your browser of choice (even Edge) and any other software for that matter.
<br/>

> [!NOTE]
> I have marked common customizations that vary a lot user to user with "OPTIONAL" in their code comments, to change these settings - Open the autounattend.xml file in a text editor then, CTRL+F and Search for OPTIONAL. Now just uncomment or comment out the features as per your liking :)
<br/>

## Usage Instructions

In short, you need to include the `autounattend.xml` answer file on your Windows Installation Media so it can be read and executed during the Windows Setup. Here are a few ways to do it:
> [!CAUTION]
> The filename included on the root of the Windows Installation Media must be `autounattend.xml` or else it won't execute.
</br>

> [!NOTE]
> If the following instructions are unclear, follow this [video tutorial.](https://youtu.be/pDEZDD_gEbo)
</br>

### <ins>Method 1: Create a Bootable Windows Installation Media</ins>

1. Download your preferred `autounattend.xml` file and save it on your computer.
2. Create a [Windows 10](https://www.microsoft.com/en-us/software-download/windows10) or [Windows 11](https://www.microsoft.com/en-us/software-download/windows11) Bootable Installation USB drive with [Rufus](https://rufus.ie/en/). 
> [!IMPORTANT]
> 1. DO NOT USE the Media Creation Tool to Create the Windows Installation USB!
> 
> 2. When using Rufus, don’t select any of the checkboxes in “Customize Your Windows Experience” as it creates another answer file, we don't want that.
3. Copy the `autounattend.xml` file you downloaded in Step 1 to the root of the Bootable Windows Installation USB you created in Step 2.
4. Boot from the Windows Installation USB, do a clean install of Windows as normal, and the scripts will run automatically.
</br>

### <ins>Method 2: Create a Custom ISO File</ins>

1. Download your preferred `autounattend.xml` file and save it on your computer.
2. Download the [Windows 10](https://www.microsoft.com/en-us/software-download/windows10) or [Windows 11](https://www.microsoft.com/en-us/software-download/windows11) ISO file depending on the version you want.
3. Download and Install [AnyBurn](https://anyburn.com/download.php)
   - In AnyBurn, select the “Edit Image File” option.
   - Navigate to and select the Official Windows ISO file you downloaded in Step 2.
   - Click on “Add” and select the `autounattend.xml` file you downloaded in Step 1 or just click and drag the `autounattend.xml` into the AnyBurn window.
   - Click on “Next,” then on “Create Now.” You should be prompted to overwrite the ISO file, click on “Yes.”
   - Once the process is complete, close AnyBurn.
4. Use the ISO file to Install Windows on a Virtual Machine OR use a program like [Rufus](https://rufus.ie/en/) or [Ventoy](https://github.com/ventoy/Ventoy) to create a bootable USB flash drive with the edited Windows ISO file.

> [!IMPORTANT]
> When using Rufus, don’t select any of the checkboxes in “Customize Your Windows Experience” as it creates another answer file, we don't want that.
5. Boot from the Windows Installation USB, do a clean install of Windows as normal, and the scripts will run automatically.
</br>

### <ins>Method 3: Use Ventoy Auto Install plugin</ins>

1. Download your preferred `autounattend.xml` file and save it on your computer.
2. Download the [Windows 10](https://www.microsoft.com/en-us/software-download/windows10) or [Windows 11](https://www.microsoft.com/en-us/software-download/windows11) ISO file, depending on the version you want.
3. Download and install [Ventoy](https://github.com/ventoy/Ventoy) to your desired USB flash drive. Before we get started, we have to prepare the folder structure:
    - In your newly created Ventoy disk, create the following folders: `ISO/Windows` and `Templates`. They should be at the root of the drive.
    - Put your ISOs in the newly created `ISO/Windows` folder.
    - Put your downloaded `autounattend.xml` into `Templates`.
    - You are ready to keep on going!
4. Start VentoyPlugson. Depending on your OS, the steps might differ. On Windows, run the `.exe` file. A browser window should open up with a Ventoy web interface ready to go.
5. Navigate to the Auto Install Plugin menu. In there, add a new entry.
    - Select [parent] to make the whole Windows ISO folder benefit from the plugin.
    - In the Directory Path, paste in the absolute path to your `ISO/Windows` folder.
    - In the Template Path, paste in the absolute path to your `autounattend.xml` file. (PSA: If you have more `autounattend.xml` files, you can add them later on!)
6. Restart your computer and boot from the Ventoy drive. After selecting a Windows ISO under the `ISO/Windows` path, you will be prompted to select the `autounattend.xml` file.
</br>

Enjoy your fresh Windows install!

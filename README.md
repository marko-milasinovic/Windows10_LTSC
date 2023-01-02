<h2 align="center"> Windows 10 LTSC first setup </h2> 

<h3 align="center"> A checklist for a complete first time setup </h3>

<p align="center">
<img src="https://img.shields.io/badge/Support-Windows%20x64-blue?logo=Windows&style=flat-square">
<img src="https://img.shields.io/github/license/marko-milasinovic/Windows10_LTSC?style=flat-square">
<img src="https://img.shields.io/github/directory-file-count/marko-milasinovic/Windows10_LTSC?style=flat-square">
</p>

# Requirements
1) Windows 10 Enterprise LTSC image
2) USB drive with at least 8GiB, final image file size on flash drive is ~4.8 GiB.
3) Host PC machine with at least 4GB of RAM for normal operation

### Table of Contents
 * [Windows system alterations](#windows-system-alterations)
   + [Registry](#registry)
   + [Services](#services)
   + [Group policy](#group-policy)
   + [Firewall changes](#firewall-changes)
   + [Turn Windows Features on or off](#turn-windows-features-on-or-off)
 * [Task Scheduler](#task-scheduler)
- [Program list](#program-list)
 * [DefaultApplications](#defaultapplications)
   + [Browsers](#browsers)
     - [Browser extensions](#browser-extensions)
 * [Intel drivers](#intel-drivers)
 * [Security / Peer clients](#security---peer-clients)
 * [Programming](#programming)
 * [Utilities](#utilities)
 * [Communication](#communication)
   + [File Explorer](#file-explorer)

<small><i><a href='http://ecotrust-canada.github.io/markdown-toc/'>Table of contents generated with markdown-toc</a></i></small>

# Preparation
1) **Unplug the ethernet** cable before installing windows
2) **Enable** default **Windows administrator** account and log in as administrator
```
net user administrator /active:yes
```
3) **Unzip** the provided **SystemFolders.7zip** file

## Windows system alterations
Start > PowerShell > Run the following [script](https://github.com/Sycnex/Windows10Debloater) as administrator:

```
iwr -useb https://git.io/debloat|iex
```
Disable Telemetry, OneDrive, remove bloatware regkeys, Stop Edge PDF
```
Edit Power Plan
```
* Start > Edit Power Plan > Select High performance > Change plan settings > Change advanced power settings > Processor power management > Maximum processor state: 99%
  * File explorer Address bar: Navigate to Power options > Left side menu: Choose what the power buttons do > Change settings that are currently unavailable > Disable: Turn on fast startup; Enable sleep.

```
View Advanced System Settings
```
* Start > View Advanced System Settings > Performance: Settings > Visual Effects > Select:
	* **Show windows content while dragging**
	* **Smooth edges of screen fonts**
  * Advanced > Automatically manage=DISABLE > Page file size: 
  		* **Initial=1024**, **Maximum=8192**

```
Automatically hide scroll bars in windows
```
* Start > Automatically hide scroll bars in windows -> Uncheck

```
%SystemRoot%\system32\drivers\etc\
```
* [Custom HOST file](https://github.com/StevenBlack/hosts#readme) - Edit **hosts** file found in: *Windows: %SystemRoot%\system32\drivers\etc\hosts*
(due to lag only ~16k entries)

### Registry
Disable windows update in Registry (Set "start"=4)
<p>
Run > <strong>REGEDIT</strong>

* HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\WaaSMedicSvc
* HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Wuaserv
* HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\UsoSvc
* HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\windefend
* x
* HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\WdNisDrv ??
* HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\WdNisSvc ??
* HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\msmpeng ?

### Services
Usefull [wiki](http://revertservice.com/10/) website for W10 service documentation
<p>
Run the following commands in Command Prompt (as an administrator):

```
sc config XboxGipSvc start= disabled 
& sc config xboxgip start= disabled 
& sc config xbgm start=disabled 
& sc config XblAuthManager start= disabled 
& sc config XblGameSave start=disabled 
& sc config workfolderssvc start= disabled 
& sc config wuauserv start= disabled 
& sc config WinRM start= disabled 
& sc config icssvc start= disabled 
& sc config WMPNetworkSvc start= disabled 
& sc config wisvc start= disabled 
& sc config SharedRealitySvc start= disabled 
& sc config RetailDemo start= disabled 
& sc config RetailDemo start= disabled 
& sc config RemoteAccess start= disabled 
& sc config shpamsvc start= disabled
```
<p>

```
sc config WFDSConMgrSvc start= disabled & sc config WFDSConMgrSvc start= disabled
```

### Disable Windows Updates
Run the following file: [Windows update blocker](https://www.sordum.org/9470/windows-update-blocker-v1-7/)

### Services to be disabled
* [WpcMonSvc] Parental Controls
* [Fax] Fax - Enables you to send and receive faxes, utilizing fax resources available on this computer or on the network
* [wisvc] Windows Insider Service - infrastructure support for the Windows Insider Program
* [RetailDemo] Retail Demo Service
* [DiagTrack] Connected User Experiences and Telemetry
* [XboxGipSvc] Xbox Accessory Management Service
* [XblAuthManager] Xbox Live Auth Manager
* [XboxNetApiSvc] Xbox Live Networking Service
* [XblGameSave] Xbox Live Game Save

Maps / geolocation are disabled
* [MapsBroker] Downloaded Maps Manager - application access to downloaded maps
* [lfsvc] Geolocation Service
* [SharedRealitySvc] Spatial Data Service

Offline files / file backups are disabled
* [CscService] Offline Files - performs maintenance activities on the Offline Files cache
* [fhsvc] File History Service
* [smphost] Microsoft Storage Spaces SMP
* [BDESVC] BitLocker Drive Encryption Service

Phone services
* [PhoneSvc] Phone Service - Manages the telephony
* [SmsRouter] Microsoft Windows SMS Router Service
* [icssvc] Windows Mobile Hotspot Service
* [irmon] Infrared monitor service
* [RmSvc] Radio Management Service

Ipv6 services:
* [IpxlatCfgSvc] IP Translation Configuration Service (ipv6)
* [iphlpsvc] IP Helper (ipv6)


Windows Update & Co.
* [InstallService] Microsoft Store Install Service
* [PushToInstall] Windows PushToInstall Service

Remote Access
* [RemoteAccess] Routing and Remote Access
* [RemoteRegistry] Remote Registry
* [TermService] Remote Desktop Services
* [SessionEnv] Remote Desktop Configuration

WiFi services:
* [WFDSConMgrSvc] Wi-Fi Direct Services Connection Manager Service

### Group policy
[Admx GP wiki](https://admx.help/?Category=Windows_10_2016&Policy=Microsoft.Policies.ControlPanel::ForceClassicControlPanel) - Website with detailed Group Policy documentation

```
Edit group policy
```
#### Change GP settings to the following:

Start > Edit group policy > **User Configuration** > Administrative Templates > All Settings:

* Always open All Control Panel Items when opening Control Panel = ENABLED
* Windows Automatic Updates = ENABLED
* Do not search Internet = ENABLED
* Do not send additional Data = ENABLED
* Do not keep history of recently opened documents = ENABLED

**Computer Configuration** > Administrative Templates > All Settings:
* Weather = DISABLED
* Configure Automatic Updates = DISABLED
* Allow Automatic Updates immediate installation = DISABLED
* Allow Automatic Update of Speech Data = DISABLED
* Allow Cloud Search = DISABLED
* Allow Clipboard History = DISABLED

**Control Panel**:
* Allow Online Tips = DISABLED

**Control Panel** > Personalization:
* Do not display the lock screen = ENABLED
* Prevent enabling lock screen camera = ENABLED
* Turn off automatic learning = ENABLED
* Allow users to enable online speech recognition services = DISABLED

**Microsoft Peer-to-Peer Networking Services**:
* ? Turn off Microsoft Peer-to-Peer Networking Services = ENABLED ?

**Offline Files**:
Allow or Disallow use of the Offline Files feature = DISABLED

**IPv6 Transition Technologies** > TCPIP Settings:
* Set 6to4 State = DISABLED

**Start Menu and Taskbar**:
* Remove "Recently added" list from Start Menu = ENABLED
* Remove frequent programs list from Start Menu = ENABLED

System > **Power Management**:
* Select an active power plan = ENABLED = 2 (High Performance)

System > **Remote Assistance**:
* Configure Solicited Remote Assistance = DISABLED
* Configure Offer Remote Assistance = DISABLED

System > **Storage Sense**
* Allow Storage Sense = DISABLED

System > **OS Policies**:
* Allow Clipboard History = DISABLED
* Allow Clipboard synchronization across devices = DISABLED
* Allow publishing of User Activities = DISABLED
* Allow upload of User Activities = DISABLED
* Enables Activity Feed = DISABLED

<d>
### Firewall changes

Copy **BlockAll.bat** to the following folder:

```
C:\Program Files\Microsoft Office\
```
* Run **BlockAll.bat** as administrator in the **\MicrosoftOffice** folder (to stop it from updating)

### Windows Firewall
```
Windows Defender Firewall with Advanced Security
```
Start > Windows Defender Firewall with Advanced Security > **Disable firewall rule** for the following:

**Inbound**: 
* Cortana
* Work or shool account
* Microsoft Lync
* Microsoft Lync UcMapi

**Outbound**:
* Microsoft Content
* Microsoft family features
* Narrator Quick Start
* Cortana
* Work or shool account
* Xbox Game UI
* Wi-FI Direct network discovery
* Wi-FI Direct Scan
* Wi-FI Direct Spooler

### Windows features

```
Turn Windows features on or off
```
Disable the following:
* Internet Explorer 11

## Task Scheduler

```
Task Scheduler
```
Start > Task Scheduler > Task Scheduler Library:
* **OneDrive Standalone Update Task** = DISABLED
* **Edge**  = DISABLED
* Microsoft > **XblGameSaveTask** = DISABLED
* Microsoft > Office:
	* **OfficeTelemetryAgentLogOn** = DISABLED
	* **OfficeTelemetryAgentFallBack** = DISABLED
	* **OfficeFeatureUpdates** = DISABLED
	* **OfficeFeatureUpdatesLogOn** = DISABLED
	* **OfficeAutomaticUpdates** = DISABLED
	* **OfficeClickToRunServiceMonitor** = DISABLED

# Program list
Abbreviations: 
* **FOSS** - Free and Open-Source Software
* **Freeware** - software, most often proprietary, that is distributed at no monetary cost to the end user
* **Shareware** - proprietary software which has trial use at little or no cost with usually limited functionality, but which can be upgraded upon payment

## DefaultApplications

 ```
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
```
* [Chocolatey](https://chocolatey.org/install) (FOSS) - Software management solution

```
choco install mpv
```
* [MPV - github](https://github.com/mpv-player/mpv) (FOSS) - optimised and simple media player | [Manual / wiki](https://mpv.io/manual/stable/#keyboard-control)
  * [MPV - chocolatey](https://community.chocolatey.org/packages/mpv)

* [Clementine](https://github.com/clementine-player/Clementine/releases/latest) (FOSS) - A "winamp" style music player
* [GreenShot](https://getgreenshot.org/downloads/) (FOSS) - A screenshot tool
* [Calibre](https://calibre-ebook.com/dist/win64) (FOSS) - Ebook viewer & manager
* [qimgv](https://github.com/easymodo/qimgv/releases/latest) (FOSS) - Image viewer
* [Inkscape](https://gitlab.com/inkscape/inkscape) (FOSS) - vector image editor
* [KeePass](https://keepass.info/download.html) (FOSS) - light-weight password manager
* [Notepad++](https://github.com/notepad-plus-plus/notepad-plus-plus/releases/latest) (FOSS) - multifunctional code/text editor
* [Revo Uninstaller](https://www.revouninstaller.com/start-freeware-download/) (Shareware) - advanced program uninstaller
* [7-zip with Zstandard](https://github.com/mcmilk/7-Zip-zstd/releases/latest) - file archiver with additional functions (eg. hash verification)
* [VLC](https://www.videolan.org/vlc/download-windows.html) (FOSS) - multimedia player and framework
* [Adobe Reader](https://get.adobe.com/reader/download?os=Windows+10&name=Reader+DC+2022.002.20191+English+Windows%2864Bit%29&lang=en&nativeOs=Windows+10&accepted=&declined=mss%2Cmsc%2Ccr&preInstalled=&site=otherversions) (Freeware) - Propriatery pdf reader
 * After installing disable the following: Start > Services > **Adobe Acrobat Update Service** = DISABLED

### Browsers
* [Ungoogled Chromium v101](https://github.com/Nifury/ungoogled-chromium-binaries/releases/download/101.0.4951.64/ungoogled-chromium_101.0.4951.64-1.1_installer_x64.exe/) (FOSS) - chromium browser without Google's libraries, based on the Chromium engine [versions 100.0.4896.60 / 199.0.4844.82-1]
  * as per: https://avoidthehack.com/manually-install-extensions-ungoogled-chromium#1downloadchromiumwebstore
  * download https://github.com/NeverDecaf/chromium-web-store/releases
  * change chrome://flags/#extension-mime-request-handling to "Always prompt for ins"
  * change chrome://extensions to enable Developer mode
  * drag the downloaded chromium webstore, install addons as per usual
  * Bookmarks C: Users /AppData /Local /Chromium /User Data /Default /Bookmarks
* [Vivaldi](https://vivaldi.com/download/) (Freeware) - customisable browser, based on the Chromium engine
* [Google Chrome](https://www.google.com/chrome/thank-you.html) (Freeware) - made by Google, based on the Chromium engine

#### Browser extensions
* [UBlock Origin](https://github.com/gorhill/uBlock) (FOSS) - An efficient (ad) blocker for Chromium and Firefox
* [HTTPS Everywhere](https://www.eff.org/https-everywhere/) (FOSS) - encrypts your communications with many major websites
* [ClearUrls](https://gitlab.com/KevinRoebert/ClearUrls) (FOSS) - based on the new WebExtensions technology, automatically removes tracking elements from URLs

## Intel drivers
* [Intel's bluetooth driver](https://www.intel.com/content/www/us/en/support/articles/000005489/wireless/intel-wireless-products.html) (Freeware) - a bluetooth driver that works
* [Intel's gpu driver](https://www.intel.com/content/www/us/en/support/products/80939/graphics.html) (Freeware) - For the specific gpu line

## Security / Peer clients
* [QBitTorrent](https://www.qbittorrent.org/download.php) - a bittorrent client


## Programming
* [Pyhon + pip](https://www.python.org/downloads/windows/) (FOSS) - python with a software management solution
* [VirtualBox](https://www.virtualbox.org/wiki/Downloads) - x86 AMD64/Intel64 full virtualization
* [NodeJS](https://nodejs.org/en/download/) - a JavaScript runtime built on Chrome's V8 JavaScript engine
* [XAMPP](https://www.apachefriends.org/download.html) - Apache distribution containing MariaDB, PHP, and Perl
* [Anaconda3](https://www.anaconda.com/products/distribution#Downloads) - package / environment manager,  Python3 distribution with 1,500+ open source packages
* [IntelliJ Idea](https://www.jetbrains.com/idea/) - for Java, with assistance for a variety of other languages such as SQL, JPQL, HTML, JavaScript
* [WebStorm](https://www.jetbrains.com/webstorm/) - for JavaScript and related technologies
* [Visual Studio Code](https://github.com/VSCodium/vscodium/releases/latest) (FOSS) - JavaScript, TypeScript, Node.js (C++, C#, Java, Python, PHP, Go, .NET, Unity)
* [Eclipse](https://www.eclipse.org/downloads/packages/release) - oxygen release? - mainly for Java Integrated Development Environment
  
## Utilities
* [DeepL](https://appdownload.deepl.com/windows/0install/DeepLSetup.exe) (Freeware) - Auto translate tool
* [Freeplane](https://github.com/freeplane/freeplane/releases) (FOSS) - Tools for mind mapping
* [PowerToys](https://docs.microsoft.com/en-us/windows/powertoys/install) (Microsoft) - Usefull windows tools
* [HWmonitor](https://www.cpuid.com/softwares/hwmonitor.html) (Shareware) - general purpose hardware monitoring program
* [ffmpeg](https://ffmpeg.org/download.html) (FOSS) - command line multimedia framework (a complete, cross-platform solution to record, convert and stream audio and video)
  * ```
    choco install ffmpeg
    ```
* [MKVToolNix](https://mkvtoolnix.download/downloads.html#windows) (FOSS) - gui for working with Matroska files [gitlab](https://gitlab.com/mbunkus/mkvtoolnix)
  * ```
    choco install mkvtoolnix
    ```
## Communication
* [Telegram](https://desktop.telegram.org/) (Shareware) - unsecure chat app with phone contact synchronisation
* [Discord](https://discord.com/download) (Shareware) - unsecure chat app with chat rooms
* [Steam](https://store.steampowered.com/about/) (Shareware) - unsecure app

### File Explorer
* Go to the Last Active window with a single click / Switch to last opened window

Computer\HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced
> Right-click on Advanced> New> DWORD (32-bit) Value. Rename it to LastActiveClick
> Double click on LastActiveClick and change its value to 1


= = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = = 
HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer\MyComputer\NameSpace
> HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Explorer\MyComputer\NameSpace
> add minus infront of -{0DB7E03F-FC29-4DC6-9020-FF41B59E513A}


* Disable the LockScreen (Optional)
> Computer\HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Microsoft\Windows
> Right-click on Windows> New> Key, rename it to Personalization
> Right-click on Personlization> New> DWORD 32. Rename it to NoLockscreen
> Double-click on it “NoLockscreen” and change the value to 1

* Disable Action Center (Optional)
> Computer\HKEY_CURRENT_USER\Software\Policies\Microsoft\Windows
> Right-click on Windows> New> Key. Rename it to Explorer.
> Right-click on Explorer> New> DWORD (32-bit) Value. Rename it to DisableNotificationCenter.
> Double-click on DisableNotificationCenter and change the value to 1.

* [WinTweaker](https://www.thewindowsclub.com/ultimate-windows-tweaker-4-windows-10) (Freeware) - Windows settings tweaker

* Enable verbose status messages
> HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\Policies\System
> System entry and then right-click on the white space in the right panel and select New > DWORD (32-bit) Value.
> verbosestatus = 1

* Apps in the context menu [GeekFlare](https://geekflare.com/windows-11-registry-tweak/)
> HKEY_CLASSES_ROOT\Directory\Background\shell






* Windows search: [askVG](https://www.askvg.com/collection-of-registry-tweaks-for-windows-7/)
> HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer
> “LinkResolveIgnoreLinkInfo”=dword:00000001
> “NoResolveSearch”=dword:00000001
> “NoResolveTrack”=dword:00000001
> “NoInternetOpenWith”=dword:00000001



### Optional
* [Tor](https://www.torproject.org/download/) (FOSS) - browser focused on anonymity and security, based on firefox

### Optional
* [Crystalmark](https://crystalmark.info/en/download/) (FOSS) - benchmark software that measures the transfer speed of media data storage
* [Process Monitor](https://docs.microsoft.com/en-us/sysinternals/downloads/procmon) (Propriatery) - Microsoft's advanced monitoring tool that shows real-time file system, Registry and process/thread activity
* [Process Explorer](https://docs.microsoft.com/en-us/sysinternals/downloads/process-explorer) (Propriatery) - shows information about which handles and DLLs processes have opened or loaded
* [Rufus](https://github.com/pbatard/rufus/releases) (FOSS) - USB Formatting Utility
* [PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html) (FOSS ?) - SSH, Telnet, and SFTP client, typically used for remote access to server computers over a network using the SSH protocol along with an xterm terminal emulator
* [ImDIsk](https://www.ltr-data.se/opencode.html/#ImDisk) (FOSS) - virtual disk driver

* [Arduino](https://www.arduino.cc/en/software) (FOSS) - Arduino Software IDE
* [Total Commander](https://www.ghisler.com/download.htm) (Shareware) - tried and true file manager

## Optional expansion hardware apps
* [Logitech G-Hub](https://www.logitechg.com/en-us/innovation/g-hub.html) (Freeware) - propriatery Logitech software for peripherals
* [Thunderbird](https://www.thunderbird.net/en-US/download/) - an E-Mail client

### Optional
* [WireGuard](https://www.wireguard.com/) - fast, secure vpn tunnel
* [GNU Privacy Guard](https://gnupg.org/) - free implementation of the OpenPGP standard, that allows you to encrypt and sign your data and communications

### Optional
* [XWiki](https://xwiki.com/en/offerings/products/xwiki-standard) (FOSS) - WebServer wiki
* [OpenCRX](https://github.com/opencrx/opencrx) (FOSS) - Customer relationship management software

## Optional Image manipulation 
* [Paint.net](https://www.getpaint.net/doc/latest/InstallPDN.html) (Freeware) - raster graphics editor, developed on the.NET Framework

### Optional video software
* [Waifu2x Caffe](https://github.com/lltcggie/waifu2x-caffe/releases/latest) (FOSS) - image denoiser & upscaler (slow, best used with Nvidia GPU's)
  * [Intel OpenVino](https://docs.openvinotoolkit.org/latest/openvino_docs_install_guides_installing_openvino_windows.html) - speeds up Waifu2x Caffe
  * [Waifu2x Vulkan](https://github.com/nihui/waifu2x-ncnn-vulkan/) (FOSS) - image denoiser & upscaler (faster but less accurate)
* [Waifu2x Extension GUI](https://github.com/AaronFeng753/Waifu2x-Extension-GUI/releases/latest) video & image denoiser & upscaler with multiple algorithms

* [Ruby - chocolatey](https://community.chocolatey.org/packages/ruby) 
	```
	choco install ruby
	```
  * [Other Video Transcoding](https://github.com/donmelton/other_video_transcoding) (FOSS) - highly efficient transcoding cli for h.264 to h.265
  	```
    gem install other_video_transcoding
    ```
  
* [Dandere 2x](https://github.com/akai-katto/dandere2x/releases/latest) (FOSS) - Efficient video upscaler that uses waifu2x and video compression technology
<hr>

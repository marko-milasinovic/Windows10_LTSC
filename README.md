<h2 align="center"> Windows 10 LTSC first setup </h2> 

<h3 align="center"> A checklist for a complete first time windows ameliorated setup </h3>

<p align="center">
<img src="https://img.shields.io/badge/Support-Windows%20x64-blue?logo=Windows&style=flat-square">
<img src="https://img.shields.io/github/license/marko-milasinovic/w10_first_setup?style=flat-square">
</p>

Windows 10 [Ameliorated (build 1903)](https://ameliorated.info/index.html)
```
magnet:?xt=urn:btih:F6AE4B1303E0D800B329F3DDA1170D7E9865D93D&dn=AME_1903_%282020-02-06%29.iso&tr=https%3a%2f%2f1337.abcvg.info%3a443%2fannounce
```

## Excerp from [Archive.org's](https://archive.org/details/windows-10-ameliorated-1903) explanation regarding Windows 10 AME
Windows 10 AME aims at delivering a stable, non-intrusive yet fully functional build of Windows 10 to anyone, who requires the Windows operating system natively. 
 
Spyware systems, which are abundant in Windows 10 by default, have not been disabled using group policy, registry entries or various other workarounds – they have been entirely removed and deleted from the system, on an executable-level. This includes Windows Update, and any related services intended to re-patch the system via what is essentially a universal backdoor. Core applications, such as the included Edge web-browser, Windows Media Player, Cortana, as well as any appx applications, have also been successfully eliminated. The total size of removed files is about 2 GB.
 
Final image file size is 5.35 Gb.

##### Table of Contents
  * [Credentials](#credentials)
  * [Windows system alterations](#windows-system-alterations)
- [Program list](#program-list)
  * [Browsers](#browsers)
    + [Optional](#optional)
    + [Browser extensions](#browser-extensions)
  * [Utilities](#utilities)
    + [Optional](#optional-1)
  * [Communication](#communication)
  * [Programming](#programming)
    + [Optional](#optional-2)
  * [Integrated Development Environments (IDE)](#integrated-development-environments--ide-)
    + [Optional](#optional-3)
  * [Optional expansion hardware apps](#optional-expansion-hardware-apps)
  * [Security / Peer clients](#security---peer-clients)
    + [Optional](#optional-4)
  * [Documentation](#documentation)
    + [Optional](#optional-5)
  * [Image manipulation](#image-manipulation)
    + [Optional](#optional-6)
  * [Video manipulation](#video-manipulation)
    + [Optional](#optional-7)
  * [Music playback](#music-playback)
  * [Testing](#testing)
  * [Removed](#removed)
  * [Additional info](#additional-info)
  * [Incompatible](#incompatible)

<small><i><a href='http://ecotrust-canada.github.io/markdown-toc/'>Table of contents generated with markdown-toc</a></i></small>


## Credentials
| username | password | generic key |
|:----:|:-----------:|:----:|
user | malte | 2B87N-8KFHP-DKV6R-Y2C8J-PKCKT

## Windows system alterations
* [Custom HOST file](https://github.com/StevenBlack/hosts#readme) - Can be found in: *Windows: %SystemRoot%\system32\drivers\etc\hosts* (due to lag only ~16k entries)
* Disabled QuickBoot
* Performance display settings
* Page file size (both min and max) = ram size + 256
* in power settings max pci processor state = 99%
* regedit change for auto login functionality

# Program list
Abbreviations: 
* FOSS - Free and Open-Source Software
* Freeware - software, most often proprietary, that is distributed at no monetary cost to the end user
* Shareware - proprietary software which has trial use at little or no cost with usually limited functionality, but which can be upgraded upon payment

## Package managers
* [Chocolatey](https://chocolatey.org/install) (FOSS) - software management solution
  * ```
    Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))
    ```
* [Pyhon + pip](https://www.python.org/downloads/windows/) (FOSS) - python with a software management solution

## Windows configs
* System configuration > Microsoft Store install service > Disable
* Task Scheduler > Disable Edge
* Windows Features > Disable Internet Explorer
* PageFile size = 8Gb
* Edit Power plan > High Performance > 99% max cpu state

### File Explorer
* Go to the Last Active window with a single click 
> Computer\HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced
> Right-click on Advanced> New> DWORD (32-bit) Value. Rename it to LastActiveClick
> Double click on LastActiveClick and change its value to 1

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

* Switch to last opened window
> HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Explorer\Advanced
> Advanced entry is selected, right-click on the white space in the right panel and select New > DWORD (32-bit) Value.
> LastActiveClick = 1

* Windows search: [askVG](https://www.askvg.com/collection-of-registry-tweaks-for-windows-7/)
> HKEY_CURRENT_USER\Software\Microsoft\Windows\CurrentVersion\Policies\Explorer
> “LinkResolveIgnoreLinkInfo”=dword:00000001
> “NoResolveSearch”=dword:00000001
> “NoResolveTrack”=dword:00000001
> “NoInternetOpenWith”=dword:00000001

### Windows services
 ```
 net stop wuauserv 
 ```
 ```
 sc config wuauserv start= disabled
 ```
Start Menu > Services > Windows Update > Disable the following:
* Xbox Live Networking Service (XboxNetApiSvc)
* Xbox Accessory Management Service (XboxGipSvc)
* Xbox Live Game Save (XblGameSave)
* Xbox Live Auth Manager (XblAuthManager)
* autotimesvc
* DiagTrack
* dmwappushservice
* FrameServer
* icssvc
* InstallService
* MapsBroker
* PhoneSvc
* RasMan
* RemoteAccess
* RemoteRegistry
* SessionEnv
* SysMain ?
* UevAgentService
* UmRdpService
* WalletService
* WbioSrvc
* wisvc
* WMPNetworkSvc
* WpcMonSvc
* WSearch
* ALG ?
* AppReadiness ?
* BITS
* Microsoft (R) Diagnostics Hub Standard Collector Service (diagnosticshub.standardcollector.service)
* Enterprise App Management Service (EntAppSvc)
* MicrosoftEdgeElevationService
* MixedRealityOpenXRSvc
* Windows Perception Simulation Service (perceptionsimulation)
* Performance Logs & Alerts (pla)
* RetailDemo
* Windows Backup (SDRSVC)
* Radio Management Service (RmSvc)
* Spatial Data Service (SharedRealitySvc)
* Smart Card Removal Policy (SCPolicySvc)
* Microsoft Storage Spaces SMP (smphost)
* ??? Windows Defender Advanced Threat Protection Service (Sense)
* Microsoft Software Shadow Copy Provider (swprv)
* Telephony (TapiSrv)
* Remote Desktop Services (TermService)
* ?????? Web Account Manager (TokenBroker)
* Volume Shadow Copy (VSS)
* Windows Update Medic Service (WaaSMedicSvc) 
//added in update 1803 
//Computer Configuration \ Preferences \ Windows Settings \ Registry 
//HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\WaaSMedicSvc
//value name is Start and please set the value to 4.
// Source [technet](https://social.technet.microsoft.com/Forums/en-US/8706fda2-f7cd-4dab-814b-72283b17c423/how-to-disable-windows-update-medic-service)
* BitLocker Drive Encryption Service (BDESVC)
* Offline Files (CscService)
* Microsoft Edge Update Service (edgeupdatem)
* IP Translation Configuration Service (IpxlatCfgSvc)
* Geolocation Service (lfsvc)
* Microsoft Passport (NgcSvc)
* Microsoft Passport Container (NgcCtnrSvc)
* Windows PushToInstall Service (PushToInstall)
* Microsoft Windows SMS Router Service (SmsRouter)
* Windows Perception Service (spectrum)
* Touch Keyboard and Handwriting Panel Service (TabletInputService)
* Microsoft Account Sign-in Assistant (wlidsvc)
* Windows Update (wuauserv)


* LGHUB Updater Service (LGHUBUpdaterService)
## Bios Configs
* Fixed ram speed / XMP
* TBD

## Browsers
* [Ungoogled Chromium](https://ungoogled-software.github.io/ungoogled-chromium-binaries/releases/windows/64bit/) (FOSS) - chromium browser without Google's libraries, based on the Chromium engine [versions 100.0.4896.60 / 199.0.4844.82-1]
  * as per: https://avoidthehack.com/manually-install-extensions-ungoogled-chromium#1downloadchromiumwebstore
  * download https://github.com/NeverDecaf/chromium-web-store/releases
  * change chrome://flags/#extension-mime-request-handling to "Always prompt for ins"
  * change chrome://extensions to enable Developer mode
  * drag the downloaded chromium webstore, install addons as per usual
* [Vivaldi](https://vivaldi.com/download/) (Freeware) - customisable browser, based on the Chromium engine

### Optional
* [Chrome](https://www.google.com/chrome/) (Freeware) - made by Google, based on the Chromium engine
* [Tor](https://www.torproject.org/download/) (FOSS) - browser focused on anonymity and security, based on firefox

### Browser extensions
* [UBlock Origin](https://github.com/gorhill/uBlock) (FOSS) - An efficient (ad) blocker for Chromium and Firefox
* [HTTPS Everywhere](https://www.eff.org/https-everywhere/) (FOSS) - encrypts your communications with many major websites
* [ClearUrls](https://gitlab.com/KevinRoebert/ClearUrls) (FOSS) - based on the new WebExtensions technology, automatically removes tracking elements from URLs

## Utilities
* [KeePass](https://keepass.info/download.html) (FOSS) - light-weight password manager
* [Notepad++](https://github.com/notepad-plus-plus/notepad-plus-plus/releases/latest) (FOSS) - multifunctional code/text editor
* [Revo Uninstaller](https://www.revouninstaller.com/start-freeware-download/) (Shareware) - advanced program uninstaller
* [7-zip with Zstandard](https://github.com/mcmilk/7-Zip-zstd/releases/latest) - file archiver with additional functions (eg. hash verification)
* [PowerToys](https://docs.microsoft.com/en-us/windows/powertoys/) (Microsoft) - Usefull windows tools

### Optional
* [HWmonitor](https://www.cpuid.com/softwares/hwmonitor.html) (Shareware) - general purpose hardware monitoring program
* [Crystalmark](https://crystalmark.info/en/download/) (FOSS) - benchmark software that measures the transfer speed of media data storage
* [ffmpeg](https://ffmpeg.org/download.html) (FOSS) - command line multimedia framework (a complete, cross-platform solution to record, convert and stream audio and video)
  * ```
    choco install ffmpeg
    ```
* [MKVToolNix](https://mkvtoolnix.download/downloads.html#windows) (FOSS) - gui for working with Matroska files [gitlab](https://gitlab.com/mbunkus/mkvtoolnix)
* [Process Monitor](https://docs.microsoft.com/en-us/sysinternals/downloads/procmon) (Propriatery) - Microsoft's advanced monitoring tool that shows real-time file system, Registry and process/thread activity
* [Process Explorer](https://docs.microsoft.com/en-us/sysinternals/downloads/process-explorer) (Propriatery) - shows information about which handles and DLLs processes have opened or loaded
* [Total Commander](https://www.ghisler.com/download.htm) (Shareware) - tried and true file manager


### Optional
* [WireShark](https://www.wireshark.org/#download) (FOSS) - network protocol analyzer
* [Koodo Reader](https://github.com/troyeguo/koodo-reader/releases/latest) (FOSS) - full-featured ebook manager, albeit slightly impractical
* [Calibre](https://calibre-ebook.com/download_windows) (FOSS) - e-book viewer & manager
 * [Sumatra PDF](https://github.com/sumatrapdfreader/sumatrapdf/releases/latest) (FOSS) - multi-format (PDF, EPUB, MOBI, FB2, CHM, XPS, DjVu) reader for Windows 

## Communication
* [Telegram](https://desktop.telegram.org/) (Shareware) - unsecure chat app with phone contact synchronisation
* [Discord](https://discord.com/download) (Shareware) - unsecure chat app with chat rooms
* [Steam](https://store.steampowered.com/about/) (Shareware) - unsecure app

## Programming
* [VirtualBox](https://www.virtualbox.org/wiki/Downloads) - x86 AMD64/Intel64 full virtualization
* [NodeJS](https://nodejs.org/en/download/) - a JavaScript runtime built on Chrome's V8 JavaScript engine
* [XAMPP](https://www.apachefriends.org/download.html) - Apache distribution containing MariaDB, PHP, and Perl
* [Anaconda3](https://docs.anaconda.com/anaconda/install/hashes/win-3-64/) - package / environment manager,  Python3 distribution with 1,500+ open source packages
  * [Anaconda-alternate](https://www.anaconda.com/products/individual) - data science platform and package manager
* [Rufus](https://github.com/pbatard/rufus/releases) (FOSS) - USB Formatting Utility
* [PuTTY](https://www.chiark.greenend.org.uk/~sgtatham/putty/latest.html) (FOSS ?) - SSH, Telnet, and SFTP client, typically used for remote access to server computers over a network using the SSH protocol along with an xterm terminal emulator

### Optional
* [ImDIsk](https://www.ltr-data.se/opencode.html/#ImDisk) (FOSS) - virtual disk driver

## Integrated Development Environments (IDE)
* [IntelliJ Idea](https://www.jetbrains.com/idea/) - for Java, with assistance for a variety of other languages such as SQL, JPQL, HTML, JavaScript
* [WebStorm](https://www.jetbrains.com/webstorm/) - for JavaScript and related technologies
* [Visual Studio Code](https://github.com/VSCodium/vscodium/releases/latest) (FOSS) - JavaScript, TypeScript, Node.js (C++, C#, Java, Python, PHP, Go, .NET, Unity)
* [Eclipse](https://www.eclipse.org/downloads/packages/release) - oxygen release?

### Optional
* [Arduino](https://www.arduino.cc/en/software) (FOSS) - Arduino Software IDE
* [Eclipse](https://www.eclipse.org/downloads/packages/release/2021-06/r/eclipse-ide-enterprise-java-and-web-developers) - mainly for Java Integrated Development Environment

## Optional expansion hardware apps
* [Logitech G-Hub](https://www.logitechg.com/en-us/innovation/g-hub.html) (Freeware) - propriatery Logitech software for peripherals
* [Intel's bluetooth driver](https://www.intel.com/content/www/us/en/support/articles/000005489/wireless/intel-wireless-products.html) - a bluetooth driver

## Security / Peer clients
* [QBitTorrent](https://www.qbittorrent.org/download.php) - a bittorrent client
* [SimpleDNSCrypt](https://simplednscrypt.org/) - management tool for configuring the dnscrypt-proxy service (introduces latency)
* [Thunderbird](https://www.thunderbird.net/en-US/download/) - an E-Mail client

### Optional
* [WireGuard](https://www.wireguard.com/) - fast, secure vpn tunnel
* [GNU Privacy Guard](https://gnupg.org/) - free implementation of the OpenPGP standard, that allows you to encrypt and sign your data and communications
* [PasswordStore](https://www.passwordstore.org/) - by https://www.zx2c4.com/

## Documentation
* [Freeplane](https://github.com/freeplane/freeplane/releases) (FOSS) - tools for mind mapping

### Optional
* [XWiki](https://xwiki.com/en/offerings/products/xwiki-standard) (FOSS) - WebServer wiki
* [OpenCRX](https://github.com/opencrx/opencrx) (FOSS) - Customer relationship management software

## Image manipulation 
* [qimgv](https://github.com/easymodo/qimgv/releases/latest) (FOSS) - Image viewer

### Optional
* [Inkscape](https://gitlab.com/inkscape/inkscape) (FOSS) - vector image editor
* [Waifu2x Caffe](https://github.com/lltcggie/waifu2x-caffe) (FOSS) - image denoiser & upscaler (slow, best used with Nvidia GPU's)
  * [Intel OpenVino](https://docs.openvinotoolkit.org/latest/openvino_docs_install_guides_installing_openvino_windows.html) - speeds up Waifu2x Caffe
  * [Waifu2x Vulkan](https://github.com/nihui/waifu2x-ncnn-vulkan/) (FOSS) - image denoiser & upscaler (faster but less accurate)

### Optional
* [Paint.net](https://www.getpaint.net/doc/latest/InstallPDN.html) (Freeware) - raster graphics editor, developed on the.NET Framework

## Video manipulation
* [VLC](https://www.videolan.org/vlc/download-windows.html) (FOSS) - multimedia player and framework
* [MPV - github](https://github.com/mpv-player/mpv) (FOSS) - optimised and simple media player
  * [MPV - chocolatey](https://community.chocolatey.org/packages/mpv) 
  * ```
    choco install mpv
    ```

### Optional
* [Handbrake](https://handbrake.fr/downloads.php) (FOSS) - video transcoder
  * [Ruby - chocolatey](https://community.chocolatey.org/packages/ruby) 
  * ```
    choco install ruby
    ```
  * [video_transcoding](https://github.com/donmelton/video_transcoding) (FOSS) - highly efficient transcoding cli for h.264
  * [other_video_transcoding](https://github.com/donmelton/other_video_transcoding) (FOSS) - highly efficient transcoding cli for h.264 to h.265
* [Dandere2x](https://github.com/akai-katto/dandere2x/releases/latest) (FOSS) - efficient video upscaler that uses waifu2x and video compression technology

### Optional
* [ScreenToGif](https://github.com/NickeManarin/ScreenToGif/releases/latest) (FOSS) - allows recording of a selected area of your screen, as well as edit and save it as a gif or video.

## Music playback
* [Clementine](https://github.com/clementine-player/Clementine/releases/latest) (FOSS) - a "winamp" style music player

## Testing
* [FogProject](https://fogproject.org/) (FOSS) - network computer cloning and management solution
 
<hr>

## Removed
* [Hamachi](https://vpn.net/) - due to licensing changes
* [Adobe Reader](https://get.adobe.com/reader/) (Shareware) - Adobe's pdf reader, too much bloat if not necessary
* [Notable](https://github.com/notable/notable/tree/v1.5.1) (FOSS) - Up to version v1.5.1 FOSS, later Freeware, note taking app, i just dont find it intuitive, nor usefull
* [Claws Mail](https://claws-mail.org/win32/) (FOSS) - secure multiplatform mail client with OpenPGP implementation //while nice takes time to set up

## Additional info
* List of [Windows Terminal Commands](https://ss64.com/nt/)
* Windows [setup script](https://github.com/Disassembler0/Win10-Initial-Setup-Script)
* -- testing https://pc.wps.com/
* -- testing https://github.com/chrdavis/SmartRename

## Incompatible
* [Custom Files gui](https://github.com/files-community/Files) - cannot be implemented due to removed MS store framework

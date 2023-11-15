Installation notes from when I install my Windows 10 machine for penetration testing. Focussing on On-Prem Active Directory and Azure. I wrote these notes down so it is easier to reinstall my system. Posted it publicly as inspiration for others. If you have any good tool recommendations feel free to DM me on discord 0xjs#9027.

# Install W10
- Download W10 ISO from [Evaluation Center](https://www.microsoft.com/en-us/evalcenter/evaluate-windows-10-enterprise) or with the [Media Creation Tool](https://www.microsoft.com/en-us/software-download/windows10) and install Windows 10 Enterprise or Pro
- Install all updates, restart and repeat till there are no more updates
- Disable stuff you want (cortana, search, task, news and interest etc) from the taskbar and update windows! Restart the machine a couple of times till there are no updates left.
- Open defender and disable `Cloud-delivered protection` and `Automatic sample submission`

## Download tools manually
- [Vistual Studio 2022](https://visualstudio.microsoft.com/thank-you-downloading-visual-studio/?sku=Community&channel=Release&version=VS2022&source=VSLandingPage&cid=2030&passive=false) and install it. 
  - Make sure to select ".Net Desktop Development" and "Desktop Development with C++"
- [Visual Studio 2019](https://visualstudio.microsoft.com/thank-you-downloading-visual-studio/?sku=Community&rel=16&src=myvs&utm_medium=microsoft&utm_source=my.visualstudio.com&utm_campaign=download&utm_content=vs+community+2019) or [here](https://visualstudio.microsoft.com/vs/older-downloads/) and install it. 
  - Make sure to select ".Net Desktop Development" and "Desktop Development with C++"
- Open internet explorer and select use recommended settings.
- [Process hacker](https://processhacker.sourceforge.io/downloads.php)
- [PE Bear](https://github.com/hasherezade/pe-bear-releases)
- [x64dbg](https://x64dbg.com/)
- [APIMonitor](http://www.rohitab.com/downloads)

## Tools PowerShell
```
# Create directories
Set-Location C:\; New-Item "C:\Tools\" -ItemType "Directory"; New-Item "C:\Tools\AD" -ItemType "Directory"; New-Item "C:\Tools\Azure" -ItemType "Directory"; New-Item "C:\Tools\Evasion" -ItemType "Directory"; New-Item "C:\Tools\Misc" -ItemType "Directory"

# Add exclusion for C:\Tools\ and set execution policy unrestricted
Set-MpPreference -ExclusionPath C:\Tools\*, C:\users\*\.shiv\*, C:\users\*\.cme\*

# Download some tools with winget
winget install -e --id Python.Python.3.11
winget install -e --id 9N0DX20HK701
winget install -e --id Notepad++.Notepad++
winget install -e --id Git.Git
winget install -e --id OpenVPNTechnologies.OpenVPN

Set-ExecutionPolicy -ExecutionPolicy Unrestricted
Get-WindowsCapability -Name RSAT* -Online | Add-WindowsCapability –Online

# Download AD tools
Set-Location C:\Tools\AD
git clone https://github.com/samratashok/ADModule
git clone https://github.com/PowerShellMafia/PowerSploit
git clone https://github.com/samratashok/nishang
git clone https://github.com/r3motecontrol/Ghostpack-CompiledBinaries
git clone https://github.com/PowerShell/GPRegistryPolicyParser
git clone https://github.com/Kevin-Robertson/Invoke-TheHash
git clone https://github.com/Kevin-Robertson/Powermad
git clone https://github.com/S3cur3Th1sSh1t/PowerSharpPack
git clone https://github.com/NetSPI/PowerUpSQL
git clone https://github.com/itm4n/PrivescCheck
git clone https://github.com/Dliv3/SharpGPO
git clone https://github.com/leechristensen/SpoolSample
git clone https://github.com/maaaaz/impacket-examples-windows
git clone https://github.com/dafthack/MailSniper
git clone https://github.com/BloodHoundAD/BloodHound
git clone https://github.com/mandiant/ADFSDump
git clone https://github.com/szymex73/ADFSpoof
git clone https://github.com/PowerShellMafia/PowerSCCM

Invoke-WebRequest https://www.heidisql.com/downloads/releases/HeidiSQL_12.1_64_Portable.zip -OutFile HeidiSQL_12.1_64_Portable.zip; Expand-Archive HeidiSQL_12.1_64_Portable.zip; Remove-Item HeidiSQL_12.1_64_Portable.zip
Invoke-WebRequest https://github.com/MichaelGrafnetter/DSInternals/releases/download/v4.7/DSInternals_v4.7.zip -OutFile DSInternals_v4.7.zip; Expand-Archive DSInternals_v4.7.zip; Remove-Item DSInternals_v4.7.zip
Invoke-WebRequest https://github.com/Porchetta-Industries/CrackMapExec/releases/download/v5.3.0/cme-windows-latest-3.10.zip -OutFile cme-windows-latest-3.10.zip; Expand-Archive cme-windows-latest-3.10.zip; Remove-Item cme-windows-latest-3.10.zip

# Download Evasion tools
Set-Location C:\Tools\Evasion\
git clone https://github.com/iomoath/PowerShx
git clone https://github.com/rasta-mouse/ThreatCheck
git clone https://github.com/yoda66/PowerStrip
git clone https://github.com/hfiref0x/UACME
git clone https://github.com/danielbohannon/Invoke-Obfuscation

Invoke-WebRequest https://github.com/mkaring/ConfuserEx/releases/download/v1.6.0/ConfuserEx-GUI.zip -OutFile ConfuserEx-GUI.zip; Expand-Archive ConfuserEx-GUI.zip; Remove-Item ConfuserEx-GUI.zip

# Download Azure Tools
Set-Location C:\Tools\Azure\
Invoke-WebRequest https://psg-prod-eastus.azureedge.net/packages/azuread.2.0.2.140.nupkg -OutFile azuread.2.0.2.140.zip; Expand-Archive azuread.2.0.2.140.zip; Remove-Item azuread.2.0.2.140.zip
Invoke-WebRequest https://psg-prod-eastus.azureedge.net/packages/azureadpreview.2.0.2.149.nupkg -OutFile azureadpreview.2.0.2.149.zip; Expand-Archive azureadpreview.2.0.2.149.zip; Remove-Item azureadpreview.2.0.2.149.zip
Invoke-WebRequest https://psg-prod-eastus.azureedge.net/packages/msonline.1.1.183.66.nupkg -OutFile msonline.1.1.183.66.zip; Expand-Archive msonline.1.1.183.66.zip; Remove-Item msonline.1.1.183.66.zip
Invoke-WebRequest https://psg-prod-eastus.azureedge.net/packages/microsoft.graph.1.7.0.nupkg -OutFile microsoft.graph.1.7.0.zip; Expand-Archive microsoft.graph.1.7.0.zip; Remove-Item microsoft.graph.1.7.0.zip
Invoke-WebRequest https://psg-prod-eastus.azureedge.net/packages/az.7.1.0.nupkg -OutFile az.7.1.0.zip; Expand-Archive az.7.1.0.zip; Remove-Item az.7.1.0.zip
Invoke-WebRequest https://github.com/BloodHoundAD/AzureHound/releases/download/v1.2.1/azurehound-windows-amd64.zip -OutFile azurehound-windows-amd64.zip; Expand-Archive azurehound-windows-amd64.zip; Remove-Item azurehound-windows-amd64.zip
Invoke-WebRequest https://psg-prod-eastus.azureedge.net/packages/exchangeonlinemanagement.3.0.0.nupkg -OutFile exchangeonlinemanagement.3.0.0.zip; Expand-Archive exchangeonlinemanagement.3.0.0.zip; Remove-Item exchangeonlinemanagement.3.0.0.zip

git clone https://github.com/NetSPI/MicroBurst
git clone https://github.com/Gerenios/AADInternals
git clone https://github.com/dafthack/MSOLSpray
git clone https://github.com/dafthack/MFASweep
git clone https://github.com/Azure/Stormspotter
git clone https://github.com/dirkjanm/ROADtools
git clone https://github.com/0xJs/AzurePowerCommands

Install-Module -Name Az -Repository PSGallery -Force -Confirm
Install-Module -Name Microsoft.Graph -Repository PSGallery -Force -Confirm
Install-Module -Name Microsoft.Graph.Intune -Force -Confirm

# General tools
Set-Location C:\Tools\Misc
Invoke-WebRequest https://github.com/islamadel/bat2exe/archive/refs/tags/2.0.zip -outfile 2.0.zip; Expand-Archive 2.0.zip; Remove-Item 2.0.zip; Move-Item .\2.0\bat2exe-2.0\ C:\Tools\Misc\; Remove-Item C:\Tools\Misc\2.0\
Invoke-WebRequest https://github.com/microsoft/etl2pcapng/releases/download/v1.7.0/etl2pcapng.zip -OutFile etl2pcapng.zip; Expand-Archive etl2pcapng.zip; Remove-Item etl2pcapng.zip
Invoke-WebRequest https://download.sysinternals.com/files/SysinternalsSuite.zip -OutFile SysinternalsSuite.zip; Expand-Archive SysinternalsSuite.zip; Remove-Item SysinternalsSuite.zip
```

# DTUWC Webinar Examples
Examples shown at Dell Technologies Unified Workspace Community Webinar 15 Mar 2022
For more examples and details please review each tools documentation. 

## ***Dell Command Configure CLI (aka CCTK)***
Getting help and an overview of all BIOS Attributes: 
```
cctk -h
```
Getting help for an specific BIOS Attribute:
```
cctk -h --camera
```
![image](https://user-images.githubusercontent.com/88332918/158579086-03f0f7de-16dd-4272-b33a-1215db216885.png)

Current Value of Camera:
```
cctk --camera
```
![image](https://user-images.githubusercontent.com/88332918/158579693-d06d557a-1908-411f-bd0b-4758adb2ee21.png)

Disable Camera
``` 
cctk -h --camera=disabled
```
![image](https://user-images.githubusercontent.com/88332918/158579836-047f9a83-8f54-4967-b6ca-9ceb5aeffc95.png)

Using the new search feature (for example fastboot):
```
cctk S fastboot
```
![image](https://user-images.githubusercontent.com/88332918/158580113-b0d4f199-5cd0-4601-b1b6-40cb8d6208ff.png)

---

## ***Dell Command Monitor***


> Basic query for DCIM_Battery class. For example get complete PPID of battery: 
```
Get-CimInstance -Namespace root\dcim\sysman -ClassName DCIM_Battery
```


> Query for DCIM_Chassis class. For example getting values for Asset Tag, Dockingstation Service-Tag and Firmwareversion etc. 
```
"Get-CimInstance -Namespace root\dcim\sysman -ClassName DCIM_Chassis"
```

> Query for DCIM_AssetWarrantyInformation Class. For example to get warranty data. 
```
Get-CimInstance -Namespace root\dcim\sysman -ClassName DCIM_AssetWarrantyInformation
```

>Set an Asset Tag with Dell Command Monitor
```
$SetAssetTag = Get-WmiObject -Namespace root\dcim\sysman -ClassName DCIM_Chassis | Where-Object {$_.CreationClassName -ne "DCIM_Dockingstation"}
$SetAssetTag.ChangeAssetTag("Your Asset Tag in Bios")
```
---
## ***Dell PowerShell Provider***
Please review documentation for pre-requisites. Especially about ExecutionPolicy etc. 

To find, download or update Dell PowerShell Provider you may use the following commands: 
```
Find-Module DellBIOSProvider
Install-Module DellBIOSProvider
update-Module -name DellBIOSProvider
```
After install you can start by importing the Dell PowerShell Provider like this: 
```
Import-Module DellBIOSProvider -verbose
```
You will get a folder like structure that you can look at by using: 
```cd dellsmbios:
dir
```
To show information from SystemInformation screen of BIOS you would go the SystemInformation section/folder: 
```cd SystemInformation
dir
```
To Change the Asset Tag of your system to "Dell Technologies Unified Workspace Community" you could use this command: 
```
si Asset "Dell Technologies Unified Workspace Community"
```
To get an overview of all BIOS Settings you could use: 
```
get-dellbiossettings
```
Some more examples... 
```
Get-Item -Path DellSmbios:\SystemInformation\Asset| select -expand CurrentValue
Set-Item -Path DellSmbios:\SystemInformation\Asset "DellNew"
```
## ***Direct WMI Capabilities***
Query some BIOS settings:
```
Get-CimInstance -Namespace root\dcim\sysman\biosattributes -ClassName EnumerationAttribute | Where-Object AttributeName -eq "Fastboot"
Get-CimInstance -Namespace root\dcim\sysman\biosattributes -ClassName EnumerationAttribute | Where-Object {$_.AttributeName -eq "ChasIntrusion"} | select currentvalue
```
Setting an Asset Tag in BIOS: 
```
$AttributeInterface = Get-WmiObject -Namespace root\dcim\sysman\biosattributes -Class BIOSAttributeInterface
$AttributeInterface.SetAttribute(0,0,0,"Asset","DTUWC")
```

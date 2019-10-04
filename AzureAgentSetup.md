## Installation

* Start from an Azure Win10 VM
* Git for windows
* Same as Xenko README.md: Visual Studio, FBX SDK
* Visual C++ 2013 runtime (x64 & x86)

## Install teamcity agent

* Don't install as service
* Set build and temp paths to D: instead of C:
* Also let it build a few stuff, so that it will precache a recent Xenko git on C: (faster warmup time when cloning VM)

## Auto start

### Agent

Link to `C:\BuildAgent\bin\agent.bat start` (don't forget start parameter) to add in `C:\Users\default\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup` (create directory)

### Add to C:\Windows\OEM\SetupComplete2.cmd

#### middle

```
ECHO Enable auto logon BEGIN >> %windir%\Panther\WaSetup.log
regedit /s %~dp0AutoLogin.reg
ECHO Enable auto logon END >> %windir%\Panther\WaSetup.log
```

#### end

```
ECHO Reboot >> %windir%\Panther\WaSetup.log
shutdown -r -t 0 -f
```

### Create C:\Windows\OEM\AutoLogin.reg

```
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Winlogon]
"AutoAdminLogon"="1"
"DefaultUserName"="xenko"
"DefaultPassword"="Replace with VM password!!"
```

### Codesign

* Copy <codesign>.pfx to C:\Windows\OEM (replace <codesign> with actual filename)
* Add register_certs.bat to startup folder of default user (replace <codesign> with actual filename)
```
certutil -f -user -importpfx C:\Windows\OEM\<codesign>.pfx NoRoot
```

### Permissions

Give full control to C:\AzureData to Authenticated Users.

## sysprep

* Run sysprep with OOBE / Generalize / Shutdown
* In Azure portal, capture VM
* Set new image in Teamcity

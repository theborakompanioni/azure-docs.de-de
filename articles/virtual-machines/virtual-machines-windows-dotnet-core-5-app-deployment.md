---
title: Automatisieren der Anwendungsbereitstellung mit VM-Erweiterungen | Microsoft Docs
description: ".NET Core-Tutorial für virtuelle Azure-Computer"
services: virtual-machines-windows
documentationcenter: virtual-machines
author: neilpeterson
manager: timlt
editor: tysonn
tags: azure-resource-manager
ms.assetid: 79c91304-6c1b-4354-a185-fecc129b139d
ms.service: virtual-machines-windows
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 11/21/2016
ms.author: nepeters
translationtype: Human Translation
ms.sourcegitcommit: b4fb534cf18fd17f636e88cc31d6c997a9f09e45
ms.openlocfilehash: e72afd857025773b3aadc3de124b4e79ec6cd512


---
# <a name="application-deployment-with-azure-resource-manager-templates"></a>Anwendungsbereitstellung mit Azure Resource Manager-Vorlagen
Nachdem Sie alle Infrastrukturanforderungen ermittelt und in einer Bereitstellungsvorlage berücksichtigt haben, muss die eigentliche Anwendungsbereitstellung durchgeführt werden. In diesem Kontext meint „Anwendungsbereitstellung“ das Installieren der eigentlichen Anwendungsbinärdateien auf Azure-Ressourcen. Für das Music Store-Beispiel müssen .Net Core und IIS auf jedem virtuellen Computer installiert und konfiguriert werden. Die Music Store-Binärdateien müssen auf jedem virtuellen Computer installiert und die Music Store-Datenbank muss vorab erstellt werden.

In diesem Dokument erfahren Sie, wie Sie mithilfe von VM-Erweiterungen die Anwendungsbereitstellung und -konfiguration auf virtuellen Azure-Computern automatisieren können. Alle Abhängigkeiten und individuellen Konfigurationen werden hervorgehoben. Stellen Sie am besten vorab eine Instanz der Lösung in Ihrem Azure-Abonnement bereit, und orientieren Sie sich an der Azure Resource Manager-Vorlage. Die vollständige Vorlage finden Sie hier – [Music Store Deployment on Windows (Music Store-Bereitstellung unter Windows)](https://github.com/Microsoft/dotnet-core-sample-templates/tree/master/dotnet-core-music-windows).

## <a name="configuration-script"></a>Konfigurationsskript
VM-Erweiterungen sind spezielle Programme, die zur Automatisierung des Konfigurationsprozesses für virtuelle Computer ausgeführt werden. Erweiterungen stehen für verschiedenste Zwecke (etwa Virenschutz, Protokollierungskonfiguration und Docker-Konfiguration) zur Verfügung. Mit einer benutzerdefinierten Skripterweiterung können Sie ein beliebiges Skript für einen virtuellen Computer ausführen. Beim Music Store-Beispiel konfiguriert die benutzerdefinierte Skripterweiterung die virtuellen Windows-Computer und installiert die Music Store-Anwendung.

Machen Sie sich zunächst mit dem auszuführenden Skript vertraut, bevor Sie sich mit der Vorgehensweise zum Deklarieren von VM-Erweiterungen in einer Azure Resource Manager-Vorlage beschäftigen. Dieses Skript konfiguriert den virtuellen Windows-Computer, der als Host für die Music Store-Anwendung fungiert. Wenn das Skript ausgeführt wird, installiert es sämtliche erforderliche Software sowie die Music Store-Anwendung über die Quellcodeverwaltung und bereitet die Datenbank vor. 

> Dieses Beispiel dient zu Demonstrationszwecken.

```PowerShell
<#
    .SYNOPSIS
        Downloads and configures .Net Core Music Store application sample across IIS and Azure SQL DB.
#>

Param (
    [string]$user,
    [string]$password,
    [string]$sqlserver
)

# Firewall
netsh advfirewall firewall add rule name="http" dir=in action=allow protocol=TCP localport=80

# Folders
New-Item -ItemType Directory c:\temp
New-Item -ItemType Directory c:\music

# Install iis
Install-WindowsFeature web-server -IncludeManagementTools

# Install dot.net core sdk
Invoke-WebRequest http://go.microsoft.com/fwlink/?LinkID=615460 -outfile c:\temp\vc_redistx64.exe
Start-Process c:\temp\vc_redistx64.exe -ArgumentList '/quiet' -Wait
Invoke-WebRequest https://go.microsoft.com/fwlink/?LinkID=809122 -outfile c:\temp\DotNetCore.1.0.0-SDK.Preview2-x64.exe
Start-Process c:\temp\DotNetCore.1.0.0-SDK.Preview2-x64.exe -ArgumentList '/quiet' -Wait
Invoke-WebRequest https://go.microsoft.com/fwlink/?LinkId=817246 -outfile c:\temp\DotNetCore.WindowsHosting.exe
Start-Process c:\temp\DotNetCore.WindowsHosting.exe -ArgumentList '/quiet' -Wait

# Download music app
Invoke-WebRequest  https://github.com/Microsoft/dotnet-core-sample-templates/raw/master/dotnet-core-music-windows/music-app/music-store-azure-demo-pub.zip -OutFile c:\temp\musicstore.zip
Expand-Archive C:\temp\musicstore.zip c:\music

# Azure SQL connection sting in environment variable
[Environment]::SetEnvironmentVariable("Data:DefaultConnection:ConnectionString", "Server=$sqlserver;Database=MusicStore;Integrated Security=False;User Id=$user;Password=$password;MultipleActiveResultSets=True;Connect Timeout=30",[EnvironmentVariableTarget]::Machine)

# Pre-create database
$env:Data:DefaultConnection:ConnectionString = "Server=$sqlserver;Database=MusicStore;Integrated Security=False;User Id=$user;Password=$password;MultipleActiveResultSets=True;Connect Timeout=30"
Start-Process 'C:\Program Files\dotnet\dotnet.exe' -ArgumentList 'c:\music\MusicStore.dll'

# Configure iis
Remove-WebSite -Name "Default Web Site"
Set-ItemProperty IIS:\AppPools\DefaultAppPool\ managedRuntimeVersion ""
New-Website -Name "MusicStore" -Port 80 -PhysicalPath C:\music\ -ApplicationPool DefaultAppPool
& iisreset
```

## <a name="vm-script-extension"></a>VM-Skripterweiterung
VM-Erweiterungen können zur Buildzeit für einen virtuellen Computer ausgeführt werden. Hierzu muss die Erweiterungsressource in die Azure Resource Manager-Vorlage eingeschlossen werden. Die Erweiterung kann mithilfe des Visual Studio-Assistenten zum Hinzufügen von Ressourcen oder durch Einfügen von gültigem JSON-Code hinzugefügt werden. Die Skripterweiterungsressource ist innerhalb der Ressource des virtuellen Computers geschachtelt, wie im folgenden Beispiel zu sehen.

Das entsprechende JSON-Beispiel in der Resource Manager-Vorlage finden Sie [hier](https://github.com/Microsoft/dotnet-core-sample-templates/blob/master/dotnet-core-music-windows/azuredeploy.json#L339). 

Beachten Sie, dass das Skript im folgenden JSON-Code in GitHub gespeichert ist. Dieses Skript könnte aber auch in Azure-Blobspeicher gespeichert sein. Bei Azure Resource Manager-Vorlagen kann die Zeichenfolge für die Skriptausführung außerdem so erstellt werden, dass Vorlagenparameterwerte als Parameter für die Skriptausführung verwendet werden. In diesem Fall werden Daten beim Bereitstellen der Vorlagen angegeben, und diese Werte können dann beim Ausführen des Skripts verwendet werden.

```json
{
  "apiVersion": "2015-06-15",
  "type": "extensions",
  "name": "config-app",
  "location": "[resourceGroup().location]",
  "dependsOn": [
    "[concat('Microsoft.Compute/virtualMachines/', variables('vmName'),copyindex())]",
    "[variables('musicstoresqlName')]"
  ],
  "tags": {
    "displayName": "config-app"
  },
  "properties": {
    "publisher": "Microsoft.Compute",
    "type": "CustomScriptExtension",
    "typeHandlerVersion": "1.4",
    "autoUpgradeMinorVersion": true,
    "settings": {
      "fileUris": [
        "https://raw.githubusercontent.com/Microsoft/dotnet-core-sample-templates/master/dotnet-core-music-windows/scripts/configure-music-app.ps1"
      ]
    },
    "protectedSettings": {
      "commandToExecute": "[concat('powershell -ExecutionPolicy Unrestricted -File configure-music-app.ps1 -user ',parameters('adminUsername'),' -password ',parameters('adminPassword'),' -sqlserver ',variables('musicstoresqlName'),'.database.windows.net')]"
    }
  }
}
```

Weitere Informationen zum Verwenden der benutzerdefinierten Skripterweiterung finden Sie unter [Verwenden der benutzerdefinierten Skripterweiterung für virtuelle Linux-Computer mit Azure Resource Manager-Vorlagen](virtual-machines-windows-extensions-customscript.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).

## <a name="next-step"></a>Nächster Schritt
<hr>

[Sehen Sie sich weitere Azure Resource Manager-Vorlagen an.](https://github.com/Azure/azure-quickstart-templates)




<!--HONumber=Feb17_HO2-->



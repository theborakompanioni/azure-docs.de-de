---
title: Herstellen einer Verbindung mit Azure Stack | Microsoft-Dokumentation
description: "Enthält Informationen dazu, wie Sie eine Verbindung mit Azure Stack herstellen."
services: azure-stack
documentationcenter: 
author: SnehaGunda
manager: byronr
editor: 
ms.assetid: 3cebbfa6-819a-41e3-9f1b-14ca0a2aaba3
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/22/2017
ms.author: sngun
ms.translationtype: HT
ms.sourcegitcommit: 1c730c65194e169121e3ad1d1423963ee3ced8da
ms.openlocfilehash: b5a1cd57106895fdd04f0281f0f81c0be0aca428
ms.contentlocale: de-de
ms.lasthandoff: 09/15/2017

---
# <a name="connect-to-azure-stack"></a>Herstellen einer Verbindung mit Azure Stack

Zum Verwalten von Ressourcen müssen Sie eine Verbindung mit dem Azure Stack Development Kit herstellen. Dieses Thema enthält Informationen zu den Schritten, die zum Herstellen einer Verbindung mit dem Development Kit erforderlich sind. Sie können eine der folgenden Verbindungsoptionen verwenden:

* [Remotedesktop](#connect-with-remote-desktop): Ermöglicht einem einzelnen gleichzeitigen Benutzer die schnelle Verbindungsherstellung über das Development Kit.
* [Virtuelles privates Netzwerk (VPN)](#connect-with-vpn): Ermöglicht mehreren gleichzeitigen Benutzern die Verbindungsherstellung von Clients, die außerhalb der Azure Stack-Infrastruktur angeordnet sind (Konfiguration erforderlich).

## <a name="connect-to-azure-stack-with-remote-desktop"></a>Herstellen einer Verbindung mit Azure Stack per Remotedesktop
Bei einer Remotedesktopverbindung kann ein einzelner gleichzeitiger Benutzer das Portal verwenden, um Ressourcen zu verwalten.

1. Öffnen Sie eine Remotedesktopverbindung, und stellen Sie eine Verbindung mit dem Development Kit her. Geben Sie **AzureStack\AzureStackAdmin** als Benutzernamen und das Kennwort des Betreibers ein, das Sie bei der Einrichtung von Azure Stack angegeben haben.  

2. Öffnen Sie auf dem Development Kit-Computer den Server-Manager, klicken Sie auf **Lokaler Server**, deaktivieren Sie die Verstärkte Sicherheitskonfiguration für Internet Explorer, und schließen Sie den Server-Manager.

3. Navigieren Sie zum Öffnen des Benutzer-[Portals](azure-stack-key-features.md#portal) zu (https://portal.local.azurestack.external/), und melden Sie sich mit Ihren Benutzeranmeldeinformationen an. Navigieren Sie zum Öffnen des [Portals](azure-stack-key-features.md#portal) für den Azure Stack-Betreiber zu (https://adminportal.local.azurestack.external/), und melden Sie sich mit den Azure Active Directory-Anmeldeinformationen an, die Sie während der Installation angegeben haben.

## <a name="connect-to-azure-stack-with-vpn"></a>Herstellen einer Verbindung mit Azure Stack per VPN

Sie können eine VPN-Verbindung vom Typ „Geteilter Tunnel“ mit einem Azure Stack Development Kit herstellen. Über die VPN-Verbindung können Sie auf das Portal des Azure Stack-Betreibers, das Benutzerportal und lokal installierte Tools, z.B. Visual Studio und PowerShell zugreifen, um Azure Stack-Ressourcen zu verwalten. Die VPN-Konnektivität wird sowohl für Bereitstellungen auf AAD-Basis (Azure Active Directory) als auch auf AD FS-Basis (Active Directory-Verbunddienste) unterstützt. Mit VPN-Verbindungen können mehrere Clients gleichzeitig eine Verbindung mit Azure Stack herstellen. 

> [!NOTE] 
> Diese VPN-Verbindung stellt keine Konnektivität mit VMs der Azure Stack-Infrastruktur bereit. 

### <a name="prerequisites"></a>Voraussetzungen

* Installieren Sie auf Ihrem lokalen Computer das [Azure Stack-kompatible Azure PowerShell-Modul](azure-stack-powershell-install.md).  
* Laden Sie die [Tools herunter, die zum Arbeiten mit Azure Stack erforderlich sind](azure-stack-powershell-download.md). 

### <a name="configure-vpn-connectivity"></a>Konfigurieren von VPN-Konnektivität

Öffnen Sie zum Erstellen einer VPN-Verbindung mit dem Development Kit auf Ihrem lokalen Windows-basierten Computer eine PowerShell-Sitzung mit erhöhten Rechten, und führen Sie das folgende Skript aus (aktualisieren Sie die Werte für die IP-Adresse und das Kennwort für Ihre Umgebung):

```PowerShell 
# Configure winrm if it's not already configured
winrm quickconfig  

Set-ExecutionPolicy RemoteSigned

# Import the Connect module
Import-Module .\Connect\AzureStack.Connect.psm1 

# Add the development kit computer’s host IP address & certificate authority (CA) to the list of trusted hosts. Make sure to update the the IP address and password values for your environment. 

$hostIP = "<Azure Stack host IP address>"

$Password = ConvertTo-SecureString `
  "<operator's password provided when deploying Azure Stack>" `
  -AsPlainText `
  -Force

Set-Item wsman:\localhost\Client\TrustedHosts `
  -Value $hostIP `
  -Concatenate

# Create a VPN connection entry for the local user
Add-AzsVpnConnection `
  -ServerAddress $hostIP `
  -Password $Password

```

Wenn die Einrichtung erfolgreich ist, sollte in Ihrer Liste mit den VPN-Verbindungen der Eintrag **azurestack** enthalten sein.

![Netzwerkverbindungen](media/azure-stack-connect-azure-stack/image3.png)  

### <a name="connect-to-azure-stack"></a>Herstellen einer Verbindung mit Azure Stack

Stellen Sie eine Verbindung mit der Azure Stack-Instanz her, indem Sie eines der beiden folgenden Verfahren verwenden:  

* Führen Sie den Befehl `Connect-AzsVpn ` aus: 
    
  ```PowerShell
  Connect-AzsVpn `
    -Password $Password
  ```

  Stufen Sie den Azure Stack-Host bei entsprechender Aufforderung als vertrauenswürdig ein, und installieren Sie das Zertifikat über **AzureStackCertificateAuthority** im Zertifikatspeicher Ihres lokalen Computers. (Die Aufforderung wird unter Umständen hinter dem Fenster mit der PowerShell-Sitzung angezeigt.) 

* Öffnen Sie für Ihren lokalen Computer **Netzwerkeinstellungen** > **VPN**, und klicken Sie auf **azurestack** > **connect**. Geben Sie an der Eingabeaufforderung für die Anmeldung den Benutzernamen (AzureStack\AzureStackAdmin) und das Kennwort ein.

### <a name="test-the-vpn-connectivity"></a>Testen der VPN-Konnektivität

Öffnen Sie zum Testen der Portalverbindung einen Internetbrowser, und navigieren Sie entweder zum Benutzerportal (https://portal.local.azurestack.external/) oder zum Betreiberportal (https://adminportal.local.azurestack.external/). Melden Sie sich an, und erstellen Sie Ressourcen.  

## <a name="next-steps"></a>Nächste Schritte

[Verfügbarmachen von virtuellen Computern für Ihre Azure Stack-Benutzer](azure-stack-tutorial-tenant-vm.md)



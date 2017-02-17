---
title: Azure Government-Abonnements | Microsoft Docs
description: Informationen zum Verwalten Ihres Abonnements in Azure Government
services: azure-government
cloud: gov
documentationcenter: 
author: zakramer
manager: liki
ms.assetid: d3375e84-a37d-4e44-9040-70dbe08eabfc
ms.service: azure-government
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: azure-government
ms.date: 01/12/2017
ms.author: zakramer
translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 398abc29328adc179b860ab2cde5e6122f81779d


---
# <a name="managing-and-connecting-to-your-subscription-in-azure-government"></a>Verwalten und Herstellen einer Verbindung mit Ihrem Abonnement in Azure Government
Azure Government verfügt über eindeutige URLs und Endpunkte für die Verwaltung Ihrer Umgebung. Es ist wichtig, dass die richtigen Verbindungen zum Verwalten der Umgebung über das Portal oder PowerShell verwendet werden. Nach dem Herstellen der Verbindung mit der Azure Government-Umgebung können die normalen Vorgänge zum Verwalten eines Diensts durchgeführt werden, sofern die Komponente bereitgestellt wurde.

## <a name="connecting-via-the-portal"></a>Herstellen einer Verbindung über das Portal
Das Portal ist für die meisten Benutzer der einfachste Weg zum Herstellen einer Verbindung mit Azure Government.  Navigieren Sie zum Herstellen einer Verbindung unter [https://portal.azure.us](https://portal.azure.us) zum Portal.  Auf die ältere Version des Azure-Portals kann über [https://manage.windowsazure.us](https://manage.windowsazure.us) zugegriffen werden.

Abonnements für Ihr Konto können über [https://account.windowsazure.us](https://account.windowsazure.us) erstellt werden.

## <a name="connecting-via-powershell"></a>Herstellen einer Verbindung über PowerShell
Ob Sie nun mithilfe von Azure PowerShell ein umfangreiches Abonnement über Skripts verwalten oder auf Features zugreifen, die im Azure-Portal derzeit nicht verfügbar sind, Sie müssen dafür anstelle einer Verbindung mit dem öffentlichen Azure eine Verbindung mit Azure Government herstellen.  Wenn Sie PowerShell bereits im öffentlichen Azure verwendet haben, ist dies größtenteils identisch.  In Azure Government bestehen folgende Unterschiede:

* Herstellen der Verbindung mit Ihrem Konto
* Regionsnamen

> [!NOTE]
> Wenn Sie PowerShell noch nicht verwendet haben, lesen Sie zunächst die [Einführung in Azure PowerShell](/powershell/azureps-cmdlets-docs).
> 
> 

Wenn Sie PowerShell starten, müssen Sie durch Angeben eines Umgebungsparameters festlegen, dass eine Verbindung mit Azure Government hergestellt wird.  Mit dem Parameter wird sichergestellt, dass PowerShell eine Verbindung mit den richtigen Endpunkten herstellt.  Die Sammlung von Endpunkten wird bestimmt, wenn Sie sich bei Ihrem Konto anmelden.  Für unterschiedliche APIs sind verschiedene Versionen des Umgebungsparameters erforderlich:

| Verbindungstyp | Befehl |
| --- | --- |
| Befehle für [Dienstverwaltung](https://msdn.microsoft.com/library/dn708504.aspx) |`Add-AzureAccount -Environment AzureUSGovernment` |
| Befehle für [Ressourcenverwaltung](https://msdn.microsoft.com/library/mt125356.aspx) |`Login-AzureRmAccount -EnvironmentName AzureUSGovernment` |
| [Azure Active Directory](https://msdn.microsoft.com/library/azure/jj151815.aspx)-Befehle |`Connect-MsolService -AzureEnvironment UsGovernment` |
| [Azure Active Directory Version 2](https://msdn.microsoft.com/library/azure/mt757189.aspx)-Befehle |`Connect-AzureAD -AzureEnvironmentName AzureUSGovernment` |
| [Azure CLI-Befehlszeile](../xplat-cli-install.md) |`azure login –environment "AzureUSGovernment"` |

Sie können den Umgebungsparameter auch verwenden, wenn Sie eine Verbindung mit einem Speicherkonto über „New-AzureStorageContext“ herstellen und „AzureUSGovernment“ angeben.

### <a name="determining-region"></a>Festlegen der Region
Nach der Verbindungsherstellung ergibt sich ein weiterer Unterschied. Er betrifft die Regionen, die für die Ausrichtung auf einen Dienst verwendet werden.  Jede Azure-Cloud verfügt über unterschiedliche Regionen.  Diese sind auf der Seite für die Dienstverfügbarkeit aufgeführt.  Normalerweise verwenden Sie die Region im Location-Parameter für einen Befehl.

Hier gibt es allerdings einen Haken.  Die Azure Government-Regionen müssen abweichend von ihrem allgemeinen Namen angegeben werden:

| Allgemeiner Name | Befehl |
| --- | --- |
| US Government, Virginia |US Government, Virginia |
| US Government, Iowa |US Government, Iowa |

> [!NOTE]
> Bei Verwendung des Location-Parameters entfällt das Leerzeichen zwischen „US“ und „Gov“.
> 
> 

Wenn Sie jemals die verfügbaren Regionen in Azure Government überprüfen möchten, können Sie den folgenden Befehl ausführen und die aktuelle Liste drucken:

    Get-AzureLocation

Wenn Sie sich für die verfügbaren Umgebungen in Azure interessieren, können Sie den folgenden Befehl ausführen:

    Get-AzureEnvironment

## <a name="connecting-via-visual-studio"></a>Herstellen der Verbindung über Visual Studio
Visual Studio wird von Entwicklern beim Erstellen von Lösungen zur einfachen Verwaltung ihrer Azure-Abonnements verwendet.  Visual Studio lässt die Konfiguration einer Verbindung mit Azure Government auf der Benutzeroberfläche derzeit nicht zu.  

### <a name="updating-visual-studio-for-azure-government"></a>Aktualisieren von Visual Studio für Azure Government
Sie müssen die Registrierung aktualisieren, damit Visual Studio eine Verbindung mit Azure Government zu herstellen kann.

1. Schließen Sie Visual Studio.
2. Erstellen Sie eine Textdatei mit dem Namen **VisualStudioForAzureGov.reg**.
3. Kopieren Sie den folgenden Text in **VisualStudioForAzureGov.reg**:
   
        Windows Registry Editor Version 5.00
   
        [HKEY_CURRENT_USER\Software\Microsoft\VSCommon\ConnectedUser]
        "AadInstance"="https://login-us.microsoftonline.com/"
        "adaluri"="https://management.core.usgovcloudapi.net"
        "AzureRMEndpoint"="https://management.usgovcloudapi.net"
        "AzureRMAudienceEndpoint"="https://management.core.usgovcloudapi.net"
        "EnableAzureRMIdentity"="true"
        "GraphUrl"="graph.windows.net"
4. Speichern Sie die Datei und führen sie anschließend durch Doppelklick aus.  Sie werden aufgefordert, die Datei in der Registrierung zusammenzuführen.
5. Starten Sie Visual Studio, und beginnen Sie mit der Verwendung von [Cloud-Explorer](../vs-azure-tools-resources-managing-with-cloud-explorer.md).

> [!NOTE]
> Nach dem Festlegen dieses Registrierungsschlüssels sind nur Azure Government-Abonnements zugänglich.  Zuvor konfigurierte Abonnements werden zwar noch angezeigt, funktionieren jedoch nicht, da Visual Studio jetzt mit Azure Government anstatt Azure Public verbunden ist.  Im folgenden Abschnitt finden Sie die Schritte zum Rückgängigmachen der Änderungen.
> 
> 

### <a name="reverting-visual-studio-connection-to-azure-government"></a>Zurücksetzen der Visual Studio-Verbindung mit Azure Government
Damit Visual Studio eine Verbindung mit Azure Public herstellen kann, müssen Sie die Registrierungseinträge entfernen, die eine Verbindung mit Azure Government ermöglichen.

1. Schließen Sie Visual Studio.
2. Erstellen Sie eine Textdatei mit dem Namen **VisualStudioForAzureGov_Remove.reg**.
3. Kopieren Sie den folgenden Text in **VisualStudioForAzureGov_Remove.reg**:
   
        Windows Registry Editor Version 5.00
   
        [HKEY_CURRENT_USER\Software\Microsoft\VSCommon\ConnectedUser]
        "AadInstance"=-
        "adaluri"=-
        "AzureRMEndpoint"=-
        "AzureRMAudienceEndpoint"=-
        "EnableAzureRMIdentity"=-
        "GraphUrl"=-
4. Speichern Sie die Datei und führen sie anschließend durch Doppelklick aus.  Sie werden aufgefordert, die Datei in der Registrierung zusammenzuführen.
5. Starten Sie Visual Studio.

> [!NOTE]
> Nach dem Zurücksetzen dieses Registrierungsschlüssels werden Ihre Azure Government-Abonnements zwar angezeigt, der Zugriff darauf ist jedoch nicht möglich.  Sie können problemlos entfernt werden.
> 
> 

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie unter:

* [PowerShell-Dokumentation auf GitHub](https://github.com/Azure/azure-powershell)
* [Schrittanleitung zum Herstellen einer Verbindung mit dem Ressourcenmanagement](https://blogs.msdn.microsoft.com/azuregov/2015/10/08/configuring-arm-on-azure-gc/)
* [Azure PowerShell-Dokumentation in MSDN](https://msdn.microsoft.com/library/mt619274.aspx)

Weitere Informationen und Updates erhalten Sie, indem Sie den [Microsoft Azure Government-Blog](https://blogs.msdn.microsoft.com/azuregov/) abonnieren.




<!--HONumber=Dec16_HO2-->



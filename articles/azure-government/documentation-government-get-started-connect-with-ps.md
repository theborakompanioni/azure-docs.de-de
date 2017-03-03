---
title: "Azure Government – Herstellen einer Verbindung über PowerShell | Microsoft Docs"
description: "Informationen zum Herstellen einer Verbindung Ihres Abonnements in Azure Government über PowerShell"
services: azure-government
cloud: gov
documentationcenter: 
author: zakramer
manager: liki
ms.assetid: 47e5e535-baa0-457e-8c41-f9fd65478b38
ms.service: azure-government
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: azure-government
ms.date: 02/13/2017
ms.author: zakramer
translationtype: Human Translation
ms.sourcegitcommit: 6b29e98aa3458a76e186234c2e9800294902bea9
ms.openlocfilehash: 3a0594fa639a2ac34c53b2263dbf7dd2410cf16a
ms.lasthandoff: 02/21/2017


---



# <a name="connect-to-azure-government-with-powershell"></a>Herstellen einer Verbindung mit Azure Government über PowerShell
Zur Verwendung der Azure-Befehlszeilenschnittstelle müssen Sie anstelle einer Verbindung mit dem öffentlichen Azure eine Verbindung mit Azure Government herstellen. Die Azure-Befehlszeilenschnittstelle kann zum Verwalten eines umfangreichen Abonnements über Skripts oder für den Zugriff auf Features verwendet werden, die derzeit im Azure-Portal nicht verfügbar sind. Wenn Sie PowerShell bereits im öffentlichen Azure verwendet haben, ist dies größtenteils identisch.  In Azure Government bestehen folgende Unterschiede:

* Herstellen der Verbindung mit Ihrem Konto
* Regionsnamen

> [!NOTE]
> Wenn Sie PowerShell noch nicht verwendet haben, lesen Sie zunächst die [Einführung in Azure PowerShell](/powershell/azureps-cmdlets-docs).
> 
> 

Wenn Sie PowerShell starten, müssen Sie durch Angeben eines Umgebungsparameters festlegen, dass eine Verbindung mit Azure Government hergestellt wird.  Mit dem Parameter wird sichergestellt, dass PowerShell eine Verbindung mit den richtigen Endpunkten herstellt.  Die Sammlung von Endpunkten wird bestimmt, wenn Sie sich bei Ihrem Konto anmelden.  Für unterschiedliche APIs sind verschiedene Versionen des Umgebungsparameters erforderlich:

| Verbindungstyp | Befehl |
| --- | --- |
| Befehle für [Azure Active Directory (klassisches Bereitstellungsmodell)](https://msdn.microsoft.com/library/dn708504.aspx) |`Add-AzureAccount -Environment AzureUSGovernment` |
| Befehle für [Ressourcenverwaltung](https://msdn.microsoft.com/library/mt125356.aspx) |`Login-AzureRmAccount -EnvironmentName AzureUSGovernment` |
| [Azure Active Directory](https://msdn.microsoft.com/library/azure/jj151815.aspx)-Befehle |`Connect-MsolService -AzureEnvironment UsGovernment` |
| Befehle für [Azure Active Directory (Resource Manager-Bereitstellungsmodell)](https://msdn.microsoft.com/library/azure/mt757189.aspx) |`Connect-AzureAD -AzureEnvironmentName AzureUSGovernment` |
| [Azure CLI-Befehlszeile](../xplat-cli-install.md) |`azure login –environment "AzureUSGovernment"` |

Sie können den Umgebungsparameter auch verwenden, wenn Sie eine Verbindung mit einem Speicherkonto über „New-AzureStorageContext“ herstellen und „AzureUSGovernment“ angeben.

### <a name="determining-region"></a>Festlegen der Region
Nach der Verbindungsherstellung ergibt sich ein weiterer Unterschied. Er betrifft die Regionen, die für die Ausrichtung auf einen Dienst verwendet werden.  Jede Azure-Cloud verfügt über unterschiedliche Regionen.  Diese sind auf der Seite für die Dienstverfügbarkeit aufgeführt.  Normalerweise verwenden Sie die Region im Location-Parameter für einen Befehl.

Hier gibt es allerdings einen Haken.  Die Azure Government-Regionen weichen von den entsprechenden allgemeinen Namen ab:

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


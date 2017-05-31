---
title: "Herstellen einer Verbindung mit Azure Deutschland über PowerShell | Microsoft-Dokumentation"
description: "Informationen zum Verwalten Ihres Abonnements in Azure Deutschland über PowerShell"
services: germany
cloud: na
documentationcenter: na
author: gitralf
manager: rainerst
ms.assetid: na
ms.service: germany
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/13/2017
ms.author: ralfwi
ms.translationtype: Human Translation
ms.sourcegitcommit: be3ac7755934bca00190db6e21b6527c91a77ec2
ms.openlocfilehash: 3dafd3b15a4b83d523e04facaa030053b7a65ec0
ms.contentlocale: de-de
ms.lasthandoff: 05/03/2017

---

# <a name="connect-to-azure-germany-by-using-powershell"></a>Herstellen einer Verbindung mit Azure Deutschland über PowerShell
Zur Verwendung von Azure PowerShell mit Azure Deutschland müssen Sie anstelle einer Verbindung mit der globalen Azure-Umgebung eine Verbindung mit Azure Deutschland herstellen. Sie können Azure PowerShell zum Verwalten eines umfangreichen Abonnements über Skripts oder für den Zugriff auf Features verwenden, die derzeit im Azure-Portal nicht verfügbar sind. Wenn Sie PowerShell bereits in der globalen Azure-Umgebung verwendet haben, ist dies größtenteils identisch. In Azure Deutschland bestehen folgende Unterschiede:

* Herstellen der Verbindung mit Ihrem Konto
* Regionsnamen

> [!NOTE]
> Wenn Sie PowerShell noch nicht verwendet haben, lesen Sie zunächst die [Einführung in Azure PowerShell](/powershell/azure/overview).


Wenn Sie PowerShell starten, müssen Sie durch Angeben eines Umgebungsparameters festlegen, dass eine Verbindung mit Azure Deutschland hergestellt wird. Mit dem Parameter wird sichergestellt, dass PowerShell eine Verbindung mit der richtigen Umgebung herstellt. Die Sammlung von Endpunkten wird bestimmt, wenn Sie eine Verbindung mit Ihrem Konto herstellen. Für unterschiedliche APIs sind verschiedene Varianten des Umgebungsparameters erforderlich:

| Verbindungstyp | Befehl |
| --- | --- |
| Befehle für [Azure (klassisches Bereitstellungsmodell)](https://msdn.microsoft.com/library/dn708504.aspx) |`Add-AzureAccount -Environment AzureGermanCloud` |
| Befehle für [Azure (Resource Manager-Bereitstellungsmodell)](https://msdn.microsoft.com/library/mt125356.aspx) |`Login-AzureRmAccount -EnvironmentName AzureGermanCloud` |
| Befehle für [Azure Active Directory (klassisches Bereitstellungsmodell)](https://msdn.microsoft.com/library/azure/jj151815.aspx) |`Connect-MsolService -AzureEnvironment AzureGermanyCloud` |
| Befehle für [Azure Active Directory (Resource Manager-Bereitstellungsmodell)](https://msdn.microsoft.com/library/azure/mt757189.aspx) |`Connect-AzureAD -AzureEnvironmentName AzureGermanyCloud` |

Sie können auch die Option `Environment` verwenden, wenn Sie eine Verbindung mit einem Speicherkonto über `New-AzureStorageContext` herstellen, und `AzureGermanCloud` angeben.

## <a name="determining-region"></a>Festlegen der Region
Nach erfolgreicher Verbindungsherstellung besteht ein weiterer Unterschied in den verfügbaren Zielregionen für einen Dienst. Jeder Cloud Service von Azure verfügt über unterschiedliche Regionen. Diese sind auf der Seite für die Dienstverfügbarkeit aufgeführt. Normalerweise verwenden Sie die Region im Parameter `Location` für einen Befehl.


| Allgemeiner Name | Anzeigename | Standortname |
| --- | --- | --- |
| Deutschland, Mitte |`Germany Central` | `germanycentral` |
| Deutschland, Nordosten |`Germany Northeast` | `germanynortheast` |


> [!NOTE]
> Wie bei PowerShell für die globale Azure-Umgebung können Sie entweder den Anzeigenamen oder den Standortnamen für den `Location`-Parameter verwenden.
>
>

Wenn Sie die verfügbaren Regionen in Azure Deutschland überprüfen möchten, können Sie den folgenden Befehl ausführen und die aktuelle Liste ausgeben. Verwenden Sie für klassische Bereitstellungen den ersten Befehl. Verwenden Sie für Resource Manager-Bereitstellungen den zweiten Befehl.

    Get-AzureLocation
    Get-AzureRmLocation

Wenn Sie sich für die in Azure verfügbaren Umgebungen interessieren, können Sie den folgenden Befehl ausführen:

    Get-AzureEnvironment
    Get-AzureRmEnvironment

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zum Herstellen der Verbindung mit Azure Deutschland finden Sie in den folgenden Ressourcen:

* [Herstellen einer Verbindung mit Azure Deutschland über die Azure CLI](./germany-get-started-connect-with-cli.md)
* [Herstellen einer Verbindung mit Azure Deutschland über Visual Studio](./germany-get-started-connect-with-vs.md)
* [Herstellen einer Verbindung mit Azure Deutschland über das Azure-Portal](./germany-get-started-connect-with-portal.md)






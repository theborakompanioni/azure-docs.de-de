---
title: Wichtige Unterschiede zwischen Azure und Azure Stack beim Verwenden von Diensten und Erstellen von Apps | Microsoft-Dokumentation
description: "Enthält die Informationen, die Sie für das Verwenden von Diensten oder Erstellen von Apps für Azure Stack benötigen."
services: azure-stack
documentationcenter: 
author: twooley
manager: byronr
editor: 
ms.assetid: c81f551d-c13e-47d9-a5c2-eb1ea4806228
ms.service: azure-stack
ms.workload: na
pms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 8/2/2017
ms.author: twooley
ms.translationtype: HT
ms.sourcegitcommit: cf381b43b174a104e5709ff7ce27d248a0dfdbea
ms.openlocfilehash: 0567811e36102177af9f9c339587b251e04a1ff5
ms.contentlocale: de-de
ms.lasthandoff: 09/15/2017

---
# <a name="key-considerations-using-services-or-building-apps-for-azure-stack"></a>Wichtige Aspekte: Verwenden von Diensten oder Erstellen von Apps für Azure Stack

In Bezug auf die Verwendung von Diensten oder die Erstellung von Apps für Azure Stack müssen Sie wissen, dass zwischen Azure Stack und Azure Unterschiede bestehen. Dieser Artikel enthält eine Übersicht über die wichtigsten Aspekte, die für die Nutzung von Azure Stack als Hybrid Cloud-Entwicklungsumgebung gelten.

## <a name="overview"></a>Übersicht

Azure Stack ist eine Hybrid Cloud-Plattform, mit der Sie Azure-Dienste aus dem Rechenzentrum Ihres Unternehmens oder Ihres Service Providers verwenden können. Als Entwickler können Sie Apps erstellen, die unter Azure Stack ausgeführt werden. Anschließend können Sie diese Apps in Azure Stack bzw. Azure bereitstellen, oder Sie können echte Hybrid-Apps erstellen, für die die Konnektivität zwischen einer Azure Stack-Cloud und Azure genutzt wird.

Ihr Azure Stack-Cloudadministrator bzw. Ihr Service Provider teilt Ihnen mit, welche Dienste für Sie verfügbar sind und wie Sie Support erhalten. Diese Dienste sind jeweils über die angepassten Pläne und Angebote erhältlich.

Für den technischen Inhalt von Azure wird davon ausgegangen, dass Apps nicht für Azure Stack, sondern für einen Azure-Dienst entwickelt werden. Für die Erstellung und Bereitstellung von Apps für Azure Stack müssen Sie einige wichtige Unterschiede kennen, z.B.:

* Azure Stack stellt eine Teilmenge dieser Dienste und Features bereit, die in Azure verfügbar sind.
* Ihr Unternehmen oder Service Provider kann auswählen, welche Dienste angeboten werden sollen. Dies gilt auch für angepasste Dienste oder Anwendungen.
* Sie müssen die richtigen Azure Stack-spezifischen Endpunkte verwenden (z.B. die URLs für die Portaladresse und den Azure Resource Manager-Endpunkt).
* Sie müssen PowerShell- und API-Versionen verwenden, die von Azure Stack unterstützt werden. Mit dieser Vorgehensweise wird sichergestellt, dass Ihre Apps sowohl in Azure Stack als auch in Azure funktionieren.

## <a name="cheat-sheet-high-level-differences"></a>Cheat Sheet: Allgemeine Unterschiede

In der folgenden Tabelle sind die allgemeinen Unterschiede zwischen Azure Stack und Azure beschrieben. Beachten Sie dies, wenn Sie für Azure Stack entwickeln oder Azure Stack-Dienste verwenden.

| Bereich | Azure (global) | Azure Stack |
| -------- | ------------- | ----------|
| Wer ist der Betreiber? | Microsoft | Ihr Unternehmen oder Service Provider|
| An wen können Sie sich wenden, um Support zu erhalten? | Microsoft | Support zum Azure Stack Development Kit erhalten Sie in den [Microsoft-Foren](https://social.msdn.microsoft.com/Forums/home?forum=azurestack). Da das Development Kit eine Evaluierungsumgebung ist, wird über den Microsoft-Kundensupport (Customer Support Services, CSS) kein offizieller Support angeboten.
| Verfügbare Dienste | Sehen Sie sich die Liste mit den [Azure-Produkten](https://azure.microsoft.com/services/?b=17.04b) an. Die verfügbaren Dienste variieren je nach Azure-Region. | Azure Stack unterstützt eine Teilmenge der Azure-Dienste. <br><br>Die Dienste variieren in Abhängigkeit davon, was von Ihrem Unternehmen oder Service Provider angeboten wird.
| Azure Resource Manager-Endpunkt* | https://management.azure.com | Für das Development Kit: https://management.local.azurestack.external
| Portal-URL* | [https://portal.azure.com](https://portal.azure.com) | Für das Development Kit: https://portal.local.azurestack.external
| Region | Sie können die Region für die Bereitstellung auswählen. | Für das Development Kit lautet die Region immer **local**. <br><br>Das Development Kit unterstützt nur eine Region.
| Ressourcengruppen | Eine Ressourcengruppe kann mehrere Regionen umfassen. | Für das Development Kit ist nur eine Region vorhanden.
|Unterstützte Namespaces, Ressourcentypen und API-Versionen | Aktuelle Versionen (oder frühere Versionen, die noch nicht als veraltet eingestuft sind). | Azure Stack unterstützt bestimmte Versionen. Informationen hierzu finden Sie im Abschnitt „Versionsanforderungen“ dieses Artikels.
| | |

*Als Azure Stack-Cloudbetreiber finden Sie Informationen zum Administratorportal und zu den Resource Manager-Endpunkt-URLs für Administratoren unter [Using the administrator and user portals in Azure Stack](azure-stack-manage-portals.md) (Verwenden der Administrator- und Benutzerportale in Azure Stack).

## <a name="helpful-tools-and-best-practices"></a>Hilfreiche Tools und bewährte Methoden
 
 Microsoft stellt mehrere Tools und Anleitungen für die Azure Stack-Entwicklung bereit.

| Empfehlung | Referenzen | 
| -------- | ------------- | 
| Installieren Sie die richtigen Tools auf der Entwicklerarbeitsstation. | - [Installieren von PowerShell](azure-stack-powershell-install.md)<br>- [Herunterladen von Tools](azure-stack-powershell-download.md)<br>- [Konfigurieren von PowerShell](azure-stack-powershell-configure-user.md)<br>- [Installieren von Visual Studio](azure-stack-install-visual-studio.md) 
| Lesen Sie sich die Informationen zu folgenden Punkten durch:<br>- Aspekte zu Azure Resource Manager-Vorlagen<br>- Suchen nach Schnellstartvorlagen<br>- Verwenden eines Richtlinienmoduls als Hilfe zur Verwendung von Azure für die Azure Stack-Entwicklung | [Entwickeln für Azure Stack](azure-stack-developer.md) | 
| Lesen Sie sich die bewährten Methoden für Vorlagen durch, und befolgen Sie diese Methoden. | [Resource Manager-Schnellstartvorlagen](https://github.com/Azure/azure-quickstart-templates/blob/master/1-CONTRIBUTION-GUIDE/best-practices.md#best-practices)
| | |

## <a name="version-requirements"></a>Versionsanforderungen

Azure Stack unterstützt bestimmte Versionen von Azure PowerShell und Azure-Dienst-APIs. Sie müssen die unterstützten Versionen verwenden, um sicherzustellen, dass Ihre App sowohl für Azure Stack als auch für Azure bereitgestellt werden kann.

Verwenden Sie [API-Versionsprofile](azure-stack-version-profiles.md), damit sichergestellt ist, dass Sie eine richtige Version von Azure PowerShell nutzen. Zum Ermitteln des aktuellen API-Versionsprofils, das Sie nutzen können, müssen Sie wissen, welchen Build von Azure Stack Sie verwenden. Sie erhalten diese Informationen von Ihrem Azure Stack-Administrator.

>[!NOTE]
 Wenn Sie das Azure Stack Development Kit nutzen und über Administratorzugriff verfügen, können Sie den Azure Stack-Build ermitteln, indem Sie unter [Verwalten von Updates in Azure Stack](https://docs.microsoft.com/azure/azure-stack/azure-stack-updates#determine-the-current-version) die Schritte im Abschnitt „Bestimmen der aktuellen Version“ ausführen.

Führen Sie für die anderen APIs den folgenden PowerShell-Befehl aus, um die Namespaces, Ressourcentypen und API-Versionen auszugeben, die in Ihrem Azure Stack-Abonnement unterstützt werden. Beachten Sie, dass auf Eigenschaftsebene trotzdem noch Unterschiede bestehen können. (Damit dieser Befehl funktioniert, müssen Sie PowerShell für eine Azure Stack-Umgebung bereits [installiert](azure-stack-powershell-install.md) und [konfiguriert](azure-stack-powershell-configure-user.md) haben. Sie müssen auch über ein Abonnement für ein Azure Stack-Angebot verfügen.)

 ```powershell
Get-AzureRmResourceProvider | Select ProviderNamespace -Expand ResourceTypes | Select * -Expand ApiVersions | `
Select ProviderNamespace, ResourceTypeName, @{Name="ApiVersion"; Expression={$_}} 
```

Beispielausgabe (gekürzt): ![Beispielausgabe für den Befehl Get-AzureRmResourceProvider](media/azure-stack-considerations/image1.png)
 
## <a name="next-steps"></a>Nächste Schritte

Ausführlichere Informationen zu den Unterschieden auf Dienstebene finden Sie unter:

* [Aspekte von virtuellen Computern in Azure Stack](azure-stack-vm-considerations.md)
* [Azure Stack-Speicher: Unterschiede und Überlegungen](azure-stack-acs-differences.md)
* [Überlegungen zu Azure Stack-Netzwerken](azure-stack-network-differences.md)



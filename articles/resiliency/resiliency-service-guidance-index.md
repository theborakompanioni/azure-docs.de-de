---
title: Leitfaden zur Resilienz von Diensten | Microsoft Azure
description: "Links zu Leitfäden zur Notfallwiederherstellung sowie proaktive Anleitungen zum Thema Resilienz und Verfügbarkeit für Microsoft Azure-Dienste."
services: 
documentationcenter: na
author: adamglick
manager: saladki
editor: 
ms.assetid: 919a197d-ba47-4e49-a64c-118e27d5a7df
ms.service: resiliency
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/18/2016
ms.author: aglick
translationtype: Human Translation
ms.sourcegitcommit: aafb03c86884d1af3e2cae1134c9e6d7ac42397b
ms.openlocfilehash: 4186211af944c0c0109799a94e4163cfceeee4c4


---
# <a name="microsoft-azure-service-resiliency-guidance"></a>Leitfaden zur Resilienz von Diensten in Microsoft Azure
Microsoft Azure bietet Ihnen die Ressourcen, die Sie benötigen, und zwar genau dann, wenn Sie sie benötigen. Im Rahmen der bewährten Entwurfs- und Betriebsverfahren müssen Sie wissen, wie Sie die Nutzung von Azure-Diensten gestalten, um eine hohe Verfügbarkeit zu erreichen. Sie müssen auch wissen, was zu tun ist, wenn Ihre Anwendung von einer Dienstunterbrechung betroffen ist. Um Sie in diesem Prozess zu unterstützen, bietet dieses Dokument Links zu Leitfäden zur Notfallwiederherstellung sowie Entwurfsleitfäden für verschiedene Azure-Dienste.

## <a name="disaster-recovery-guidance"></a>Leitfäden zur Notfallwiederherstellung
Die Links zu Leitfäden zur Notfallwiederherstellung bieten Informationen, die Sie benötigen, um Ihre Anwendung schnell wieder online zu schalten, wenn diese von der Unterbrechung eines Azure-Diensts betroffen war. Diese Links helfen bei der Beantwortung der Frage „Meine Anwendungen sind von einer Azure-Dienstunterbrechung betroffen. Was kann ich tun?“

## <a name="design-guidance"></a>Entwurfsleitfäden
Die Entwurfsleitfäden bieten Anleitungen zu Design und Architektur und stellen wichtige Informationen dazu bereit, wie Sie die einzelnen Azure-Dienste optimal nutzen, um die Verfügbarkeit Ihrer Anwendung zu maximieren. Diese Links helfen bei der Beantwortung der Frage „Wie stelle ich sicher, dass ein Bug, ein Hardwarefehler, eine Dienstunterbrechung oder ein sonstiger Fehler sich nicht auf die allgemeine Verfügbarkeit meiner Anwendung auswirkt?“ Wenn es keinen spezifischen Leitfaden für den Dienst gibt, den Sie derzeit suchen, finden Sie im Artikel [Hohe Verfügbarkeit für in Microsoft Azure erstellte Anwendungen](resiliency-high-availability-azure-applications.md) möglicherweise zusätzliche Informationen, die Sie bei Ihrem Entwurf unterstützen.

## <a name="service-guidance"></a>Dienstleitfäden
| Dienst | Leitfäden zur Notfallwiederherstellung | Entwurfsleitfäden |
|:--- |:---:|:---:|
| [Cloud Services](https://azure.microsoft.com/services/cloud-services/ "Azure Cloud Services") |[link](../cloud-services/cloud-services-disaster-recovery-guidance.md "Azure Cloud Services – Leitfaden zur Notfallwiederherstellung") |Nicht verfügbar. |
| [Schlüsseltresor](https://azure.microsoft.com/services/key-vault/ "Azure Schlüsseltresor") |[link](../key-vault/key-vault-disaster-recovery-guidance.md "Azure Key Vault – Leitfaden zur Notfallwiederherstellung") |Nicht verfügbar. |
| [Speicher](https://azure.microsoft.com/services/storage/ "Azure Speicher") |[link](../storage/storage-disaster-recovery-guidance.md "Azure Storage – Leitfaden zur Notfallwiederherstellung") |Nicht verfügbar. |
| [SQL-Datenbanken](https://azure.microsoft.com/services/sql-database/ "Azure SQL-Datenbanken") |[link](../sql-database/sql-database-disaster-recovery.md "Azure SQL-Datenbanken – Leitfaden zur Notfallwiederherstellung") |[link](../sql-database/sql-database-performance-guidance.md "Azure SQL-Datenbanken – Entwurfsleitfaden") |
| [Virtuelle Computer](https://azure.microsoft.com/services/virtual-machines/ "Azure Virtuelle Computer") |[link](../virtual-machines/virtual-machines-disaster-recovery-guidance.md "Azure Virtual Machines – Leitfaden zur Notfallwiederherstellung") |Nicht verfügbar. |
| [Virtuelles Netzwerk](https://azure.microsoft.com/services/virtual-network/ "Azure Virtuelles Netzwerk") |[link](../virtual-network/virtual-network-disaster-recovery-guidance.md "Azure Virtual Network – Leitfaden zur Notfallwiederherstellung") |Nicht verfügbar. |

## <a name="next-steps"></a>Nächste Schritte
Wenn Sie allgemeinere Informationen zu Systemen und Lösungen suchen, lesen Sie den Artikel [Notfallwiederherstellung und hohe Verfügbarkeit für in Microsoft Azure erstellte Anwendungen](https://aka.ms/drtechguide).



<!--HONumber=Nov16_HO3-->



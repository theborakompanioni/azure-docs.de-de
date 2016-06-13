<properties
   pageTitle="Leitfaden zur Resilienz von Diensten | Microsoft Azure"
   description="Links zu Leitfäden zur Notfallwiederherstellung sowie proaktive Anleitungen zum Thema Resilienz und Verfügbarkeit für Microsoft Azure-Dienste."
   services=""
   documentationCenter="na"
   authors="adamglick"
   manager="hongfeig"
   editor=""/>

<tags
   ms.service="resiliency"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="05/10/2016"
   ms.author="aglick"/>

# Leitfaden zur Resilienz von Diensten in Microsoft Azure
Microsoft Azure bietet Ihnen die Ressourcen, die Sie benötigen, und zwar genau dann, wenn Sie sie benötigen. Im Rahmen der bewährten Entwurfs- und Betriebsverfahren müssen Sie wissen, wie Sie die Nutzung von Azure-Diensten gestalten, um eine hohe Verfügbarkeit zu erreichen. Sie müssen auch wissen, was zu tun ist, wenn Ihre Anwendung von einer Dienstunterbrechung betroffen ist. Um Sie in diesem Prozess zu unterstützen, bietet dieses Dokument Links zu Leitfäden zur Notfallwiederherstellung sowie Entwurfsleitfäden für verschiedene Azure-Dienste.

##Leitfäden zur Notfallwiederherstellung
Die Links zu Leitfäden zur Notfallwiederherstellung bieten Informationen, die Sie benötigen, um Ihre Anwendung schnell wieder online zu schalten, wenn diese von der Unterbrechung eines Azure-Diensts betroffen war. Diese Links helfen bei der Beantwortung der Frage „Meine Anwendungen sind von einer Azure-Dienstunterbrechung betroffen. Was kann ich tun?“

##Entwurfsleitfäden
Die Entwurfsleitfäden bieten Anleitungen zu Design und Architektur und stellen wichtige Informationen dazu bereit, wie Sie die einzelnen Azure-Dienste optimal nutzen, um die Verfügbarkeit Ihrer Anwendung zu maximieren. Diese Links helfen bei der Beantwortung der Frage „Wie stelle ich sicher, dass ein Bug, ein Hardwarefehler, eine Dienstunterbrechung oder ein sonstiger Fehler sich nicht auf die allgemeine Verfügbarkeit meiner Anwendung auswirkt?“ Wenn es keinen spezifischen Leitfaden für den Dienst gibt, den Sie derzeit suchen, finden Sie im Artikel [High availability for applications built on Microsoft Azure](./resiliency-high-availability-azure-applications.md) (Hohe Verfügbarkeit für Anwendungen, die in Microsoft Azure erstellt wurden) möglicherweise zusätzliche Informationen, die Sie bei Ihrem Entwurf unterstützen.

##Dienstleitfäden
| Dienst | Leitfäden zur Notfallwiederherstellung | Entwurfsleitfäden |
|:---------|:--------------------------:|:------------------:|
| [Cloud Services](https://azure.microsoft.com/services/cloud-services/ "Azure Cloud Services") | [link](../cloud-services/cloud-services-disaster-recovery-guidance.md "Azure Cloud Services – Leitfaden zur Notfallwiederherstellung") | Nicht verfügbar. |
| [Schlüsseltresor](https://azure.microsoft.com/services/key-vault/ "Azure-Schlüsseltresor") | [link](../key-vault/key-vault-disaster-recovery-guidance.md "Azure Key Vault – Leitfaden zur Notfallwiederherstellung") | Nicht verfügbar. |
| [Speicher](https://azure.microsoft.com/services/storage/ "Azure Storage") | [link](../storage/storage-disaster-recovery-guidance.md "Azure Storage – Leitfaden zur Notfallwiederherstellung") | Nicht verfügbar. |
| [SQL-Datenbanken](https://azure.microsoft.com/services/sql-database/ "Azure SQL-Datenbanken") | [link](../sql-database/sql-database-disaster-recovery.md "Azure SQL-Datenbanken – Leitfaden zur Notfallwiederherstellung") | [link](../sql-database/sql-database-business-continuity-design.md "Azure SQL-Datenbanken – Entwurfsleitfaden") |
| [Virtuelle Computer](https://azure.microsoft.com/services/virtual-machines/ "Azure Virtual Machines") | [link](../virtual-machines/virtual-machines-disaster-recovery-guidance.md "Azure Virtual Machines – Leitfaden zur Notfallwiederherstellung") | Nicht verfügbar. |
| [Virtuelles Netzwerk](https://azure.microsoft.com/services/virtual-network/ "Azure Virtual Network") | [link](../virtual-network/virtual-network-disaster-recovery-guidance.md "Azure Virtual Network – Leitfaden zur Notfallwiederherstellung") | Nicht verfügbar. |

##Nächste Schritte
Wenn Sie allgemeinere Informationen zu Systemen und Lösungen suchen, lesen Sie den Artikel [Notfallwiederherstellung und hohe Verfügbarkeit für in Microsoft Azure erstellte Anwendungen](https://aka.ms/drtechguide).

<!---HONumber=AcomDC_0601_2016-->
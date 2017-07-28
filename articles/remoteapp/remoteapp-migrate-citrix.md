---

title: Migrieren von Azure RemoteApp zu Citrix XenApp Essentials | Microsoft-Dokumentation
description: Migrieren von Azure RemoteApp zu Citrix XenApp Essentials
services: remoteapp
documentationcenter: 
author: msmbaldwin
manager: mbaldwin
ms.assetid: 695a8165-3454-4855-8e21-f2eb2c61201b
ms.service: remoteapp
ms.workload: compute
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/26/2017
ms.author: mbaldwin
ms.translationtype: Human Translation
ms.sourcegitcommit: 07584294e4ae592a026c0d5890686eaf0b99431f
ms.openlocfilehash: fcd96a466d1c0dad17d7012308281ef868463b19
ms.contentlocale: de-de
ms.lasthandoff: 06/01/2017


---

# <a name="migrate-from-azure-remoteapp-to-citrix-xenapp-essentials"></a>Migrieren von Azure RemoteApp zu Citrix XenApp Essentials

Wenn Sie Azure RemoteApp verwenden und zu Citrix XenApp Essentials migrieren möchten, gibt es einige Voraussetzungen zu berücksichtigen. Lesen Sie zunächst den [Step by Step Technical Deployment Guide for Citrix XenApp Essentials](https://docs.citrix.com/content/dam/docs/en-us/citrix-cloud/downloads/xenapp-essentials-deployment-guide.pdf) (Schritt-für-Schritt-Anleitung zur technischen Bereitstellung für Citrix XenApp Essentials), und konsultieren Sie die entsprechende [technische Onlinebibliothek](http://docs.citrix.com/en-us/citrix-cloud/xenapp-and-xendesktop-service/xenapp-essentials.html). 

## <a name="prerequisite-steps-for-migration"></a>Für die Migration erforderliche Schritte

1. Erstellen Sie ein neues virtuelles Netzwerk, oder legen Sie in Azure Resource Manager das virtuelle Azure-Netzwerk fest, in dem Citrix XenApp Essentials bereitgestellt werden soll. Für Azure RemoteApp wird das klassische Azure-Portal verwendet. Citrix XenApp Essentials unterstützt dagegen nur Azure Resource Manager.  
2. Stellen Sie sicher, dass für das von Ihnen gewählte virtuelle Netzwerk Netzwerkzugriff auf Ihren Domänencontroller besteht, da Citrix nur Hybridbereitstellungen unterstützt. Stellen Sie bei Verwendung einer Cloudbereitstellung von Azure RemoteApp sicher, dass über Ihr virtuelles Netzwerk Zugriff auf einen Active Directory-Domänencontroller besteht. Sie können auch Azure Active Directory Domain Services (Azure AD DS) verwenden. 
3. Stellen Sie sicher, dass das DNS für das virtuelle Netzwerk ordnungsgemäß konfiguriert wurde, sodass der Domänenbeitritt beim ersten Versuch erfolgreich ist. Sie können im gewählten virtuellen Netzwerk einen virtuellen Computer (VM) erstellen und einen manuellen Domänenbeitritt durchführen, um zu überprüfen, ob das DNS und der Domänenbeitritt wie erwartet funktionieren. So können Sie dafür sorgen, dass der erste Versuch der Bereitstellung von Citrix XenApp Essentials erfolgreich ist. 
4. Erstellen Sie bei Bedarf ein Peering virtueller Netzwerke zwischen einem klassischen Azure-Portal im virtuellen Netzwerk, das Sie für Azure RemoteApp verwenden, und dem virtuellen Netzwerk für Azure Resource Manager. Dieser Peering-Prozess funktioniert so, als ob zwei Netzwerke in einer Region vorhanden wären. Ist dies nicht der Fall, verwenden Sie ein VPN zwischen Standorten, um die virtuellen Netzwerke miteinander zu verbinden. 
5. Lesen Sie sich den Artikel [Migrieren von Daten in und aus Azure RemoteApp](remoteapp-migrate.md) durch, falls erforderlich. 
6. Aktualisieren Sie Ihr vorhandenes Azure RemoteApp-Image, um die Citrix VDA-Komponente einzubinden. (Eine Anleitung hierzu finden Sie in der Citrix-Dokumentation.) 
7. Navigieren Sie zum Azure Marketplace, und beginnen Sie mit der Bereitstellung von Citrix XenApp Essentials.

## <a name="other-considerations"></a>Weitere Überlegungen

Berücksichtigen Sie bei der Migration die folgenden weiteren Überlegungen:
- Citrix XenApp Essentials unterstützt nur Hybridbereitstellungen. Für die Anwendung ist also Netzwerkzugriff auf einen Domänencontroller erforderlich, damit der Beitritt zur Domäne durchgeführt werden kann. Wenn Sie eine Cloudbereitstellung von Azure RemoteApp verwenden, müssen Sie entweder Azure AD DS verwenden oder sicherstellen, dass Ihr virtuelles Netzwerk für den Domänenbeitritt über Zugriff auf Active Directory verfügt. 
- Informationen zum Verschieben von Benutzerdaten zu Citrix XenApp Essentials finden Sie unter [Migrieren von Daten in und aus Azure RemoteApp](remoteapp-migrate.md). 
- Citrix XenApp Essentials unterstützt nur Active Directory-Konten. Microsoft-Konten (wie outlook.com, msn.com oder hotmail.com) werden nicht unterstützt. 

## <a name="citrix-xenapp-essentials-billing"></a>Abrechnung von Citrix XenApp Essentials

Ausführliche Informationen zu den Preisen finden Sie unter [FAQ](https://www.citrix.com/global-partners/microsoft/resources/xenapp-essentials-faq.html#tab-30699) (Häufig gestellte Fragen) und im Artikel mit der [Übersicht über Citrix](https://www.citrix.com/global-partners/microsoft/remote-app.html). Für Citrix XenApp Essentials gibt es drei Abrechnungskomponenten:

- Die Citrix-Servicegebühr in Höhe von 12 USD pro Benutzer pro Monat. Wie bei allen Azure Marketplace-Einkäufen wird dies anhand der Zahlungsmethode abgerechnet, die Ihrem Azure-Abonnement zugeordnet ist. Für EA-Kunden (Enterprise Agreement) können monetäre Azure-Gutschriften nicht verwendet werden. 
- RDS-CALs (Remote Data Services-Clientzugriffslizenzen). Derzeit können Sie die Remote Access Fee erwerben, die im Paket mit der Citrix XenApp Essentials-Zahlung zum Preis von 6,25 USD enthalten ist. Wenn Sie EA-Kunde sind, können Sie zur Bezahlung hierfür monetäre Azure-Gutschriften nutzen. Falls Sie Ihre vorhandenen RDS-CALs verwenden möchten, wenden Sie sich unter [arainfo@microsoft.com](mailto:arainfo@microsoft.com) an uns, damit wir dies entsprechend auf Ihre Rechnung anwenden können. 
- Azure-Compute- und -Speicherkosten: Dies sind die Azure-Speicherkosten und der Computeverbrauch für die genutzten VMs. Achten Sie jeweils auf die Preise, wenn Sie Ihre VM-Größe und -Benutzerdichte auswählen. Wenn Sie EA-Kunde sind, können Sie zur Bezahlung hierfür monetäre Azure-Gutschriften nutzen.

Wenn Sie weitere Fragen haben, haben Sie folgende Möglichkeiten:
- Senden Sie eine E-Mail an [arainfo@microsoft.com](mailto:arainfo@microsoft.com).
- Wenden Sie sich an den [Azure-Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). [Öffnen Sie zuerst eine Azure-Supportanfrage](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), um Hilfe zu den erforderlichen Schritten 1 bis 5 zu erhalten. Wenden Sie sich an Citrix, indem Sie im Citrix-Verwaltungsportal für die Schritte 6 bis 7 ein Supportticket öffnen. 


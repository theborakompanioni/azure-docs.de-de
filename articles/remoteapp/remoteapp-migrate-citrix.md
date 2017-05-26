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
ms.sourcegitcommit: 8f987d079b8658d591994ce678f4a09239270181
ms.openlocfilehash: de47edc5ea4dc1b67bd69b74929acf249b0d43e6
ms.contentlocale: de-de
ms.lasthandoff: 05/25/2017


---

# <a name="how-to-migrate-from-azure-remoteapp-to-citrix-xenapp-essentials"></a>Migrieren von Azure RemoteApp zu Citrix XenApp Essentials

Für Azure RemoteApp-Kunden, die eine Migration zu Citrix XenApp Essentials durchführen möchten, müssen einige Voraussetzungen erfüllt sein.  Wir empfehlen Ihnen, zuerst den [Step by Step Technical Deployment Guide for Citrix XenApp Essentials](https://docs.citrix.com/content/dam/docs/en-us/citrix-cloud/downloads/xenapp-essentials-deployment-guide.pdf) (Schritt-für-Schritt-Anleitung zur technischen Bereitstellung für Citrix XenApp Essentials) zu lesen und die [technische Onlinebibliothek](http://docs.citrix.com/en-us/citrix-cloud/xenapp-and-xendesktop-service/xenapp-essentials.html) zu konsultieren. 

Glücklicherweise können Sie den größten Teil der bereits getätigten Investitionen in Azure RemoteApp wiederverwenden, aber für die Bereitstellung von XenApp Essentials müssen einige allgemeine Voraussetzungen erfüllt sein.

## <a name="prerequisites"></a>Voraussetzungen

1. Erstellen Sie ein neues VNET, oder legen Sie in Azure Resource Manager das Azure VNET fest, in dem Citrix XenApp Essentials bereitgestellt werden soll. Für Azure RemoteApp wird klassisches Azure verwendet. Citrix XenApp Essentials unterstützt dagegen nur Azure Resource Manager.  
2. Stellen Sie sicher, dass für das von Ihnen gewählte VNET Netzwerkzugriff auf Ihren Domänencontroller besteht, da Citrix nur Hybridbereitstellungen unterstützt. Wenn Sie eine Cloudbereitstellung von Azure RemoteApp verwenden, müssen Sie sicherstellen, dass Ihr VNET Netzwerkzugriff auf einen Active Directory-Domänencontroller hat. Außerdem empfehlen wir Ihnen, Azure Active Directory Domain Services (AAD DS) zu verwenden. 
3. Vergewissern Sie sich, dass das DNS für das VNET richtig konfiguriert ist, damit der Beitritt zur Domäne beim ersten Versuch erfolgreich ist. Sie können im gewählten VNET einen virtuellen Computer erstellen und einen manuellen Domänenbeitritt durchführen, um zu überprüfen, ob das DNS und der Domänenbeitritt wie erwartet funktionieren. So können Sie dafür sorgen, dass der erste Versuch der Bereitstellung von Citrix XenApp Essentials erfolgreich ist. 
4. Erstellen Sie bei Bedarf ein VNET-Peering zwischen einem klassischen Azure VNET, das Sie mit Azure RemoteApp und Ihrem Azure Resource Manager VNET verwenden, sofern sich diese in derselben Region befinden. Verwenden Sie andernfalls ein S2S-VPN, um für VNETs eine Netzwerkverbindung herzustellen. 
5. Lesen Sie sich den Artikel [Migrieren von Daten in und aus Azure RemoteApp](remoteapp-migrate.md) durch, falls erforderlich. 
6. Aktualisieren Sie Ihr vorhandenes Azure RemoteApp-Image, um die Citrix VDA-Komponente einzubinden. Eine Anleitung hierzu finden Sie in der Citrix-Dokumentation. 
7. Navigieren Sie zum Azure Marketplace, und beginnen Sie mit der Bereitstellung von Citrix XenApp Essentials.

Gutes Gelingen und vielen Dank, dass Sie Azure RemoteApp verwenden. 

## <a name="other-considerations"></a>Weitere Überlegungen:

- Citrix XenApp Essentials unterstützt nur Hybridbereitstellungen. Für die Anwendung ist also Netzwerkzugriff auf einen Domänencontroller erforderlich, damit der Beitritt zur Domäne durchgeführt werden kann. Wenn Sie eine Cloudbereitstellung von Azure RemoteApp verwenden, müssen Sie entweder AAD DS verwenden oder sicherstellen, dass Ihr VNET für den Domänenbeitritt über Zugriff auf Active Directory verfügt. 
- Informationen dazu, wie Sie Benutzerdaten zu CXE verschieben, finden Sie unter [Migrieren von Daten in und aus Azure RemoteApp](remoteapp-migrate.md). 
- Citrix XenApp Essentials unterstützt nur Active Directory-Konten. Die Anwendung unterstützt keine Microsoft-Konten (@outlook.com, @msn.com, @hotmail.com usw.). 

## <a name="understanding-billing-for-citrix-xenapp-essentials"></a>Grundlagen der Abrechnung für Citrix XenApp Essentials 

Ausführliche Informationen zu den Preisen finden Sie unter [FAQ](https://www.citrix.com/global-partners/microsoft/resources/xenapp-essentials-faq.html#tab-30699) (Häufig gestellte Fragen) und im Artikel mit der [Übersicht über Citrix](https://www.citrix.com/global-partners/microsoft/remote-app.html). Für Citrix XenApp Essentials gibt es drei Abrechnungskomponenten:

1. Citrix-Servicegebühr in Höhe von 12 USD pro Benutzer pro Monat: Wie bei allen Azure Marketplace-Einkäufen wird dies anhand der Zahlungsmethode abgerechnet, die Ihrem Azure-Abonnement zugeordnet ist. Für EA-Kunden können monetäre Azure-Gutschriften nicht verwendet werden. 
2. RDS-CAL: Sie müssen entweder Ihre eigene RDS-Clientzugriffslizenz (CAL) (in Kürze verfügbar) verwenden oder das Angebot mit der „Remote Access Fee“ (RAF) erwerben, die im Paket mit der Citrix XenApp Essentials-Zahlung zum Preis von 6,25 USD enthalten ist. EA-Kunden können monetäre Azure-Gutschriften hierfür zur Bezahlung nutzen. Falls Sie Ihre vorhandenen RDS-CALs verwenden möchten, können Sie sich unter [arainfo@microsoft.com](mailto:arainfo@microsoft.com an uns wenden, damit wir dies entsprechend auf Ihre Rechnung anwenden können. 
3. Azure-Compute- und -Speicherkosten: Dies sind die Azure-Speicherkosten und der Computeverbrauch für die genutzten VMs. Achten Sie jeweils auf die Preise, wenn Sie Ihre VM-Größe und -Benutzerdichte auswählen. EA-Kunden können monetäre Azure-Gutschriften hierfür zur Bezahlung nutzen.

Wenden Sie sich an uns, falls Sie weitere Fragen haben.
1. Senden Sie eine E-Mail an [arainfo@microsoft.com](mailto:arainfo@microsoft.com).
2. Wenden Sie sich an den [Azure-Support](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade). [Öffnen Sie zuerst eine Azure-Supportanfrage](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade), um Hilfe zu den obigen Schritten 1 bis 5 zu erhalten. Nehmen Sie Kontakt mit Citrix auf, indem Sie im Citrix-Verwaltungsportal für die Schritte 6 bis 7 ein Supportticket öffnen. 


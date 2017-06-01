---
title: "Aufgabenliste für Verwaltung und Entwicklung in BizTalk Services | Microsoft Docs"
description: "Unterstützung bei Planung und Aufträgen für die Entwicklung in Azure BizTalk Services."
services: biztalk-services
documentationcenter: 
author: msftman
manager: erikre
editor: 
ms.assetid: 0ab70b5b-1a88-4ba5-b329-ec51b785010e
ms.service: biztalk-services
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/15/2016
ms.author: deonhe
ms.translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 1013419f9283c3e81a6a6e8ed56a34d6ee5937ea
ms.contentlocale: de-de
ms.lasthandoff: 11/17/2016


---
# <a name="administration-and-development-task-list-in-biztalk-services"></a>Aufgabenliste für Verwaltung und Entwicklung in BizTalk Services

> [!INCLUDE [BizTalk Services is being retired, and replaced with Azure Logic Apps](../../includes/biztalk-services-retirement.md)]

## <a name="getting-started"></a>Erste Schritte
Bei der Arbeit mit Microsoft Azure BizTalk Services sind verschiedene lokale und cloudbasierte Komponenten zu berücksichtigen. Betrachten Sie zunächst den folgenden Prozessablauf:  

| Schritt | Wer ist verantwortlich? | Task | Verwandte Links |
| --- | --- | --- | --- |
| 1. |Administrator |Erstellen Sie das Microsoft Azure-Abonnement über ein Microsoft-Konto oder ein Organisationskonto. |[Klassisches Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=213885) |
| 2. |Administrator |Erstellen Sie einen BizTalk Service, oder stellen Sie ihn bereit. |[Erstellen eines BizTalk Service im klassischen Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=302280) |
| 3. |Administrator |Registrieren Sie Ihre BizTalk Services-Bereitstellung oder die Bereitstellung Ihres Unternehmens. |[Registrieren und Aktualisieren einer BizTalk Service-Bereitstellung im BizTalk Services-Portal](https://msdn.microsoft.com/library/azure/hh689837.aspx) |
| 4. |Administrator |Gilt, wenn die Anwendung mithilfe von BizTalk Adapter Service eine Verbindung mit einem lokalen Branchensystem herstellt oder ein Warteschlangen- oder Themenziel verwendet.  Erstellen Sie den Azure Service Bus-Namespace. Teilen Sie dem Entwickler die Werte für Namespace, Service Bus-Ausstellername und Service Bus-Ausstellerschlüssel mit. |[Vorgehensweise: Erstellen oder Ändern eines Service Bus-Dienstnamespaces](../service-bus-messaging/service-bus-dotnet-get-started-with-queues.md) und [Abrufen der Werte für Ausstellername und -schlüssel](biztalk-issuer-name-issuer-key.md) |
| 5. |Developer |Installieren Sie das SDK, und erstellen Sie das BizTalk Service-Projekt in Visual Studio. |[Installieren des Azure BizTalk Services-SDK](https://msdn.microsoft.com/library/azure/hh689760.aspx) und [Erstellen von formatierten Messagingendpunkten in Azure](https://msdn.microsoft.com/library/azure/hh689766.aspx) |
| 6. |Developer |Stellen Sie Ihr BizTalk Service-Projekt in Ihrem bei Azure gehosteten BizTalk Service bereit. |[Bereitstellen und Aktualisieren des BizTalk Services-Projekts](https://msdn.microsoft.com/library/azure/hh689881.aspx) |
| 7. |Administrator |Gilt, wenn Sie EDI verwenden.  Sie können im Microsoft Azure BizTalk Services-Portal Partner hinzufügen und Vereinbarungen erstellen. Wenn Sie eine Vereinbarung erstellen, können Sie die vom Entwickler in den Vereinbarungseinstellungen erstellten Brücken oder Transformationen hinzufügen. |[Konfigurieren von EDI, AS2 und EDIFACT im BizTalk Services-Portal](https://msdn.microsoft.com/library/azure/hh689853.aspx) |
| 8. |Administrator |Überwachen Sie über das klassische Azure-Portal die Integrität Ihres BizTalk Service, einschließlich Leistungsmetriken. |[BizTalk Services: Registerkarten "Dashboard", "Überwachen" und "Skalieren"](http://go.microsoft.com/fwlink/p/?LinkID=302281) |
| 9. |Administrator |Verwalten Sie über das Microsoft Azure BizTalk Services-Portal die von BizTalk Services verwendeten Artefakte, und verfolgen Sie Nachrichten nach, während sie von den Brückendateien verarbeitet werden. |[Verwenden des BizTalk Services-Portals](https://msdn.microsoft.com/library/azure/dn874043.aspx) |
| 10. |Administrator |Erstellen Sie einen Sicherungsplan zum Sichern des BizTalk Service. |[Geschäftskontinuität und Notfallwiederherstellung in BizTalk Services](https://msdn.microsoft.com/library/azure/dn509557.aspx) |

## <a name="next-steps"></a>Nächste Schritte
[Tutorials und Beispiele](https://msdn.microsoft.com/library/azure/hh689895.aspx)

[Erstellen des Projekts in Visual Studio](https://msdn.microsoft.com/library/azure/hh689811.aspx)

[Installieren des Azure BizTalk Services-SDKs](https://msdn.microsoft.com/library/azure/hh689760.aspx)

## <a name="concepts"></a>Konzepte
[Erstellen des Projekts in Visual Studio](https://msdn.microsoft.com/library/azure/hh689811.aspx)  
[EDI, AS2 und EDIFACT-Messaging (B2B)](https://msdn.microsoft.com/library/azure/hh689898.aspx)  

## <a name="other-resources"></a>Weitere Ressourcen
[Quelle, Ziel und Bridge-Messagingendpunkte](https://msdn.microsoft.com/library/azure/hh689877.aspx)  
[Erlernen und Erstellen von Nachrichtenzuordnungen und -transformationen](https://msdn.microsoft.com/library/azure/hh689905.aspx)  
[Verwenden des BizTalk Adapter Service](https://msdn.microsoft.com/library/azure/hh689889.aspx)  
[BizTalk Services-Dokumentation](http://go.microsoft.com/fwlink/p/?LinkID=303664)



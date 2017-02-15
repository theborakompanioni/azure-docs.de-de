---
title: "Beispiele für Service Bus Messaging – Übersicht| Microsoft Docs"
description: "Kategorisiert und beschreibt Beispiele für Service Bus-Messaging und bietet entsprechende Links."
services: service-bus-messaging
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 0b420343-2d2a-4c65-98f1-ee0e39ef55c8
ms.service: service-bus-messaging
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/07/2016
ms.author: sethm
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 719403ca2ef90cdcf5f76fe15f7a33095e71a74c


---
# <a name="service-bus-messaging-samples"></a>Beispiele für Service Bus-Messaging
Die Beispiele für Service Bus-Messaging veranschaulichen die wichtigsten Features von [Service Bus-Messaging](https://azure.microsoft.com/services/service-bus/) (Clouddienst) und [Service Bus für Windows Server](https://msdn.microsoft.com/library/dn282144.aspx). Dieser Artikel kategorisiert und beschreibt die verfügbaren Beispiele und stellt Links zu jedem der Beispiele zur Verfügung.

> [!NOTE]
> Die Service Bus-Beispiele werden nicht zusammen mit dem SDK installiert. Zum Abrufen der Beispiele besuchen Sie die [Azure SDK-Beispielseite](https://code.msdn.microsoft.com/site/search?query=service%20bus&f%5B0%5D.Value=service%20bus&f%5B0%5D.Type=SearchText&ac=5).
> 
> Außerdem finden Sie [hier](https://github.com/Azure-Samples/azure-servicebus-messaging-samples) ein aktualisierten Satz von Service Bus-Nachrichtenbeispielen (sie werden in diesem Artikel nicht behandelt).  
> 
> 

Beispiele für Relays finden Sie unter [Service Bus Relay-Beispiele](../service-bus-relay/service-bus-relay-samples.md).

## <a name="service-bus-messaging"></a>Service Bus Messaging
Die folgenden Beispiele veranschaulichen das Schreiben von Anwendungen, die Service Bus-Messaging verwenden.

Beachten Sie, dass die Messagingbeispiele eine Verbindungszeichenfolge für den Zugriff auf den Service Bus-Namespace erfordern.

### <a name="to-obtain-a-connection-string-for-azure-service-bus"></a>So rufen Sie eine Verbindungszeichenfolge für Azure Service Bus ab
1. Melden Sie sich beim [Azure-Portal](http://portal.azure.com)an.
2. Klicken Sie in der linken Spalte auf **Service Bus**.
3. Klicken Sie in der Liste auf den Namen Ihres Namespace.
4. Klicken Sie auf dem Blatt „Namespace“ auf **Richtlinien für gemeinsamen Zugriff**.
5. Klicken Sie auf dem Blatt **Richtlinien für gemeinsamen Zugriff** auf **RootManageSharedAccessKey**.
6. Kopieren Sie die Verbindungszeichenfolge in die Zwischenablage.

### <a name="to-obtain-a-connection-string-for-service-bus-for-windows-server"></a>So rufen Sie eine Verbindungszeichenfolge für Service Bus für Windows Server ab
1. Führen Sie das folgende PowerShell-Cmdlet aus:
   
    ```
    get-sbClientConfiguration
    ```
2. Fügen Sie die Verbindungszeichenfolge in die Datei "App.config" für das Beispiel ein.

### <a name="getting-started-samples"></a>Beispiele für erste Schritte
Diese Beispiele beschreiben grundlegende Messagingfunktionen.

| Name des Beispiels | Beschreibung | Mindestversion des SDK | Availability |
| --- | --- | --- | --- |
| [Erste Schritte: Messaging mit Warteschlangen](http://code.msdn.microsoft.com/Getting-Started-Brokered-aa7a0ac3) |Zeigt, wie Microsoft Azure Service Bus zum Senden und Empfangen von Nachrichten aus einer Warteschlange verwendet wird. |1.8 |Microsoft Azure Service Bus; Service Bus für Windows Server |
| [Erste Schritte: Messaging mit Themen](http://code.msdn.microsoft.com/Getting-Started-Brokered-614d42e5) |Zeigt, wie Microsoft Azure Service Bus zum Senden und Empfangen von Nachrichten aus einer Thema mit mehreren Abonnements verwendet wird. |1.8 |Microsoft Azure Service Bus; Service Bus für Windows Server |

### <a name="exploring-features"></a>Erkunden von Features
Die folgenden Beispiele zeigen verschiedene Features von Service Bus.

| Name des Beispiels | Beschreibung | Mindestversion des SDK | Availability |
| --- | --- | --- | --- |
| [HTTP-Tokenanbieter](http://code.msdn.microsoft.com/Service-Bus-HTTP-Token-38f2cfc5) |Zeigt die verschiedenen Arten der Authentifizierung eines HTTP/REST-Clients bei Service Bus. |2.1 |Microsoft Azure Service Bus; Service Bus für Windows Server |
| [Service Bus-HTTP-Client](http://code.msdn.microsoft.com/Service-Bus-HTTP-client-fe7da74a) |Zeigt, wie Nachrichten über HTTP/REST an Service Bus gesendet und von Service Bus empfangen werden. |2.3 |Microsoft Azure Service Bus; Service Bus für Windows Server |
| [Automatische Service Bus-Weiterleitung](http://code.msdn.microsoft.com/Service-Bus-Autoforwarding-b9df470b) |Zeigt, wie Nachrichten von einer Warteschlange, einem Abonnement oder einer Warteschlange für unzustellbare Nachrichten automatisch an eine andere Warteschlange oder an ein anderes Thema weitergeleitet werden. Außerdem wird gezeigt, wie eine Nachricht über eine Übertragungswarteschlange an eine Warteschlange oder ein Thema gesendet wird. |2.3 |Microsoft Azure Service Bus; Service Bus für Windows Server |
| [Brokermessaging: Beispiel für eine WCF-Kanalsitzung](http://code.msdn.microsoft.com/Brokered-Messaging-WCF-0a526451) |Zeigt, wie Microsoft Azure Service Bus mithilfe von WCF-Kanälen (Windows Communication Foundation) verwendet wird. Das Beispiel zeigt, wie WCF-Kanäle zum Senden und Empfangen von Nachrichten über eine Service Bus-Warteschlange verwendet werden. Das Beispiel zeigt die sitzungsbasierte und nicht sitzungsbasierte Kommunikation über Service Bus. |1.8 |Microsoft Azure Service Bus; Service Bus für Windows Server |
| [Brokermessaging: Transaktionen](http://code.msdn.microsoft.com/Brokered-Messaging-8cd41d1e) |Zeigt die Verwendung von Microsoft Azure Service Bus-Messagingfunktionen in einem Transaktionsbereich, um sicherzustellen, dass das Commit für Batches von Messagingvorgängen atomar erfolgt. |1.8 |Microsoft Azure Service Bus; Service Bus für Windows Server |
| [Brokermessaging: Verwaltungsvorgänge mithilfe von REST](http://code.msdn.microsoft.com/Brokered-Messaging-569cff88) |Zeigt, wie Verwaltungsvorgänge für Service Bus mithilfe von REST ausgeführt werden. |1.8 |Microsoft Azure Service Bus; Service Bus für Windows Server |
| [Ressourcenanbieter-REST-APIs](http://code.msdn.microsoft.com/Service-Bus-Resource-5d887203) |Zeigt, wie die neuen RDFE-REST-APIs von Service Bus verwendet werden, um die Namespaces und Messagingentitäten zu verwalten. |1.8 |Microsoft Azure Service Bus; Service Bus für Windows Server |
| [Brokermessaging: Beispiel für eine WCF-Dienstsitzung](http://code.msdn.microsoft.com/Brokered-Messaging-WCF-db4262c2) |Zeigt, wie Microsoft Azure Service Bus mithilfe des WCF-Dienstmodells verwendet wird. Das Beispiel zeigt die Verwendung des WCF-Dienstmodells zum Erzielen einer sitzungsbasierten Kommunikation über eine Service Bus-Warteschlange. |1.8 |Microsoft Azure Service Bus; Service Bus für Windows Server |
| [Brokermessaging: Anforderung/Antwort](http://code.msdn.microsoft.com/Brokered-Messaging-Request-2b4ff5d8) |Zeigt, wie Microsoft Azure Service Bus und die Funktion "Anforderung/Antwort" verwendet werden. Das Beispiel zeigt einfache Clients und Server, die über eine Service Bus-Warteschlange kommunizieren. |1.8 |Microsoft Azure Service Bus; Service Bus für Windows Server |
| [Brokermessaging: Warteschlange für unzustellbare Nachrichten](http://code.msdn.microsoft.com/Brokered-Messaging-Dead-22536dd8) |Zeigt die Verwendung von Microsoft Azure Service Bus und der Messagingfunktion "Warteschlange für unzustellbare Nachrichten". Das Beispiel zeigt einen einfachen Absender und Empfänger, die über eine Service Bus-Warteschlange kommunizieren. |1.8 |Microsoft Azure Service Bus; Service Bus für Windows Server |
| [Brokermessaging: Zurückgestellten Nachrichten](http://code.msdn.microsoft.com/Brokered-Messaging-ccc4f879) |Zeigt, wie das Feature zur Nachrichtenverzögerung von Microsoft Azure Service Bus verwendet wird. Das Beispiel zeigt einen einfachen Absender und Empfänger, die über eine Service Bus-Warteschlange kommunizieren. |1.8 |Microsoft Azure Service Bus; Service Bus für Windows Server |
| [Brokermessaging: Sitzungsnachrichten](http://code.msdn.microsoft.com/Brokered-Messaging-Session-41c43fb4) |Zeigt, wie Microsoft Azure Service Bus und die Funktion "Nachrichtensitzung" verwendet werden. Das Beispiel zeigt einfache Absender und Empfänger, die über eine Service Bus-Warteschlange kommunizieren. |1.8 |Microsoft Azure Service Bus; Service Bus für Windows Server |
| [Brokermessaging: Anforderung/Antwort (Thema)](http://code.msdn.microsoft.com/Brokered-Messaging-Request-6759a36e) |Zeigt, wie das Muster „Anforderung/Antwort“ mithilfe von Microsoft Azure Service Bus-Themen und -Abonnements implementiert wird. Das Beispiel zeigt einfache Clients und Server, die über ein Service Bus-Thema kommunizieren. |1.8 |Microsoft Azure Service Bus; Service Bus für Windows Server |
| [Brokermessaging: Anforderung/Antwort (Warteschlange)](http://code.msdn.microsoft.com/Brokered-Messaging-Request-0ce8fcaf) |Zeigt, wie Microsoft Azure Service Bus und die Funktion "Anforderung/Antwort" verwendet werden. Das Beispiel zeigt einfache Clients und Server, die über zwei Service Bus-Warteschlangen kommunizieren. |1.8 |Microsoft Azure Service Bus; Service Bus für Windows Server |
| [Brokermessaging: Duplikaterkennung](http://code.msdn.microsoft.com/Brokered-Messaging-c0acea25) |Zeigt, wie die Erkennung doppelt vorhandener Nachrichten in Microsoft Azure Service Bus mit Warteschlangen verwendet wird. Es werden zwei Warteschlangen erstellt: eine Warteschlange mit aktivierter Duplikaterkennung und eine Warteschlange ohne Duplikaterkennung. |1.8 |Microsoft Azure Service Bus; Service Bus für Windows Server |
| [Brokermessaging: Asynchrones Messaging](http://code.msdn.microsoft.com/Brokered-Messaging-Async-211c1e74) |Zeigt, wie Microsoft Azure Service Bus zum asynchronen Senden und Empfangen von Nachrichten aus einer Warteschlange verwendet wird. Die Warteschlange stellt entkoppelte, asynchrone Kommunikation zwischen einem Absender und einer beliebigen Anzahl von Empfängern bereit (in diesem Fall einem einzigen Empfänger). |1.8 |Microsoft Azure Service Bus; Service Bus für Windows Server |
| [Brokermessaging: Erweiterte Filter](http://code.msdn.microsoft.com/Brokered-Messaging-6b0d2749) |Zeigt, wie erweiterte Filter für Microsoft Azure Service Bus-Veröffentlichung/-Abonnements verwendet werden. Ein Thema und drei Abonnements werden mit verschiedenen Filterdefinitionen erstellt, Nachrichten an das Thema gesendet und alle Nachrichten aus Abonnements empfangen. |1.8 |Microsoft Azure Service Bus; Service Bus für Windows Server |
| [Brokermessaging: Vorabruf von Nachrichten](http://code.msdn.microsoft.com/Brokered-Messaging-be2dac1d) |Zeigt, wie die Vorabruffunktion für Nachrichten von Microsoft Azure Service Bus verwendet wird. Zeigt, wie die Vorabruffunktion für Nachrichten beim Empfang verwendet wird. |1.8 |Microsoft Azure Service Bus; Service Bus für Windows Server |

## <a name="service-bus-reference-tools"></a>Service Bus-Referenztools
Die folgenden Beispiele zeigen verschiedene weitere Features des Diensts.

| Name des Beispiels | Beschreibung | Mindestversion des SDK | Availability |
| --- | --- | --- | --- |
| [Service Bus-Explorer](http://code.msdn.microsoft.com/Service-Bus-Explorer-f2abca5a) |Mit dem Service Bus-Explorer können Benutzer eine Verbindung mit einem Service Bus-Dienstnamespace herstellen und Messagingentitäten auf einfache Weise verwalten. Das Tool stellt erweiterte Features (z. B. Import-/Exportfunktionen) und Testmöglichkeiten für Messagingentitäten und Relaydienste zur Verfügung. |1.8 |Microsoft Azure Service Bus; Service Bus für Windows Server |
| [Autorisierung: SBAzTool](http://code.msdn.microsoft.com/Authorization-SBAzTool-6fd76d93) |Dieses Beispiel zeigt das Erstellen und Verwalten von Dienstidentitäten in Microsoft Azure Active Directory Access Control (auch Access Control Service oder ACS) für die Verwendung mit Service Bus. |N/V |Microsoft Azure Servicebus |

## <a name="next-steps"></a>Nächste Schritte
Konzeptionelle Übersichten über Service Bus finden Sie in den folgenden Themen.

* [Übersicht über Service Bus-Messaging](service-bus-messaging-overview.md)
* [Service Bus-Architektur](service-bus-architecture.md)
* [Service Bus – Grundlagen](service-bus-fundamentals-hybrid-solutions.md)




<!--HONumber=Nov16_HO3-->



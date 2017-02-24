---
title: "Übersicht über Azure Service Bus Relay-Beispiele | Microsoft-Dokumentation"
description: Kategorisiert und beschreibt Service Bus Relay-Beispiele und bietet entsprechende Links.
services: service-bus-relay
documentationcenter: na
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 9237a9a2-f126-4d3f-9f9b-604ee6b32153
ms.service: service-bus-relay
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/13/2017
ms.author: sethm
translationtype: Human Translation
ms.sourcegitcommit: 6027c973f0104fff9705a70f7812f62562165c1e
ms.openlocfilehash: d830770c49caeacf762382ce1c6113d9196a5994


---
# <a name="service-bus-relay-samples"></a>Service Bus Relay-Beispiele
Die Service Bus Relay-Beispiele veranschaulichen wichtige Funktionen in [Service Bus Relay](https://azure.microsoft.com/services/service-bus/). Dieser Artikel kategorisiert und beschreibt die verfügbaren Beispiele und stellt Links zu jedem der Beispiele zur Verfügung.

> [!NOTE]
> Die Service Bus-Beispiele werden nicht zusammen mit dem Azure SDK installiert. Zum Abrufen der Beispiele besuchen Sie die [Azure SDK-Beispielseite](https://code.msdn.microsoft.com/site/search?query=service%20bus&f%5B0%5D.Value=service%20bus&f%5B0%5D.Type=SearchText&ac=5).
> 
> Darüber hinaus finden Sie [hier](https://github.com/Azure-Samples/azure-servicebus-relay-samples) einen aktualisierten Satz von Service Bus Relay-Beispielen.  
> 
> 

Beispiele für das Service Bus-Messaging finden Sie unter [Beispiele für Service Bus-Messaging](../service-bus-messaging/service-bus-samples.md).

## <a name="azure-service-bus-relay"></a>Azure Service Bus Relay
Die folgenden Beispiele veranschaulichen das Schreiben von Anwendungen, die den Azure Relay-Dienst verwenden.

Beachten Sie, dass die Relaybeispiele eine Verbindungszeichenfolge für den Zugriff auf den Relay-Namespace erfordern.

### <a name="to-obtain-a-connection-string-for-azure-relay"></a>So rufen Sie eine Verbindungszeichenfolge für Azure Relay ab
1. Melden Sie sich beim [Azure-Portal](http://portal.azure.com)an.
2. Klicken Sie, um die Liste Ihrer Namespaces im Bereich **Alle Ressourcen** zu erweitern.
3. Klicken Sie auf den Namen Ihres Relay-Namespace in der Liste.
4. Klicken Sie auf dem Blatt „Namespace“ auf **Richtlinien für gemeinsamen Zugriff**.
5. Klicken Sie auf dem Blatt **Richtlinien für gemeinsamen Zugriff** auf **RootManageSharedAccessKey**.
6. Kopieren Sie die Verbindungszeichenfolge in die Zwischenablage.

### <a name="to-obtain-a-connection-string-for-service-bus-for-windows-server"></a>So rufen Sie eine Verbindungszeichenfolge für Service Bus für Windows Server ab
1. Führen Sie das folgende PowerShell-Cmdlet aus:
   
    ```powershell
    get-sbClientConfiguration
    ```
2. Fügen Sie die Verbindungszeichenfolge in die Datei "App.config" für das Beispiel ein.

## <a name="azure-relay"></a>Azure Relay
Beispiele, in denen Azure Relay gezeigt wird.

### <a name="getting-started"></a>Erste Schritte
| Name des Beispiels | Beschreibung | Mindestversion des SDK | Verfügbarkeit |
| --- | --- | --- | --- |
| [WCF Relayed Messaging : Azure](http://code.msdn.microsoft.com/Relayed-Messaging-Windows-0d2cede3) |Veranschaulicht das Ausführen eines Service Bus-Clients und -Diensts unter Azure. In diesem Beispiel wird Service Bus programmgesteuert konfiguriert. Nur Umgebungs- und Sicherheitsinformationen sind in den Konfigurationsdateien gespeichert. |1.8 |Microsoft Azure Servicebus |
| [WCF Relayed Messaging Authentication: Shared Secret](http://code.msdn.microsoft.com/Relayed-Messaging-92b04c02) |Zeigt, wie ein Ausstellername und ein geheimer Schlüssel des Ausstellers für die Authentifizierung bei Service Bus verwendet werden. |1.8 |Microsoft Azure Servicebus |
| [WCF Relayed Messaging Authentication: WebNoAuth](http://code.msdn.microsoft.com/Relayed-Messaging-a4f0b831) |Zeigt das Verfügbarmachen eines HTTP-Diensts, der keine Clientbenutzerauthentifizierung erfordert. |1.8 |Microsoft Azure Servicebus |
| [WCF Relayed Messaging Bindings: WebHttp](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-a6477ba0) |Zeigt die Verwendung der Bindung **WebHttpRelayBinding** zum Zurückgeben von Binärdaten mithilfe des Webprogrammiermodells. |1.8 |Microsoft Azure Servicebus |
| [WCF Relayed Messaging Bindings: NetTcp Relayed](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-2dec7692) |Zeigt die Verwendung der Bindung **NetTcpRelayBinding** . |1.8 |Microsoft Azure Servicebus |

### <a name="exploring-features"></a>Erkunden von Features
Beispiele, die verschiedene Features von Service Bus Relay demonstrieren.

| Name des Beispiels | Beschreibung | Mindestversion des SDK | Verfügbarkeit |
| --- | --- | --- | --- |
| [WCF Relayed Messaging Authentication: Simple WebToken](http://code.msdn.microsoft.com/Relayed-Messaging-32c74392) |Zeigt die Verwendung von SWT-Anmeldeinformationen (Simple Web Token) für die Authentifizierung bei Service Bus. Dieses Beispiel ähnelt mit Ausnahme einiger Änderungen dem Echobeispiel. Insbesondere fügt dieses Beispiel den ServiceHost- (Dienst) und ChannelFactory-Anwendungen (Client) ein Verhalten hinzu. |1.8 |Microsoft Azure Servicebus |
| [WCF Relayed Messaging: Load Balance](http://code.msdn.microsoft.com/Relayed-Messaging-Load-bd76a9f8) |Zeigt die Verwendung von Microsoft Azure Service Bus zum Routen von Nachrichten an mehrere Empfänger. Das Beispiel zeigt mehrere Instanzen eines einfachen Diensts, der mit einem Client über die Bindung **NetTcpRelayBinding** kommuniziert. |1.8 |Microsoft Azure Servicebus |
| [WCF Relayed Messaging Bindings: Net Event](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-c0176977) |Zeigt die Verwendung der Bindung **NetEventRelayBinding** für Microsoft Azure Service Bus. |1.8 |Microsoft Azure Servicebus |
| [WCF Relayed Messaging Bindings: WS2007Http Session](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-ef1f1fcb) |Veranschaulicht die Verwendung der Bindung **WS2007HttpRelayBinding** mit aktivierten zuverlässigen Sitzungen. Das Beispiel zeigt außerdem, wie Service Bus-Anmeldeinformationen in der Konfigurationsdatei statt programmgesteuert angegeben werden. |1.8 |Microsoft Azure Servicebus |
| [WCF Relayed Messaging Bindings: WS2007Http MsgSecCertificate](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-f29c9da5) |Zeigt, wie die Bindung **WS2007HttpRelayBinding** mit Nachrichtensicherheit zum Absichern von End-to-End-Nachrichten verwendet wird, wobei die Authentifizierung von Clients bei Service Bus weiterhin erforderlich ist. |1.8 |Microsoft Azure Servicebus |
| [WCF Relayed Messaging: Metadata Exchange](http://code.msdn.microsoft.com/Relayed-Messaging-Metadata-f122312e) |Veranschaulicht das Verfügbarmachen eines Metadatenendpunkts, der die Relaybindung verwendet. **MetadataExchange** wird in den folgenden Relaybindungen unterstützt: **NetTcpRelayBinding**, **NetOnewayRelayBinding**, **BasicHttpRelayBinding** und **WS2007HttpRelayBinding**. |1.8 |Microsoft Azure Servicebus |
| [WCF Relayed Messaging Bindings: NetTcp Direct](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-ca039161) |Zeigt die Konfiguration der Bindung **NetTcpRelayBinding** zum Unterstützen des **hybriden** Verbindungsmodus, durch den zuerst eine Relayverbindung eingerichtet und anschließend nach Möglichkeit automatisch zu einer direkten Verbindung zwischen einem Client und einem Dienst gewechselt wird. |1.8 |Microsoft Azure Servicebus |
| [WCF Relayed Messaging Bindings: NetTcp MsgSec UserName](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-30542392) |Zeigt die Verwendung der Bindung **NetTcpRelayBinding** mit Nachrichtensicherheit. |1.8 |Microsoft Azure Servicebus |
| [WCF Relayed Messaging Bindings: Net Oneway](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-bb5b813a) |Zeigt das Bereitstellen und das Verwenden eines Dienstendpunkts mithilfe der Bindung **NetOnewayRelayBinding** . |1.8 |Microsoft Azure Servicebus |
| [WCF Relayed Messaging Bindings: WS2007Http Simple](http://code.msdn.microsoft.com/Relayed-Messaging-Bindings-aa4b793a) |Zeigt die Verwendung der Bindung **WS2007HttpRelayBinding** . Zeigt einen einfachen Dienst, der keine Sicherheitsoptionen verwendet und keine Authentifizierung von Clients erfordert. |1.8 |Microsoft Azure Servicebus |

## <a name="next-steps"></a>Nächste Schritte
Konzeptionelle Übersichten über Service Bus finden Sie in den folgenden Themen.

* [Übersicht über Azure Relay](relay-what-is-it.md)
* [Service Bus-Architektur](../service-bus-messaging/service-bus-architecture.md)
* [Service Bus – Grundlagen](../service-bus-messaging/service-bus-fundamentals-hybrid-solutions.md)




<!--HONumber=Feb17_HO2-->



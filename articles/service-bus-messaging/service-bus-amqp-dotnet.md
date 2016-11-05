---
title: Service Bus mit .NET und AMQP 1.0 | Microsoft Docs
description: Verwenden von Service Bus aus .NET mit AMQP
services: service-bus
documentationcenter: na
author: sethmanheim
manager: timlt
editor: ''

ms.service: service-bus
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/03/2016
ms.author: sethm

---
# <a name="using-service-bus-from-.net-with-amqp-1.0"></a>Verwenden von Service Bus aus .NET mit AMQP 1.0
[!INCLUDE [service-bus-selector-amqp](../../includes/service-bus-selector-amqp.md)]

## <a name="downloading-the-service-bus-sdk"></a>Herunterladen des Service Bus SDK
Unterstützung für AMQP 1.0 ist im Service Bus SDK ab Version 2.1 verfügbar. Sie können sicherstellen, dass Sie über die aktuelle Version verfügen, indem Sie die Service Bus-Komponenten von[NuGet][NuGet] herunterladen.

## <a name="configuring-.net-applications-to-use-amqp-1.0"></a>Konfigurieren von .NET-Anwendungen für das Verwenden von AMQP 1.0
Mithilfe eines dedizierten SOAP-basierten Protokolls kommuniziert die .NET-Clientbibliothek von Service Bus standardmäßig mit dem Service Bus-Dienst. Wenn Sie anstatt des Standardprotokolls AMQP 1.0 verwenden möchten, ist eine explizite Konfiguration der Service Bus-Verbindungszeichenfolge erforderlich, die im nächsten Abschnitt beschrieben wird. Abgesehen von dieser Änderung bleibt der Anwendungscode bei Verwendung von AMQP 1.0 unverändert.

In der aktuellen Version gibt es ein paar API-Features, die bei Verwendung von AMQP nicht unterstützt werden. Diese nicht unterstützten Features sind weiter unten im Abschnitt [Nicht unterstützte Features, Einschränkungen und Verhaltensunterschiede](#unsupported-features-restrictions-and-behavioral-differences) aufgeführt. Darüber hinaus haben einige der erweiterten Konfigurationseinstellungen eine unterschiedliche Bedeutung, wenn AMQP zum Einsatz kommt.

### <a name="configuration-using-app.config"></a>Konfiguration mithilfe von "App.config"
Es ist empfehlenswert, für Anwendungen die Konfigurationsdatei "App.config" zu verwenden, um die Einstellungen zu speichern. Bei Service Bus-Anwendungen können Sie die Datei „App.config“ verwenden, um die Einstellungen für die Service Bus-**Verbindungszeichenfolge** zu speichern. Es folgt ein Beispiel der Datei "App.config":

    <?xml version="1.0" encoding="utf-8" ?>
    <configuration>
        <appSettings>
            <add key="Microsoft.ServiceBus.ConnectionString"
                 value="Endpoint=sb://[namespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[SAS key];TransportType=Amqp" />
        </appSettings>
    </configuration>

Beim Wert der Einstellung `Microsoft.ServiceBus.ConnectionString` handelt es sich um die Service Bus-Verbindungszeichenfolge, die zum Konfigurieren der Verbindung mit Service Bus verwendet wird. Das Format sieht folgendermaßen aus:

    Endpoint=sb://[namespace].servicebus.windows.net/;SharedAccessKeyName=RootManageSharedAccessKey;SharedAccessKey=[SAS key];TransportType=Amqp

`[namespace]` und `SharedAccessKey` werden aus dem [Azure-Portal][Azure-Portal] bezogen. Weitere Informationen finden Sie unter [Verwenden von Service Bus-Warteschlangen][].

Wenn Sie AMQP verwenden, fügen Sie `;TransportType=Amqp` an die Verbindungszeichenfolge an. Diese Notation weist die Clientbibliothek an, die Verbindung mit Service Bus über AMQP 1.0 herzustellen.

## <a name="message-serialization"></a>Nachrichtenserialisierung
Bei Verwendung des Standardprotokolls sieht das Standardserialisierungsverhalten der .NET-Clientbibliothek die Verwendung des [DataContractSerializer][DataContractSerializer]-Typs zum Serialisieren einer [BrokeredMessage][BrokeredMessage]-Instanz für die Übertragung zwischen der Clientbibliothek und dem Service Bus-Dienst vor. Bei Verwendung des AMQP-Transportmodus verwendet die Clientbibliothek das AMQP-Typsystem für die Serialisierung der [im Broker gespeicherten Nachricht][BrokeredMessage] in eine AMQP-Nachricht. Diese Serialisierung ermöglicht, dass die Nachricht von einer empfangenden Anwendung empfangen und interpretiert wird, die möglicherweise auf einer anderen Plattform ausgeführt wird, z. B. einer Java-Anwendung, die die JMS-API für den Zugriff auf Service Bus verwendet.

Wenn Sie eine [BrokeredMessage][BrokeredMessage]-Instanz erstellen, können Sie ein .NET-Objekt als Parameter für den Konstruktor bereitstellen, der als Text der Nachricht dient. Für Objekte, die primitiven AMQP-Typen zugeordnet werden können, wird der Text in AMQP-Datentypen serialisiert. Wenn das Objekt nicht direkt einem primitiven AMQP-Typ, d.h. einem von der Anwendung definierten benutzerdefinierten Typ, zugeordnet werden kann, wird das Objekt mithilfe von [DataContractSerializer][DataContractSerializer] serialisiert, und die serialisierten Bytes werden in einer AMQP-Datennachricht gesendet.

Um die Interoperabilität mit Nicht-.NET-Clients zu erleichtern, verwenden Sie nur .NET-Typen, die für den Text der Nachricht direkt in AMQP-Typen serialisiert werden können. Die folgende Tabelle enthält diese Typen und die entsprechende Zuordnung zum AMQP-Typsystem.

| .NET Body-Objekttyp | Zugeordneter AMQP-Typ | AMQP Body-Abschnittstyp |
| --- | --- | --- |
| bool |Boolescher Wert |AMQP Value |
| byte |ubyte |AMQP Value |
| ushort |ushort |AMQP Value |
| uint |uint |AMQP Value |
| ulong |ulong |AMQP Value |
| sbyte |byte |AMQP Value |
| short |short |AMQP Value |
| int |int |AMQP Value |
| lang |lang |AMQP Value |
| float |float |AMQP Value |
| double |double |AMQP Value |
| decimal |decimal128 |AMQP Value |
| char |char |AMQP Value |
| DateTime |timestamp |AMQP Value |
| GUID |uuid |AMQP Value |
| Byte[] |binary |AMQP Value |
| string |string |AMQP Value |
| System.Collections.IList |list |AMQP Value: Diese Auflistung kann nur Elemente enthalten, die in dieser Tabelle definiert sind. |
| System.Array |array |AMQP Value: Diese Auflistung kann nur Elemente enthalten, die in dieser Tabelle definiert sind. |
| System.Collections.IDictionary |map |AMQP Value: Diese Auflistung kann nur Elemente enthalten, die in dieser Tabelle definiert sind. Hinweis: nur "String"-Schlüssel werden unterstützt. |
| Uri |Beschriebene Zeichenfolge (siehe die folgende Tabelle) |AMQP Value |
| DateTimeOffset |Lange Beschreibung (siehe die folgende Tabelle) |AMQP Value |
| TimeSpan |Lange Beschreibung (siehe die folgende Tabelle) |AMQP Value |
| Datenstrom |binary |AMQP Data (können mehrere sein). Die "Data"-Abschnitte enthalten die rohen Bytes, die aus dem "Stream"-Objekt gelesen. |
| Other Object |binary |AMQP Data (können mehrere sein). Enthält die serialisierten Binärdaten des Objekts, das "DataContractSerializer" oder einen von der Anwendung bereitgestellten Serialisierer verwendet. |

| .NET-Typ | Zugeordneter beschriebener AMQP-Typ | Hinweise |
| --- | --- | --- |
| Uri |`<type name=”uri” class=restricted source=”string”> <descriptor name=”com.microsoft:uri” /></type>` |Uri.AbsoluteUri |
| Datetimeoffset |`<type name=”datetime-offset” class=restricted source=”long”> <descriptor name=”com.microsoft:datetime-offset” /></type>` |DateTimeOffset.UtcTicks |
| TimeSpan |`<type name=”timespan” class=restricted source=”long”> <descriptor name=”com.microsoft:timespan” /></type> ` |TimeSpan.Ticks |

## <a name="unsupported-features,-restrictions,-and-behavioral-differences"></a>Nicht unterstützte Features, Einschränkungen und Verhaltensunterschiede
Die folgenden Features der .NET-API von Service Bus werden bei Verwendung von AMQP derzeit nicht unterstützt:

* Transaktionen
* Senden mithilfe eines Übertragungsziels

Es gibt bei Verwenden von AMQP im Vergleich zum Standardprotokoll auch einige kleine Unterschiede beim Verhalten der .NET-API von Service Bus:

* Die [OperationTimeout][OperationTimeout]-Eigenschaft wird ignoriert.
* `MessageReceiver.Receive(TimeSpan.Zero)` wird als `MessageReceiver.Receive(TimeSpan.FromSeconds(10))` implementiert.

## <a name="controlling-amqp-protocol-settings"></a>Steuern von AMQP-Protokolleinstellungen
Die .NET-APIs machen mehrere Einstellungen zum Steuern des Verhaltens des AMQP-Protokolls verfügbar:

* **MessageReceiver.PrefetchCount**: Steuert die anfänglichen Rechte, die auf einen Link angewendet werden. Der Standardwert ist 0.
* **MessagingFactorySettings.AmqpTransportSettings.MaxFrameSize**: Steuert die maximale AMQP-Framegröße, die während der Verhandlung zum Herstellen der Verbindung angeboten wird. Der Standardwert ist 65.536 Bytes.
* **MessagingFactorySettings.AmqpTransportSettings.BatchFlushInterval**: Wenn Übertragungen in Batches erfolgen können, bestimmt dieser Wert die maximale Verzögerung für das Senden von Einteilungen. Wird von Absendern/Empfängern standardmäßig vererbt. Einzelne Sender/Empfänger können die Standardeinstellung (20 Millisekunden) überschreiben.
* **MessagingFactorySettings.AmqpTransportSettings.UseSslStreamSecurity**: Steuert, ob AMQP-Verbindungen über eine SSL-Verbindung hergestellt werden. Der Standardwert ist **True**.

## <a name="next-steps"></a>Nächste Schritte
Möchten Sie mehr erfahren? Nutzen Sie die folgenden Links:

* [Übersicht über Service Bus AMQP]
* [AMQP 1.0-Unterstützung für partitionierte Warteschlangen und Themen von Service Bus]
* [AMQP in Service Bus für Windows Server]

[Erste Schritte mit Service Bus-Warteschlangen]: service-bus-dotnet-get-started-with-queues.md
[DataContractSerializer]: https://msdn.microsoft.com/library/azure/system.runtime.serialization.datacontractserializer.aspx
[BrokeredMessage]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.brokeredmessage.aspx
[Microsoft.ServiceBus.Messaging.MessagingFactory.AcceptMessageSession]: https://msdn.microsoft.com/library/azure/jj657638.aspx
[Microsoft.ServiceBus.Messaging.MessagingFactory.CreateMessageSender(System.String,System.String)]: https://msdn.microsoft.com/library/azure/jj657703.aspx
[OperationTimeout]: https://msdn.microsoft.com/library/azure/microsoft.servicebus.messaging.messagingfactorysettings.operationtimeout.aspx
[NuGet]: http://nuget.org/packages/WindowsAzure.ServiceBus/
[Azure-Portal]: https://portal.azure.com
[Übersicht über Service Bus AMQP]: service-bus-amqp-overview.md
[AMQP 1.0-Unterstützung für partitionierte Warteschlangen und Themen von Service Bus]: service-bus-partitioned-queues-and-topics-amqp-overview.md
[AMQP in Service Bus für Windows Server]: https://msdn.microsoft.com/library/dn574799.aspx


<!--HONumber=Oct16_HO2-->



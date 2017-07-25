---
title: Grundlegendes zum integrierten Azure IoT Hub-Endpunkt | Microsoft-Dokumentation
description: "Entwicklerhandbuch: Beschreibt, wie der integrierte, Event Hub-kompatible Endpunkt verwendet wird, um Nachrichten zu lesen, die von Geräten an die Cloud gesendet werden."
services: iot-hub
documentationcenter: .net
author: dominicbetts
manager: timlt
editor: 
ms.service: iot-hub
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/25/2017
ms.author: dobett
ms.translationtype: Human Translation
ms.sourcegitcommit: 43aab8d52e854636f7ea2ff3aae50d7827735cc7
ms.openlocfilehash: fcc3743028e369fdc42b71887d49fb41fba2c0dc
ms.contentlocale: de-de
ms.lasthandoff: 06/03/2017


---
# <a name="read-device-to-cloud-messages-from-the-built-in-endpoint"></a>Lesen von Nachrichten, die von Geräten an die Cloud gesendet werden, vom integrierten Endpunkt

Standardmäßig werden Nachrichten an den integrierten dienstseitigen Endpunkt (**messages/events**) gesendet, der mit [Event Hubs][lnk-event-hubs] kompatibel ist. Dieser Endpunkt wird derzeit nur mit dem [AMQP][lnk-amqp]-Protokoll über Port 5671 verfügbar gemacht. Ein IoT Hub macht die folgenden Eigenschaften verfügbar, damit Sie den integrierten Event Hub-kompatiblen Messagingendpunkt für **messages/events** steuern können.

| Eigenschaft            | Beschreibung |
| ------------------- | ----------- |
| **Anzahl von Partitionen** | Legen Sie diese Eigenschaft bei der Erstellung fest, um die Anzahl von [Partitionen][lnk-event-hub-partitions] für die D2C-Ereigniserfassung zu definieren. |
| **Aufbewahrungsdauer**  | Diese Eigenschaft gibt den Zeitraum in Tagen an, für den Nachrichten von IoT Hub aufbewahrt werden. Als Standardwert ist ein Tag festgelegt, dieser Wert kann jedoch auf sieben Tage erhöht werden. |

Mit dem IoT Hub können Sie außerdem Consumergruppen auf dem integrierten D2C-Empfangsendpunkt verwalten.

Standardmäßig werden alle Nachrichten, für die sich nicht explizit eine Übereinstimmung mit einer Nachrichtenroutingregel ergibt, auf den integrierten Endpunkt geschrieben. Wenn Sie diese Fallbackroute deaktivieren, werden Nachrichten verworfen, für die sich keine expliziten Übereinstimmungen mit Nachrichtenroutingregeln ergeben.

Sie können die Aufbewahrungsdauer sowohl programmgesteuert über die [IoT Hub-Ressourcenanbieter-REST-APIs][lnk-resource-provider-apis] als auch über das [Azure-Portal][lnk-management-portal] ändern.

IoT Hub stellt den integrierten Endpunkt **messages/events** für Ihre Back-End-Dienste bereit, damit die D2C-Nachrichten gelesen werden können, die von Ihrem Hub empfangen werden. Der Endpunkt ist Event Hub-kompatibel, sodass Sie zum Lesen von Nachrichten alle Mechanismen verwenden können, die der Event Hubs-Dienst unterstützt.

## <a name="read-from-the-built-in-endpoint"></a>Lesen vom integrierten Endpunkt

Wenn Sie das [Azure Service Bus-SDK für .NET][lnk-servicebus-sdk] oder den [Event Hubs-Ereignisprozessorhost][lnk-eventprocessorhost] verwenden, können Sie eine beliebige IoT Hub-Verbindungszeichenfolge mit den richtigen Berechtigungen verwenden. Anschließend verwenden Sie **messages/events** als Event Hub-Name.

Wenn Sie SDKs (oder Produktintegrationen) verwenden, die nicht IoT Hub-fähig sind, müssen Sie einen Event Hub-kompatiblen Endpunkt und den Event Hub-kompatiblen Namen aus den IoT Hub-Einstellungen im [Azure-Portal][lnk-management-portal] abrufen:

1. Klicken Sie auf dem Blatt „IoT Hub“ auf **Endpunkte**.
1. Klicken Sie im Abschnitt **Integrierte Endpunkte** auf **Ereignisse**. Das Blatt enthält die folgenden Werte: **Event Hub-kompatibler Endpunkt**, **Event Hub-kompatibler Name**, **Partitionen**, **Aufbewahrungszeit** und **Consumergruppen**.

    ![D2C-Einstellungen][img-eventhubcompatible]

Für das IoT Hub SDK ist der IoT Hub-Endpunktname erforderlich. Er lautet **messages/events** (wie auf dem Blatt **Endpunkte** zu sehen).

Wenn das von Ihnen verwendete SDK einen Wert für **Hostname** oder **Namespace** benötigt, entfernen Sie das Schema aus dem **Event Hub-kompatiblen Endpunkt**. Wenn es sich bei Ihrem Event Hub-kompatiblen Endpunkt beispielsweise um **sb://iothub-ns-myiothub-1234.servicebus.windows.net/** handelt, lautet der **Hostname** **iothub-ns-myiothub-1234.servicebus.windows.net**, und der **Namespace** lautet **iothub-ns-myiothub-1234**.

Sie können in diesem Fall eine SAS-Richtlinie für den gemeinsamen Zugriff mit den **ServiceConnect**-Berechtigungen zur Verbindungsherstellung mit dem angegebenen Event Hub verwenden.

Wenn Sie eine Event Hub-Verbindungszeichenfolge mit den zuvor angegebenen Informationen erstellen müssen, verwenden Sie das folgende Muster:

`Endpoint={Event Hub-compatible endpoint};SharedAccessKeyName={iot hub policy name};SharedAccessKey={iot hub policy key}`

Die SDKs und Integrationen, die Sie mit Event Hub-kompatiblen Endpunkten verwenden können, die IoT Hub verfügbar macht, enthalten die Elemente in der folgenden Liste:

* [Java-Event Hubs-Client](https://github.com/hdinsight/eventhubs-client).
* [Apache Storm-Spout](../hdinsight/hdinsight-storm-develop-csharp-event-hub-topology.md). Sie können sich die [Spoutquelle](https://github.com/apache/storm/tree/master/external/storm-eventhubs) bei GitHub ansehen.
* [Apache Spark-Integration](../hdinsight/hdinsight-apache-spark-eventhub-streaming.md).

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu IoT Hub-Endpunkten finden Sie unter [IoT Hub-Endpunkte][lnk-endpoints].

In den Tutorials [Erste Schritte][lnk-get-started] erfahren Sie, wie Sie D2C-Nachrichten über simulierte Geräte senden und die Nachrichten über den integrierten Endpunkt lesen. Ausführlichere Informationen finden Sie im Tutorial [Verarbeiten von IoT Hub-D2C-Nachrichten mit Routen][lnk-d2c-tutorial].

Informationen zum Weiterleiten von Gerät-zu-Cloud-Nachrichten an benutzerdefinierte Endpunkte finden Sie unter [Verwenden von Nachrichtenrouten und benutzerdefinierten Endpunkte für D2C-Nachrichten][lnk-custom].

[img-eventhubcompatible]: ./media/iot-hub-devguide-messages-read-builtin/eventhubcompatible.png

[lnk-custom]: iot-hub-devguide-messages-read-custom.md
[lnk-get-started]: iot-hub-get-started.md
[lnk-endpoints]: iot-hub-devguide-endpoints.md
[lnk-resource-provider-apis]: https://docs.microsoft.com/rest/api/iothub/iothubresource
[lnk-event-hubs]: http://azure.microsoft.com/documentation/services/event-hubs/
[lnk-management-portal]: https://portal.azure.com
[lnk-d2c-tutorial]: iot-hub-csharp-csharp-process-d2c.md
[lnk-event-hub-partitions]: ../event-hubs/event-hubs-features.md#partitions
[lnk-servicebus-sdk]: https://www.nuget.org/packages/WindowsAzure.ServiceBus
[lnk-eventprocessorhost]: http://blogs.msdn.com/b/servicebus/archive/2015/01/16/event-processor-host-best-practices-part-1.aspx
[lnk-amqp]: https://www.amqp.org/


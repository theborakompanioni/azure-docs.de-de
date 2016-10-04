<properties
    pageTitle="Erstellen eines Service Bus-Namespace mithilfe des Azure-Portals | Microsoft Azure"
    description="Für den Einstieg in Service Bus benötigen Sie einen Namespace. Hier wird erläutert, wie Sie einen Namespace mithilfe des Azure-Portals erstellen."
    services="service-bus"
    documentationCenter=".net"
    authors="jtaubensee"
    manager="timlt"
    editor=""/>

<tags
    ms.service="service-bus"
    ms.devlang="tbd"
    ms.topic="get-started-article"
    ms.tgt_pltfrm="dotnet"
    ms.workload="na"
    ms.date="08/22/2016"
    ms.author="jotaub"/>

# Erstellen eines Service Bus-Namespace mithilfe des Azure-Portals

Ein Namespace ist ein allgemeiner Container für alle Messagingkomponenten. Ein einzelner Namespace kann mehrere Warteschlangen und Themen enthalten, und Namespaces fungieren häufig als Anwendungscontainer. Derzeit gibt es zwei verschiedene Verfahren zum Erstellen von Service Bus-Namespaces.

1.	Azure-Portal (dieser Artikel)

2.	[Ressourcen-Manager-Vorlagen][create-namespace-using-arm]

## Erstellen eines Namespace im Azure-Portal

[AZURE.INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

Glückwunsch! Sie haben nun einen Service Bus Messaging-Namespace erstellt.

## Nächste Schritte

Sehen Sie sich das [GitHub-Repository mit Beispielen](https://github.com/Azure-Samples/azure-servicebus-messaging-samples][github-samples) an, die einige erweiterte Features von Azure Service Bus Messaging veranschaulichen.

[create-namespace-using-arm]: ../service-bus-messaging/service-bus-resource-manager-overview.md
[github-samples]: https://github.com/Azure-Samples/azure-servicebus-messaging-samples

<!---HONumber=AcomDC_0928_2016-->
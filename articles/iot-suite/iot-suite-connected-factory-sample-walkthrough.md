---
title: "Exemplarische Vorgehensweise für die Connected Factory-Lösung der Azure IoT Suite | Microsoft-Dokumentation"
description: "Eine Beschreibung der vorkonfigurierten Connected Factory-Lösung von Azure IoT und ihrer Architektur."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 31fe13af-0482-47be-b4c8-e98e36625855
ms.service: iot-suite
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/27/2017
ms.author: dobett
ms.translationtype: HT
ms.sourcegitcommit: 54774252780bd4c7627681d805f498909f171857
ms.openlocfilehash: 81ecd5771be544e250ea0df31aa274f0850527ad
ms.contentlocale: de-de
ms.lasthandoff: 07/27/2017

---
# <a name="connected-factory-preconfigured-solution-walkthrough"></a>Vorkonfigurierte Connected Factory-Lösung – Exemplarische Vorgehensweise

Die [vorkonfigurierte Connected Factory-Lösung][lnk-preconfigured-solutions] der IoT Suite ist eine Komplettlösung für die Industrie, die:

* eine Verbindung mit simulierten Industriegeräten mit OPC UA-Servern in simulierten Fertigungsstrecken im Werk und mit echten OPC UA-Servergeräten herstellt. Weitere Informationen zu OPC UA finden Sie in den [FAQ][lnk-faq].
* betriebliche KPIs und OEE dieser Geräte und Fertigungsstrecken anzeigt.
* veranschaulicht, wie eine cloudbasierte Anwendung für die Interaktion mit OPC UA-Serversystemen verwendet werden kann.
* das Herstellen einer Verbindung mit eigenen OPC UA-Servergeräten ermöglicht.
* das Durchsuchen und Ändern der OPC UA-Serverdaten ermöglicht.
* in den Azure Time Series Insights-Dienst (TSI) integriert werden kann, um benutzerdefinierte Ansichten der Daten von den OPC UA-Servern bereitzustellen.

Sie können sie als Ausgangspunkt für Ihre Implementierung verwenden und [anpassen][lnk-customize], um Ihre eigenen speziellen Geschäftsanforderungen zu erfüllen.

In diesem Artikel werden einige wichtige Elemente der Connected Factory-Lösung beschrieben, um die Funktionsweise zu verdeutlichen. Dieses Wissen ist für folgende Zwecke hilfreich:

* Behandeln von Problemen in der Lösung
* Planen der Lösungsanpassung zur Erfüllung besonderer Anforderungen
* Entwerfen einer eigenen IoT-Lösung mit Verwendung von Azure-Diensten

## <a name="logical-architecture"></a>Logische Architektur

Das folgende Diagramm beschreibt die logischen Komponenten der vorkonfigurierten Lösung:

![Logische Architektur von Connected Factorys][connected-factory-logical]

## <a name="communication-patterns"></a>Kommunikationsmuster

Die Lösung verwendet die [OPC UA-Pub/Sub-Spezifikation](https://opcfoundation.org/news/opc-foundation-news/opc-foundation-announces-support-of-publish-subscribe-for-opc-ua/), um OPC UA-Telemetriedaten im JSON-Format an IoT Hub zu senden. Zu diesem Zweck verwendet die Lösung das IoT Edge-Modul [OPC Publisher](https://github.com/Azure/iot-edge-opc-publisher).

Die Lösung verfügt auch über einen in eine Webanwendung integrierten OPC UA-Client, um die Verbindungsherstellung mit lokalen OPC UA-Servern zu ermöglichen. Der Client verwendet einen [Reverseproxy](https://wikipedia.org/wiki/Reverse_proxy) und wird von IoT Hub unterstützt, um die Verbindung ohne geöffnete Ports in der lokalen Firewall herstellen zu können. Dieses Kommunikationsmuster wird als [dienstgestützte Kommunikation](https://blogs.msdn.microsoft.com/clemensv/2014/02/09/service-assisted-communication-for-connected-devices/) bezeichnet. Zu diesem Zweck verwendet die Lösung das IoT Edge-Modul [OPC Proxy](https://github.com/Azure/iot-edge-opc-proxy/).


## <a name="simulation"></a>Simulation

Die simulierten Stationen und die simulierten MES (Manufacturing Execution Systems) bilden eine Fertigungsstrecke in einem Werk. Die simulierten Geräte und das OPC Publisher-Modul basieren auf dem [.NET-Standard für OPC UA][lnk-OPC-UA-NET-Standard], der von OPC Foundation veröffentlicht wurde.

OPC Proxy und OPC Publisher werden basierend auf [Azure IoT Edge][lnk-Azure-IoT-Gateway] als Module implementiert. An jede simulierte Fertigungsstrecke ist ein designiertes Gateway angefügt.

Alle in Docker-Containern ausgeführten Simulationskomponenten werden auf einem virtuellen Azure-Computer unter Linux gehostet. Die Simulation ist standardmäßig für die Ausführung von acht simulierten Fertigungsstrecken konfiguriert.

## <a name="simulated-production-line"></a>Simulierte Fertigungsstrecke

Auf einer Fertigungsstrecke werden Teile hergestellt. Sie besteht aus verschiedenen Stationen: Montagestation, Versuchsstand und Packstation.

Die Simulation wird ausgeführt und aktualisiert die Daten, die durch die OPC UA-Knoten verfügbar gemacht werden. Alle Stationen der simulierten Fertigungsstrecke werden vom MES über OPC UA orchestriert.

## <a name="simulated-manufacturing-execution-system"></a>Simuliertes Manufacturing Execution System

Das MES überwacht jede Station der Fertigungsstrecke über OPC UA, um Statusänderungen der Stationen zu ermitteln. Durch Aufrufe von OPC UA-Methoden steuert sie die Stationen und leitet ein Produkt bis zu seiner Fertigstellung von einer Station zur nächsten weiter.

## <a name="gateway-opc-publisher-module"></a>OPC Publisher-Gatewaymodul

Das OPC Publisher-Modul stellt eine Verbindung mit den OPC UA-Servern der Stationen her und abonniert die zu veröffentlichenden OPC-Knoten. Das Modul konvertiert die Knotendaten ins JSON-Format, verschlüsselt sie und sendet sie als OPC UA-Nachrichten für Veröffentlichung/Abonnement an IoT Hub.

Für das OPC Publisher-Modul ist nur ein ausgehender HTTPS-Port (443) erforderlich, und es kann mit der vorhandenen Unternehmensinfrastruktur verwendet werden.

## <a name="gateway-opc-proxy-module"></a>OPC Proxy-Gatewaymodul

Das OPC UA Proxy-Gatewaymodul tunnelt binäre Befehls- und Steuerungsnachrichten für OPC UA und erfordert lediglich einen ausgehenden HTTPS-Port (443). Es kann mit vorhandener Unternehmensinfrastruktur, einschließlich Webproxys, verwendet werden.

Es nutzt IoT Hub-Gerätemethoden zum Übertragen zusammengefasster TCP/IP-Daten auf Anwendungsebene und stellt somit die Endpunktvertrauensstellung, Datenverschlüsselung und Integrität mithilfe von SSL/TLS sicher.

Das über den Proxy selbst weitergeleitete OPC UA-Binärprotokoll nutzt UA-Authentifizierung und -Verschlüsselung.

## <a name="azure-time-series-insights"></a>Azure Time Series Insights

Das OPC Publisher-Gatewaymodul abonniert OPC UA-Serverknoten, um Änderungen der Datenwerte zu ermitteln. Wenn in einem der Knoten eine Datenänderung erkannt wird, sendet dieses Modul Nachrichten an Azure IoT Hub.

IoT Hub stellt Azure TSI eine Ereignisquelle zur Verfügung. TSI speichert Daten auf der Grundlage der an die Nachrichten angefügten Zeitstempel 30 Tage lang. Diese Daten enthalten Folgendes:

* OPC UA-Anwendungs-URI (ApplicationUri)
* OPC UA-Knoten-ID (NodeId)
* Wert des Knotens
* Zeitstempel der Quelle
* OPC UA-Anzeigename (DisplayName)

Derzeit ermöglicht TSI Kunden nicht die Anpassung der Aufbewahrungsdauer für Daten.

TSI führt mithilfe eines Zeitraums (Time.From, Time.To) Abfragen anhand von Knotendaten aus und aggregiert sie nach OPC UA-Anwendungs-URI, OPC UA-Knoten-ID oder OPC UA-Anzeigename.

Zum Abrufen der Daten für die OEE- and KPI-Messgeräte sowie die Zeitreihendiagramme werden Daten anhand der Ereignisanzahl, Summe, des Durchschnittswerts und des Mindest- und Höchstwerts aggregiert.

Die Zeitreihen werden mit einem anderen Prozess erstellt. OEE und KPIs werden aus den Stationsbasisdaten berechnet und für die Topologie (Produktionslinien, Werke, Unternehmen) in der Anwendung hochgerechnet.

Darüber hinaus werden Zeitreihen für OEE- und KPI-Topologie in der App berechnet, wenn ein angezeigter Zeitraum verfügbar ist. Die Tagesansicht wird beispielsweise zu jeder vollen Stunde aktualisiert.

Die Zeitreihenansicht der Knotendaten wird direkt von TSI mit einer Aggregation für den Zeitraum bereitgestellt.

## <a name="iot-hub"></a>IoT Hub
Der [IoT Hub][lnk-IoT Hub] empfängt die vom OPC Publisher-Modul an die Cloud gesendeten Daten und macht sie für den Azure TSI-Dienst verfügbar. 

Zu den Aufgaben des IoT Hub in der Lösung zählen auch:
- Die Verwaltung einer Identitätsregistrierung, die die IDs für alle OPC Publisher- und alle OPC Proxy-Module speichert
- Verwendung als Transportkanal für die bidirektionale Kommunikation des OPC Proxy-Moduls

## <a name="azure-storage"></a>Azure Storage
Die Lösung nutzt Azure-Blobspeicher als Datenspeicher für den virtuellen Computer sowie zum Speichern von Bereitstellungsdaten.

## <a name="web-app"></a>Web-App
Die als Teil der vorkonfigurierten Lösung bereitgestellte Web-App setzt sich aus einem integrierten OPC UA-Client, der Wartungsverarbeitung und der Telemetrievisualisierung zusammen.

## <a name="next-steps"></a>Nächste Schritte

Sie können mit den ersten Schritten mit IoT Suite fortfahren. Lesen Sie dazu die folgenden Artikel:

* [Berechtigungen für die Website „azureiotsuite.com“][lnk-permissions]

[connected-factory-logical]:media/iot-suite-connected-factory-walkthrough/cf-logical-architecture.png

[lnk-preconfigured-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-customize]: iot-suite-guidance-on-customizing-preconfigured-solutions.md
[lnk-IoT Hub]: https://azure.microsoft.com/documentation/services/iot-hub/
[lnk-direct-methods]: ../iot-hub/iot-hub-devguide-direct-methods.md
[lnk-OPC-UA-NET-Standard]:https://github.com/OPCFoundation/UA-.NETStandardLibrary
[lnk-Azure-IoT-Gateway]: https://github.com/azure/iot-edge
[lnk-permissions]: iot-suite-permissions.md
[lnk-faq]: iot-suite-faq.md

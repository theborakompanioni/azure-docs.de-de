---
title: Was ist Traffic Manager? | Microsoft Docs
description: "In diesem Artikel erfahren Sie, worum es sich bei Traffic Manager handelt und ob Traffic Manager die richtige Datenverkehrsrouting-Lösung für Ihre Anwendung ist."
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: 75d5ff9a-f4b9-4b05-af32-700e7bdfea5a
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/11/2016
ms.author: kumud
translationtype: Human Translation
ms.sourcegitcommit: 69b94c93ad3e9c9745af8485766b4237cac0062c
ms.openlocfilehash: 79c14e713fd58d3f69f5978e550a78e8e84d286d

---

# <a name="overview-of-traffic-manager"></a>Traffic Manager – Übersicht

Microsoft Azure Traffic Manager ermöglicht die Verteilung von Benutzerdatenverkehr für Dienstendpunkte in unterschiedlichen Rechenzentren. Zu den von Traffic Manager unterstützten Dienstendpunkten zählen virtuelle Azure-Computer, Web-Apps und Clouddienste. Darüber hinaus kann Traffic Manager auch mit externen, Azure-fremden Endpunkten verwendet werden.

Traffic Manager verwendet das Domain Name System (DNS), um Clientanforderungen auf der Grundlage einer [Datenverkehrsrouting-Methode](traffic-manager-routing-methods.md) und der Integrität der Endpunkte an den optimalen Endpunkt weiterzuleiten. Traffic Manager bietet eine Reihe von Datenverkehrsrouting-Methoden, die verschiedene Anwendungsanforderungen erfüllen und die [Überwachung](traffic-manager-monitoring.md) der Integrität von Endpunkten sowie automatisches Failover ermöglichen. Traffic Manager zeichnet sich durch eine geringe Fehleranfälligkeit aus, selbst wenn es zu einem Ausfall einer ganzen Azure-Region kommt.

## <a name="traffic-manager-benefits"></a>Vorteile von Traffic Manager

Traffic Manager unterstützt Sie bei folgenden Aufgaben:

* **Verbessern der Verfügbarkeit kritischer Anwendungen**

    Mit Traffic Manager können Sie die Verfügbarkeit wichtiger Anwendungen gewährleisten, indem Sie Ihre Endpunkte überwachen und beim Ausfall eines Endpunkts automatisches Failover bereitstellen.

* **Verbessern der Reaktionsfähigkeit hochleistungsfähiger Anwendungen**

    Azure ermöglicht die Ausführung von Cloud-Diensten oder Websites in Datencentern auf der ganzen Welt. Traffic Manager kann die Reaktionsfähigkeit Ihrer Anwendungen verbessern und Endbenutzer an den Endpunkt mit der niedrigsten Netzwerklatenz für den Client weiterleiten.

* **Ausführen von Upgrades oder Dienstwartungsarbeiten ohne Ausfallzeiten**

    Geplante Wartungsarbeiten für Ihre Anwendungen können ohne Ausfallzeiten für Ihre Anwendungen durchgeführt werden. Hierzu wird der Datenverkehr während der Wartung von Traffic Manager an andere Endpunkte weitergeleitet.

* **Kombinieren von lokalen und cloudbasierten Anwendungen**

    Traffic Manager unterstützt externe, Azure-fremde Endpunkte und kann somit in hybriden, cloudbasierten bzw. lokalen Bereitstellungen einschließlich „burst-to-cloud“ (Ausweichen in die Cloud bei Nachfragespitzen), „migrate-to-cloud“ (Migration in die Cloud) und „failover-to-cloud“ (Failover in die Cloud) verwendet werden.

* **Verteilen des Datenverkehrs für große, komplexe Bereitstellungen**

    Datenverkehrsrouting-Methoden können mit [geschachtelten Traffic Manager-Profilen](traffic-manager-nested-profiles.md) zu komplexen und flexiblen Datenverkehrsrouting-Regeln kombiniert werden, die auch den Anforderungen umfangreicher und komplexer Bereitstellungen gerecht werden.

[!INCLUDE [load-balancer-compare-tm-ag-lb-include.md](../../includes/load-balancer-compare-tm-ag-lb-include.md)]

## <a name="next-steps"></a>Nächste Schritte

* Informieren Sie sich ausführlicher über die [Funktionsweise von Traffic Manager](traffic-manager-how-traffic-manager-works.md).
* Erfahren Sie, wie Sie mithilfe der [Traffic Manager-Endpunktüberwachung](traffic-manager-monitoring.md)hoch verfügbare Anwendungen entwickeln.
* Informieren Sie sich über die von Traffic Manager unterstützten [Methoden für das Datenverkehrsrouting](traffic-manager-routing-methods.md) .
* [Erstellen Sie ein Traffic Manager-Profil](traffic-manager-manage-profiles.md).



<!--HONumber=Nov16_HO3-->



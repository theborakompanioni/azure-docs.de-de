---
title: "Einführung in die Datenflussprotokollierung für Netzwerksicherheitsgruppen mit Azure Network Watcher | Microsoft Docs"
description: "Auf dieser Seite wird erläutert, wie NSG-Datenflussprotokolle – ein Feature von Azure Network Watcher – verwendet werden."
services: network-watcher
documentationcenter: na
author: jimdial
manager: timlt
editor: 
ms.assetid: 47d91341-16f1-45ac-85a5-e5a640f5d59e
ms.service: network-watcher
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/22/2017
ms.author: jdial
ms.translationtype: Human Translation
ms.sourcegitcommit: 2f03ba60d81e97c7da9a9fe61ecd419096248763
ms.openlocfilehash: c24c2c4e297497e106691d62be76ad0591bb33de
ms.contentlocale: de-de
ms.lasthandoff: 03/04/2017

---

# <a name="introduction-to-flow-logging-for-network-security-groups"></a>Einführung in die Datenflussprotokollierung für Netzwerksicherheitsgruppen

Datenflussprotokolle für Netzwerksicherheitsgruppen sind ein Network Watcher-Feature, mit dem Sie Informationen zu ein- und ausgehendem IP-Datenverkehr über eine Netzwerksicherheitsgruppe anzeigen können. Diese Datenflussprotokolle sind im JSON-Format geschrieben und zeigen ausgehende und eingehende Datenflüsse pro Regel, die NIC, auf die sich der Datenfluss bezieht, 5-Tupel-Informationen über den Datenfluss (Quell-/Ziel-IP, Quell-/Zielport, Protokoll) und Informationen zu zugelassenem oder verweigertem Datenverkehr.

![Übersicht über Datenflussprotokolle][1]

Da sich Datenflussprotokolle auf Netzwerksicherheitsgruppen beziehen, werden sie nicht wie andere Protokolle angezeigt. Datenflussprotokolle werden nur innerhalb eines Speicherkontos gespeichert. Der Protokollpfad entspricht dabei dem folgenden Beispiel:

```
https://{storageAccountName}.blob.core.windows.net/insights-logs-networksecuritygroupflowevent/resourceId%3D/subscriptions/{subscriptionId}/resourcegroups/{resourceGroupName}/providers/microsoft.network/networksecuritygroups/{nsgName}/{year}/{month}/{day}/PT1H.json
```

Für Datenflussprotokolle gelten die gleichen Aufbewahrungsrichtlinien wie für andere Protokolle. Die Protokolle verfügen über eine Aufbewahrungsrichtlinie, die auf einen Zeitraum zwischen einem und 365 Tagen festgelegt werden kann. Wenn keine Aufbewahrungsrichtlinie festgelegt wurde, werden die Protokolle unbegrenzt aufbewahrt.

## <a name="log-file"></a>Protokolldatei

Datenflussprotokolle besitzen mehrere Eigenschaften. In der folgenden Liste finden Sie die Eigenschaften, die im NSG-Datenflussprotokoll zurückgegeben werden:

* **time:** Zeitpunkt, zu dem das Ereignis ausgelöst wurde
* **systemId:** ID der Netzwerksicherheitsgruppen-Ressource
* **category:** Kategorie des Ereignisses; lautet immer NetworkSecurityGroupFlowEvent
* **resourceid:** Ressourcen-ID der Netzwerksicherheitsgruppe
* **operationName:** immer NetworkSecurityGroupFlowEvents
* **properties:** Sammlung der Eigenschaften des Datenflusses
    * **Version:** Versionsnummer des Ereignisschemas für das Datenflussprotokoll
    * **flows:** Sammlung von Datenflüssen. Diese Eigenschaft verfügt über mehrere Einträge für verschiedene Regeln.
        * **rule:** Regel, für die die Datenflüsse aufgeführt werden
            * **flows:** Sammlung von Datenflüssen
                * **mac:** MAC-Adresse der Netzwerkkarte für den virtuellen Computer, auf dem der Datenfluss erfasst wurde
                * **flowTuples:** Zeichenfolge, die mehrere Eigenschaften für das Datenflusstupel in einem Format mit Trennzeichen enthält
                    * **Zeitstempel:** Wert des Zeitstempels für den Zeitpunkt, zu dem der Datenfluss auftrat, im UNIX EPOCHE-Format
                    * **Quell-IP:** Quell-IP-Adresse
                    * **Ziel-IP:** Ziel-IP-Adresse
                    * **Quellport:** Quellport
                    * **Zielport:** Zielport
                    * **Protokoll:** Protokoll des Datenflusses. Gültige Werte sind **T** für TCP und **U** für UDP.
                    * **Datenfluss:** Richtung des Datenflusses. Gültige Werte sind **I** für eingehende (inbound) und **O** für ausgehende (outbound) Nachrichten.
                    * **Datenverkehr:** gibt an, ob Datenverkehr zugelassen oder verweigert wurde. Gültige Werte sind **A** für zugelassen (allowed) und **D** für verweigert (denied).


Es folgt ein Beispiel für ein Datenflussprotokoll. Wie Sie sehen können, sind mehrere Datensätze vorhanden, die der im vorherigen Abschnitt beschriebenen Eigenschaftenliste entsprechen. 

> [!NOTE]
> Die Werte der flowTuples-Eigenschaft sind eine durch Trennzeichen getrennte Liste.
 
```json
{
    "records": 
    [
        
        {
             "time": "2017-02-16T22:00:32.8950000Z",
             "systemId": "2c002c16-72f3-4dc5-b391-3444c3527434",
             "category": "NetworkSecurityGroupFlowEvent",
             "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-000000000000/RESOURCEGROUPS/FABRIKAMRG/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/FABRIAKMVM1-NSG",
             "operationName": "NetworkSecurityGroupFlowEvents",
             "properties": {"Version":1,"flows":[{"rule":"DefaultRule_DenyAllInBound","flows":[{"mac":"000D3AF8801A","flowTuples":["1487282421,42.119.146.95,10.1.0.4,51529,5358,T,I,D"]}]},{"rule":"UserRule_default-allow-rdp","flows":[{"mac":"000D3AF8801A","flowTuples":["1487282370,163.28.66.17,10.1.0.4,61771,3389,T,I,A","1487282393,5.39.218.34,10.1.0.4,58596,3389,T,I,A","1487282393,91.224.160.154,10.1.0.4,61540,3389,T,I,A","1487282423,13.76.89.229,10.1.0.4,53163,3389,T,I,A"]}]}]}
        }
        ,
        {
             "time": "2017-02-16T22:01:32.8960000Z",
             "systemId": "2c002c16-72f3-4dc5-b391-3444c3527434",
             "category": "NetworkSecurityGroupFlowEvent",
             "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-000000000000/RESOURCEGROUPS/FABRIKAMRG/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/FABRIAKMVM1-NSG",
             "operationName": "NetworkSecurityGroupFlowEvents",
             "properties": {"Version":1,"flows":[{"rule":"DefaultRule_DenyAllInBound","flows":[{"mac":"000D3AF8801A","flowTuples":["1487282481,195.78.210.194,10.1.0.4,53,1732,U,I,D"]}]},{"rule":"UserRule_default-allow-rdp","flows":[{"mac":"000D3AF8801A","flowTuples":["1487282435,61.129.251.68,10.1.0.4,57776,3389,T,I,A","1487282454,84.25.174.170,10.1.0.4,59085,3389,T,I,A","1487282477,77.68.9.50,10.1.0.4,65078,3389,T,I,A"]}]}]}
        }
        ,
        {
             "time": "2017-02-16T22:02:32.9040000Z",
             "systemId": "2c002c16-72f3-4dc5-b391-3444c3527434",
             "category": "NetworkSecurityGroupFlowEvent",
             "resourceId": "/SUBSCRIPTIONS/00000000-0000-0000-0000-000000000000/RESOURCEGROUPS/FABRIKAMRG/PROVIDERS/MICROSOFT.NETWORK/NETWORKSECURITYGROUPS/FABRIAKMVM1-NSG",
             "operationName": "NetworkSecurityGroupFlowEvents",
             "properties": {"Version":1,"flows":[{"rule":"DefaultRule_DenyAllInBound","flows":[{"mac":"000D3AF8801A","flowTuples":["1487282492,175.182.69.29,10.1.0.4,28918,5358,T,I,D","1487282505,71.6.216.55,10.1.0.4,8080,8080,T,I,D"]}]},{"rule":"UserRule_default-allow-rdp","flows":[{"mac":"000D3AF8801A","flowTuples":["1487282512,91.224.160.154,10.1.0.4,59046,3389,T,I,A"]}]}]}
        }
        ,
        ...
```

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie unter [Aktivieren der Datenflussprotokollierung](network-watcher-nsg-flow-logging-portal.md), wie Sie Datenflussprotokolle aktivieren .

Erfahren Sie unter [Protokollanalysen für Netzwerksicherheitsgruppen (NSGs)](../virtual-network/virtual-network-nsg-manage-log.md) etwas über die Protokollierung bei Netzwerksicherheitsgruppen.

Erfahren Sie unter [Überprüfen des Datenverkehrs mit der IP-Datenflussüberprüfung](network-watcher-check-ip-flow-verify-portal.md), wie Sie ermitteln, ob der Datenverkehr auf einem virtuellen Computer zugelassen oder verweigert wird.

<!-- Image references -->
[1]: ./media/network-watcher-nsg-flow-logging-overview/figure1.png



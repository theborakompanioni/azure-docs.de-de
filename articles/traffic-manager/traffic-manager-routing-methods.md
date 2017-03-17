---
title: "Traffic Manager – Methoden für das Datenverkehrsrouting | Microsoft-Dokumente"
description: "Diese Artikel erläutern Ihnen die verschiedenen Methoden für das Datenverkehrsrouting, die von Traffic Manager verwendet werden"
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: db1efbf6-6762-4c7a-ac99-675d4eeb54d0
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/16/2017
ms.author: kumud
translationtype: Human Translation
ms.sourcegitcommit: 11a120338a9f76bfb0a56a70d0c566625bc518b9
ms.openlocfilehash: ee3265032a839b1c35821e60e1143ae772389804
ms.lasthandoff: 02/27/2017

---

# <a name="traffic-manager-traffic-routing-methods"></a>Traffic Manager-Methoden für das Datenverkehrsrouting

Azure Traffic Manager unterstützt drei Methoden für das Datenverkehrsrouting, um zu bestimmen, wie Netzwerkverkehr an die verschiedenen Dienstendpunkte weitergeleitet wird. Traffic Manager wendet die Methode für das Datenverkehrsrouting auf jede empfangene DNS-Abfrage an. Mit der Methode für das Datenverkehrsrouting wird festgelegt, welcher Endpunkt in der DNS-Antwort zurückgegeben wird.

Die Azure Resource Manager-Unterstützung für Traffic Manager verwendet eine andere Terminologie als das klassische Bereitstellungsmodell. Die folgende Tabelle zeigt die Unterschiede zwischen den Resource Manager- und den klassischen Begriffen:

| Resource Manager-Begriff | Klassischer Begriff |
| --- | --- |
| Datenverkehrsrouting-Methode |Lastenausgleichsmethode |
| Prioritätsmethode |Failover-Methode |
| Gewichtete Methode |Roundrobin-Methode |
| Leistungsmethode |Leistungsmethode |

Die Änderung der Terminologie wurde aufgrund von Kundenfeedback eingeführt, um die Klarheit zu verbessern und Missverständnisse zu reduzieren. Es gibt keinen Unterschied in der Funktionalität.

Drei Methoden für das Datenverkehrsrouting sind in Traffic Manager verfügbar:

* **Priorität**: Wählen Sie „Priorität“ aus, wenn Sie einen primären Dienstendpunkt für sämtlichen Datenverkehr verwenden möchten. Stellen Sie Backups bereit, falls der primäre Endpunkt nicht verfügbar ist.
* **Gewichtet** : Wählen Sie „Gewichtet“, wenn Sie Datenverkehr über eine Gruppe von Endpunkten verteilen möchten – gleichmäßig oder gemäß einer von Ihnen definieren Gewichtung.
* **Leistung** : Wählen Sie „Leistung“ aus, wenn sich Ihre Endpunkte an unterschiedlichen geografischen Standorten befinden und Endbenutzer an den „nächstgelegenen“ Endpunkt (im Hinblick auf die geringste Netzwerklatenz) weitergeleitet werden sollen.

Alle Traffic Manager-Profile beinhalten die Überwachung der Endpunktintegrität sowie automatisches Endpunktfailover. Weitere Informationen finden Sie unter [Traffic Manager-Endpunktüberwachung](traffic-manager-monitoring.md). Ein einzelnes Traffic Manager-Profil kann nur eine einzige Methode für das Datenverkehrsrouting verwenden. Sie können jederzeit eine andere Methode für das Datenverkehrsrouting für Ihr Profil auswählen. Änderungen werden innerhalb einer Minute angewendet, und es entstehen keine Ausfallzeiten. Methoden für das Datenverkehrsrouting können durch Verwendung geschachtelter Traffic Manager-Profile kombiniert werden. Durch die Schachtelung können ausgefeilte und flexible Konfigurationen für das Datenverkehrsrouting erstellt werden, um die Anforderungen größerer und komplexer Anwendungen zu erfüllen. Weitere Informationen finden Sie unter [Geschachtelte Traffic Manager-Profile](traffic-manager-nested-profiles.md).

## <a name="priority-traffic-routing-method"></a>Prioritätsmethode für das Datenverkehrsrouting

Wenn Organisationen die Zuverlässigkeit ihrer Dienste gewährleisten möchten, stellen sie häufig einen oder mehrere Sicherungsdienste bereit, für den Fall, dass der primäre Dienst ausfällt. Mithilfe der prioritätsbasierten Methode für das Datenverkehrsrouting können Azure-Kunden dieses Failovermuster problemlos implementieren.
![Prioritätsbasierte Methode für das Datenverkehrsrouting in Azure Traffic Manager][1]

Das Traffic Manager-Profil enthält eine Prioritätenliste mit Dienstendpunkten. Standardmäßig sendet Traffic Manager den gesamten Datenverkehr an den primären Endpunkt (mit der höchsten Priorität). Wenn der primäre Endpunkt nicht verfügbar ist, leitet Traffic Manager den Datenverkehr an den sekundären Endpunkt weiter. Wenn weder der primäre noch der sekundäre Endpunkt verfügbar sind, wird der Datenverkehr an den dritten Endpunkt gesendet usw. Ob ein Endpunkt verfügbar ist, hängt vom konfigurierten Status (aktiviert oder deaktiviert) sowie von der fortlaufenden Endpunktüberwachung ab.

### <a name="configuring-endpoints"></a>Konfigurieren von Endpunkten

Mit Azure Resource Manager wird die Endpunktpriorität mithilfe der für jeden Endpunkt definierten Prioritätseigenschaft explizit konfiguriert. Für diese Eigenschaft wird ein Wert zwischen 1 und 1000 festgelegt. Niedrigere Werte stellen eine höhere Priorität dar. Der gleiche Prioritätswert kann nicht für mehrere Endpunkte konfiguriert werden. Diese Eigenschaft kann optional festgelegt werden. Wird sie nicht festgelegt, wird eine Standardpriorität basierend auf der Endpunktreihenfolge verwendet.

Mit der klassischen Benutzeroberfläche wird die Endpunktpriorität implizit konfiguriert. Dabei hängt die Priorität von der Reihenfolge ab, in der die Endpunkte in der Profildefinition aufgeführt sind.

## <a name="weighted-traffic-routing-method"></a>Gewichtete Methode für das Datenverkehrsrouting
Bei der gewichteten Methode für das Datenverkehrsrouting wird der Datenverkehr gleichmäßig verteilt oder eine vordefinierte Gewichtung verwendet.

![Gewichtete Methode für das Datenverkehrsrouting in Azure Traffic Manager][2]

Bei der gewichteten Methode für das Datenverkehrsrouting wird jedem Endpunkt im Rahmen der Traffic Manager-Profilkonfiguration eine Gewichtung zugewiesen. Die Gewichtung ist eine Ganzzahl zwischen 1 und 1000. Dieser Parameter ist optional. Wenn er nicht angegeben wird, wird die Standardgewichtung&1; verwendet.

Für jede empfangene DNS-Abfrage wählt Traffic Manager einen verfügbaren Endpunkt nach dem Zufallsprinzip aus. Die Wahrscheinlichkeit für die Auswahl eines Endpunkts basiert hierbei auf der Gewichtung, die allen verfügbaren Endpunkten zugewiesen wurde. Die Verwendung der gleichen Gewichtung für alle Endpunkte führt zu einer gleichmäßigen Verteilung des Datenverkehrs. Wenn für bestimmte Endpunkte eine höhere oder niedrigere Gewichtung verwendet wird, werden diese Endpunkte häufiger oder seltener in den DNS-Antworten zurückgegeben.

Durch die gewichtete Methode werden einige nützliche Szenarios ermöglicht:

* Allmähliches Anwendungsupgrade: Weisen Sie einen Prozentwert des Datenverkehrs zu, der an einen neuen Endpunkt weitergeleitet wird, und erhöhen Sie den Datenverkehr dann im Lauf der Zeit allmählich auf 100 %.
* Anwendungsmigration zu Azure: Erstellen Sie ein Profil mit Azure- und externen Endpunkten. Passen Sie die Gewichtung der Endpunkte so an, dass die neuen Endpunkte bevorzugt werden.
* Erweiterung in die Cloud für zusätzliche Kapazität: Erweitern Sie eine lokale Bereitstellung schnell in die Cloud, indem Sie diese hinter einem Traffic Manager-Profil zur Verfügung stellen. Wenn Sie zusätzliche Kapazität in der Cloud benötigen, können Sie weitere Endpunkte hinzufügen oder aktivieren. Geben Sie dann an, welcher Teil des Datenverkehrs an jeden Endpunkt gesendet wird.

Das neue Azure-Portal unterstützt die Konfiguration von gewichtetem Datenverkehrsrouting. Gewichtungen können im klassischen Portal nicht konfiguriert werden. Gewichtungen können auch mithilfe von Resource Manager und den klassischen Versionen von Azure PowerShell, der Befehlszeilenschnittstelle und den REST-APIs konfiguriert werden.

Hinweis: DNS-Antworten werden sowohl von Clients als auch von den rekursiven DNS-Servern zwischengespeichert, die von diesen Clients für ihre DNS-Abfragen verwendet werden. Dieses Zwischenspeichern hat möglicherweise Auswirkungen auf die gewichtete Datenverkehrsverteilungen. Wenn die Anzahl von Clients und rekursiven DNS-Servern hoch ist, funktioniert die Verteilung des Datenverkehrs wie erwartet. Wenn die Anzahl von Clients oder rekursiven DNS-Servern jedoch gering ist, kann diese Zwischenspeicherung die Verteilung des Datenverkehrs massiv verzerren.

Gängige Anwendungsfälle:

* Entwicklungs- und Testumgebungen
* Kommunikation zwischen Anwendungen
* Anwendungen mit einer kleinen Benutzerbasis, die eine gemeinsame rekursive DNS-Infrastruktur nutzen (beispielsweise die Mitarbeiter eines Unternehmens, in dem Verbindungen über einen Proxy hergestellt werden)

Diese Auswirkungen der DNS-Zwischenspeicherung gelten für alle DNS-basierten Systeme für das Datenverkehrsrouting, nicht nur für Azure Traffic Manager. In einigen Fällen lässt sich das Problem möglicherweise durch eine explizite Bereinigung des DNS-Cache umgehen. In anderen Fällen eignet sich eine alternative Methode für das Datenverkehrsrouting besser.

## <a name="performance-traffic-routing-method"></a>Leistungsorientierte Methode für das Datenverkehrsrouting

Die Reaktionsfähigkeit vieler Anwendungen kann verbessert werden, indem Sie Endpunkte an mindestens zwei Standorten auf der ganzen Welt bereitstellen und Datenverkehr an den Standort weiterleiten, der Ihnen am nächsten liegt. Diese Möglichkeit bietet die leistungsorientierte Methode für das Datenverkehrsrouting.

![Leistungsorientierte Methode für das Datenverkehrsrouting in Azure Traffic Manager][3]

Der nächstgelegene Endpunkt ist nicht unbedingt derjenige, der geografisch am nächsten liegt. Vielmehr wird mit dieser Methode für das Datenverkehrsrouting der nächstgelegene Endpunkt durch die Messung der Netzwerklatenz bestimmt. Traffic Manager überwacht anhand einer Internetlatenztabelle die Roundtripzeit zwischen IP-Adressbereichen und den einzelnen Azure-Rechenzentren.

In dieser Internetlatenztabelle sucht der Dienst nach der IP-Quelladresse der eingehenden DNS-Abfrage. Er wählt aus, welcher verfügbare Endpunkt im Azure-Rechenzentrum die geringste Latenz für diesen IP-Adressbereich aufweist, und gibt diesen Endpunkt in der DNS-Antwort zurück.

Wie unter [Funktionsweise von Traffic Manager Works](traffic-manager-how-traffic-manager-works.md) bereits beschrieben, erhält Traffic Manager DNS-Abfragen nicht direkt von Clients. Vielmehr erhält er DNS-Abfragen vom rekursiven DNS-Dienst, für deren Verwendung die Clients konfiguriert sind. Daher handelt es sich bei der IP-Adresse, die zur Ermittlung des nächstgelegenen Endpunkts verwendet wird, nicht um die IP-Adresse eines Clients, sondern um die IP-Adresse des zugehörigen rekursiven DNS-Diensts. In der Praxis lässt sich diese IP-Adresse für diesen Zweck gut verwenden.


Um Änderungen im globalen Internet und neue Azure-Regionen zu berücksichtigen, aktualisiert Traffic Manager die verwendete Internetlatenztabelle regelmäßig. Die Anwendungsleistung ist jedoch je nach Echtzeitschwankung der Auslastung im Internet unterschiedlich. Die leistungsorientierte Methode für das Datenverkehrsrouting überwacht nicht die Auslastung eines bestimmten Dienstendpunkts. Wenn ein Endpunkt jedoch nicht mehr verfügbar ist, wird er von Traffic Manager nicht in DNS-Abfrageantworten aufgenommen.


Beachten Sie Folgendes:

* Wenn Ihr Profil mehrere Endpunkte in der gleichen Azure-Region enthält, wird der an diese Region geleitete Datenverkehr gleichmäßig auf die verfügbaren Endpunkte verteilt. Wenn Sie eine andere Verteilung des Datenverkehrs innerhalb einer Region bevorzugen, können Sie dies mithilfe von [geschachtelten Traffic Manager-Profilen](traffic-manager-nested-profiles.md) erreichen.
* Wenn alle aktivierten Endpunkte in einer bestimmten Azure-Region heruntergestuft wurden, wird der Datenverkehr nicht auf den nächstgelegenen Endpunkt, sondern auf alle anderen verfügbaren Endpunkte verteilt. Mit dieser Logik werden kaskadierende Fehler verhindert, die auftreten können, wenn der nächstgelegene Endpunkt überlastet ist. Wenn Sie eine bevorzugte Failoversequenz definieren möchten, verwenden Sie [geschachtelte Traffic Manager-Profile](traffic-manager-nested-profiles.md).
* Wenn die leistungsorientierte Methode für das Datenverkehrsrouting mit externen oder geschachtelten Endpunkten verwendet wird, müssen Sie den Standort dieser Endpunkte angeben. Wählen Sie die Azure-Region aus, die Ihrer Bereitstellung am nächsten ist. Bei diesen Standorten handelt es sich um die Werte, die von der Internetlatenztabelle unterstützt werden.
* Der Algorithmus, mit dem der Endpunkt ausgewählt wird, ist deterministisch. Wiederholte DNS-Abfragen des gleichen Clients werden an den gleichen Endpunkt geleitet. Clients verwenden unterwegs üblicherweise verschiedene rekursive DNS-Server. Der Client kann daher an einen anderen Endpunkt weitergeleitet werden. Routing kann sich ebenfalls auf die Internetlatenztabelle auswirken. Daher ist mit der leistungsorientierten Methode für das Datenverkehrsrouting nicht gewährleistet, dass ein Client immer an denselben Endpunkt weitergeleitet wird.
* Wenn die Internetlatenztabelle geändert wird, stellen Sie möglicherweise fest, dass einige Clients an einen anderen Endpunkt weitergeleitet werden. Dies reflektiert ein genaueres Routing basierend auf aktuellen Latenzdaten. Diese Updates sind wichtig, um angesichts der kontinuierlichen Entwicklung des Internets die Genauigkeit der leistungsorientierten Methode für das Datenverkehrsrouting sicherzustellen.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie, wie Sie mithilfe der [Traffic Manager endpoint monitoring](traffic-manager-monitoring.md)

Informationen zum [Erstellen eines Traffic Manager-Profils](traffic-manager-manage-profiles.md)

<!--Image references-->
[1]: ./media/traffic-manager-routing-methods/priority.png
[2]: ./media/traffic-manager-routing-methods/weighted.png
[3]: ./media/traffic-manager-routing-methods/performance.png


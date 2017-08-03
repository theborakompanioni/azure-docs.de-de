---
title: Geschachtelte Traffic Manager-Profile | Microsoft-Dokumente
description: "Dieser Artikel erläutert das Feature der geschachtelten Profile von Azure Traffic Manager."
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: f1b112c4-a3b1-496e-90eb-41e235a49609
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/22/2017
ms.author: kumud
ms.translationtype: HT
ms.sourcegitcommit: 74b75232b4b1c14dbb81151cdab5856a1e4da28c
ms.openlocfilehash: 1ac4ec2775ca9f690f5adf4f939908f8cee3f715
ms.contentlocale: de-de
ms.lasthandoff: 07/26/2017

---

# <a name="nested-traffic-manager-profiles"></a>Geschachtelte Traffic Manager-Profile

Traffic Manager umfasst eine Reihe von Methoden für das Datenverkehrsrouting, mit denen Sie steuern können, auf welche Weise Traffic Manager den Endpunkt auswählt, der den Datenverkehr der einzelnen Endbenutzer empfangen soll. Weitere Informationen finden Sie unter [Traffic Manager-Methoden für das Datenverkehrsrouting](traffic-manager-routing-methods.md).

Jedes Traffic Manager-Profil gibt eine einzelne Methode für das Datenverkehrsrouting an. Es gibt jedoch Szenarios, die ein detaillierteres Datenverkehrsrouting erfordern, als es von einem einzelnen Traffic Manager-Profil bereitgestellt werden kann. Sie können Traffic Manager-Profile schachteln, um die Vorteile mehrerer Datenverkehrsrouting-Methoden zu kombinieren. Mit geschachtelten Profilen können Sie das Standardverhalten von Traffic Manager überschreiben, um größere und komplexere Anwendungsbereitstellungen zu unterstützen.

Die folgenden Beispiele zeigen, wie geschachtelte Traffic Manager-Profile in verschiedenen Szenarios verwendet werden.

## <a name="example-1-combining-performance-and-weighted-traffic-routing"></a>Beispiel 1: Kombinieren des leistungsorientierten Datenverkehrsroutings mit dem gewichteten Datenverkehrsrouting

Angenommen, Sie haben eine Anwendung in den folgenden Azure-Regionen bereitgestellt: „USA, Westen“, „Europa, Westen“ und „Asien, Osten“. Sie verwenden die leistungsorientierte Traffic Manager-Methode für das Datenverkehrsrouting, um den Datenverkehr an die Region zu verteilen, die dem Benutzer am nächsten ist.

![Einzelnes Traffic Manager-Profil][4]

Angenommen, Sie möchten ein Update Ihres Diensts testen, bevor Sie es in größerem Rahmen einführen. Sie möchten die gewichtete Methode für das Datenverkehrsrouting verwenden, mit der ein kleiner Prozentsatz des Datenverkehrs an die Testbereitstellung geleitet wird. Sie richten die Testbereitstellung zusammen mit der vorhandenen Produktionsbereitstellung in Europa, Westen, ein.

Die gewichtete und die leistungsorientierte Datenverkehrsrouting-Methode können nicht einem einzigen Profil miteinander kombiniert werden. Um dieses Szenario zu unterstützen, erstellen Sie ein Traffic Manager-Profil mit den beiden Endpunkten in Europa, Westen, und der gewichteten Methode für das Datenverkehrsrouting. Als Nächstes fügen Sie dieses untergeordnete Profil als Endpunkt zum übergeordneten Profil hinzu. Das übergeordnete Profil verwendet weiterhin die leistungsorientierte Methode für das Datenverkehrsrouting und enthält die anderen globalen Bereitstellungen als Endpunkt.

Das folgende Diagramm veranschaulicht dieses Beispiel:

![Geschachtelte Traffic Manager-Profile][2]

In dieser Konfiguration wird der über das übergeordnete Profil geleitete Datenverkehr normal auf alle Regionen verteilt. In Europa, Westen, verteilt das geschachtelte Profil Datenverkehr gemäß den zugeordneten Gewichtungen an die Produktions- und Testendpunkte.

Wenn das übergeordnete Profil die leistungsorientierte Methode für das Datenverkehrsrouting verwendet, muss jeder Endpunkt einem Standort zugeordnet sein. Der Standort wird beim Konfigurieren des Endpunkts zugewiesen. Wählen Sie die Azure-Region aus, die Ihrer Bereitstellung am nächsten ist. Bei den Azure-Regionen handelt es sich um die Standortwerte, die von der Internetlatenztabelle unterstützt werden. Weitere Informationen finden Sie unter [Prioritätsmethode für das Datenverkehrsrouting](traffic-manager-routing-methods.md#performance).

## <a name="example-2-endpoint-monitoring-in-nested-profiles"></a>Beispiel 2: Endpunktüberwachung in geschachtelten Profilen

Traffic Manager überwacht aktiv die Integrität der einzelnen Dienstendpunkte. Wenn ein Endpunkt fehlerhaft ist, leitet Traffic Manager Benutzer an andere Endpunkte, um die Verfügbarkeit Ihres Dienstes zu gewährleisten. Dieses Verhalten für Endpunktüberwachung und -failover gilt für alle Methoden für das Datenverkehrsrouting. Weitere Informationen finden Sie unter [Traffic Manager-Endpunktüberwachung](traffic-manager-monitoring.md). Die Endpunktüberwachung funktioniert bei geschachtelten Profilen anders. Bei geschachtelten Profilen führt das übergeordnete Profil die Integritätsprüfung nicht direkt am untergeordneten Profil durch. Stattdessen wird mit der Integrität der Endpunkte des untergeordneten Profils die Gesamtintegrität des untergeordneten Profils berechnet. Diese Integritätsinformationen werden in der Hierarchie der geschachtelten Profile nach oben weitergegeben. Das übergeordnete Profil verwendet diese aggregierte Integrität, um festzulegen, ob der Datenverkehr an das untergeordnete Profil weitergeleitet wird. Ausführliche Informationen zur Integritätsüberwachung bei geschachtelten Profilen finden Sie unter [Häufig gestellte Fragen](traffic-manager-FAQs.md#traffic-manager-nested-profiles).

Zurück zum vorherigen Beispiel: Angenommen, in der Produktionsbereitstellung in Europa, Westen, tritt ein Fehler auf. Standardmäßig leitet das untergeordnete Profil sämtlichen Datenverkehr an die Testbereitstellung weiter. Wenn in der Testbereitstellung ebenfalls ein Fehler auftritt, legt das übergeordnete Profil fest, dass das untergeordnete Profil keinen Datenverkehr mehr empfangen soll, da alle untergeordneten Endpunkte fehlerhaft sind. Das übergeordnete Profil verteilt den Datenverkehr daraufhin auf die anderen Regionen.

![Failover eines geschachtelten Profils (Standardverhalten)][3]

Möglicherweise sind Sie mit dieser Anordnung zufrieden. Möglicherweise machen Sie sich aber auch Gedanken, dass die Testbereitstellung nicht als Failoverziel für sämtlichen Datenverkehr in Europa, Westen, verwendet werden soll. Sie möchten jedoch unabhängig von der Integrität der Testbereitstellung ein Failover in die anderen Regionen durchführen, wenn in der Produktionsbereitstellung in Europa, Westen, ein Fehler auftritt. Hierzu können Sie bei der Konfiguration des untergeordneten Profils als Endpunkt im übergeordneten Profil den MinChildEndpoints-Parameter angeben. Dieser bestimmt die Mindestanzahl von Endpunkten, die im untergeordneten Profil verfügbar sein müssen. Der Standardwert ist „1“. In diesem Beispiel wird für MinChildEndpoints der Wert 2 festgelegt. Unter diesem Schwellenwert wird das gesamte untergeordnete Profil vom übergeordneten Profil als nicht verfügbar eingestuft, und der Datenverkehr wird an andere Endpunkte geleitet.

Die folgende Abbildung veranschaulicht diese Konfiguration:

![Failover eines geschachtelten Profils mit MinChildEndpoints = 2][4]

> [!NOTE]
> Mit der prioritätsbasierten Methode für das Datenverkehrsrouting wird der gesamte Datenverkehr an einen Endpunkt geleitet. Daher ist es in diesem Fall wenig sinnvoll, MinChildEndpoints für ein untergeordnetes Profil auf einen anderen Wert als 1 festzulegen.

## <a name="example-3-prioritized-failover-regions-in-performance-traffic-routing"></a>Beispiel 3: Priorisierte Failoverregionen im leistungsorientierten Datenverkehrsrouting

Mit dem Standardverhalten der leistungsorientierten Methode für das Datenverkehrsrouting soll eine Überlastung des nächstgelegenen Endpunkts und dadurch eine kaskadierende Serie von Ausfällen vermieden werden. Wenn bei einem Endpunkt ein Fehler auftritt, wird der gesamte Datenverkehr, der an diesen Endpunkt geleitet worden wäre, gleichmäßig auf die anderen Endpunkte in allen Regionen verteilt.

![Leistungsorientierte Methode für das Datenverkehrsrouting mit Standardfailover][5]

Angenommen, Sie möchten für den Datenverkehr nach Europa, Westen, ein Failover in die Region USA, Westen, durchführen und Datenverkehr nur dann an andere Regionen weiterleiten, wenn keiner der beiden Endpunkte verfügbar ist. Diese Lösung können Sie mit einem untergeordneten Profil mit der prioritätsbasierten Methode für das Datenverkehrsrouting erstellen.

![Leistungsorientiertes Datenverkehrsrouting mit bevorzugtem Failover][6]

Da der Endpunkt Europa, Westen, eine höhere Priorität aufweist als der Endpunkt USA, Westen, wird der gesamte Datenverkehr an den Endpunkt Europa, Westen, gesendet, wenn beide Endpunkte online sind. Wenn in Europa, Westen, ein Fehler auftritt, wird der Datenverkehr nach USA, Westen, weitergeleitet. Beim geschachtelten Profil wird der Datenverkehr nur dann an Asien, Osten, weitergeleitet, wenn bei beiden Endpunkten, Europa, Westen, und USA, Westen, ein Fehler auftritt.

Dieses Muster kann für alle Regionen wiederholt werden. Ersetzen Sie alle drei Endpunkte im übergeordneten Profil mit drei untergeordneten Profilen, von denen jedes eine priorisierte Failoversequenz bereitstellt.

## <a name="example-4-controlling-performance-traffic-routing-between-multiple-endpoints-in-the-same-region"></a>Beispiel 4: Steuern des leistungsorientierten Datenverkehrsroutings zwischen mehreren Endpunkten in der gleichen Region

Angenommen, die leistungsorientierte Methode für das Datenverkehrsrouting wird in einem Profil mit mehr als einem Endpunkt in einer bestimmten Region verwendet. Standardmäßig wird für diese Region bestimmter Datenverkehr gleichmäßig auf alle verfügbaren Endpunkte in dieser Region verteilt.

![Leistungsorientiertes Datenverkehrsrouting mit regionsinterner Verteilung des Datenverkehrs (Standardverhalten)][7]

Anstatt mehrere Endpunkte in Europa, Westen, hinzuzufügen, können diese Endpunkte in ein separates untergeordnetes Profil eingeschlossen werden. Das untergeordnete Profil kann dem übergeordneten Profil als einziger Endpunkt in Europa, Westen, hinzugefügt werden. Mit den Einstellungen im untergeordneten Profil kann die Verteilung des Datenverkehrs nach Europa, Westen, gesteuert werden, indem prioritätsbasiertes oder gewichtetes Datenverkehrsrouting innerhalb dieser Region verwendet wird.

![Leistungsorientiertes Datenverkehrsrouting mit benutzerdefinierter regionsinterner Verteilung des Datenverkehrs][8]

## <a name="example-5-per-endpoint-monitoring-settings"></a>Beispiel 5: Überwachungseinstellungen pro Endpunkt

Angenommen, Sie verwenden Traffic Manager, um den Datenverkehr nahtlos zwischen einer älteren lokalen Website und einer neuen cloudbasierten Version zu migrieren, die in Azure gehostet wird. Für die ältere Website möchten Sie den URI der Startseite verwenden, um die Integrität der Website zu überwachen. Für die neue cloudbasierte Version implementieren Sie jedoch eine benutzerdefinierte Überwachungsseite (Pfad „/monitor.aspx“), die zusätzliche Überprüfungen umfasst.

![Traffic Manager-Endpunktüberwachung (Standardverhalten)][9]

Die Überwachungseinstellungen in einem Traffic Manager-Profil gelten für alle Endpunkte innerhalb eines Profils. Bei geschachtelten Profilen können Sie für jede Website ein anderes untergeordnetes Profil verwenden, um unterschiedliche Überwachungseinstellungen zu definieren.

![Traffic Manager-Endpunktüberwachung mit separaten Einstellungen für jeden Endpunkt][10]

## <a name="next-steps"></a>Nächste Schritte

Weitere Informationen zu [Traffic Manager-Profilen](traffic-manager-overview.md)

Informationen zum [Erstellen eines Traffic Manager-Profils](traffic-manager-create-profile.md)

<!--Image references-->
[1]: ./media/traffic-manager-nested-profiles/figure-1.png
[2]: ./media/traffic-manager-nested-profiles/figure-2.png
[3]: ./media/traffic-manager-nested-profiles/figure-3.png
[4]: ./media/traffic-manager-nested-profiles/figure-4.png
[5]: ./media/traffic-manager-nested-profiles/figure-5.png
[6]: ./media/traffic-manager-nested-profiles/figure-6.png
[7]: ./media/traffic-manager-nested-profiles/figure-7.png
[8]: ./media/traffic-manager-nested-profiles/figure-8.png
[9]: ./media/traffic-manager-nested-profiles/figure-9.png
[10]: ./media/traffic-manager-nested-profiles/figure-10.png


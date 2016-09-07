<properties 
   pageTitle="Traffic Manager – Methoden für das Datenverkehrsrouting | Microsoft Azure"
   description="Diese Artikel erläutern Ihnen die verschiedenen Methoden für das Datenverkehrsrouting, die von Traffic Manager verwendet werden"
   services="traffic-manager"
   documentationCenter=""
   authors="sdwheeler"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="traffic-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="05/25/2016"
   ms.author="sewhee" />

# Traffic Manager-Methoden für das Datenverkehrsrouting

Dieser Artikel beschreibt die von Azure Traffic Manager unterstützten Methoden für das Datenverkehrsrouting. Diese werden verwendet, um Endbenutzer an den richtigen Dienstendpunkt weiterzuleiten.

> [AZURE.NOTE] Die ARM-API (Azure Resource Manager) für Traffic Manager verwendet eine andere Terminologie als die ASM-API (Azure Service Manager). Diese Änderung wurde aufgrund von Kundenfeedback eingeführt, um die Klarheit zu verbessern und Missverständnisse zu reduzieren. In diesem Artikel wird die ARM-Terminologie verwendet. Folgende Unterschiede bestehen:

>- In ARM wird der Begriff „Methode für das Datenverkehrsrouting“ verwendet, um den Algorithmus zu beschreiben, mit dem festgelegt wird, an welchen Endpunkt ein bestimmter Endbenutzer zu einer bestimmten Zeit weitergeleitet werden soll. In ASM wird dies als „Lastenausgleichsmethode“ bezeichnet.

>- In ARM bezeichnet der Begriff „gewichtet“ die Methode für das Datenverkehrsrouting, die den Datenverkehr auf alle verfügbaren Endpunkte verteilt, basierend auf der für jeden Endpunkt definierten Gewichtung. In ASM wird dies „Roundrobin“ genannt.
>- In ARM wird der Begriff „Priorität“ verwendet, um die Methode für das Datenverkehrsrouting zu beschreiben, die sämtlichen Datenverkehr an den ersten verfügbaren Endpunkt einer geordneten Liste weiterleitet. In ASM wird dies als „Failover“ bezeichnet.

> In allen Fällen besteht der einzige Unterschied in der Benennung. Es gibt keinen Unterschied in der Funktionalität.


Azure Traffic Manager unterstützt eine Reihe von Algorithmen, um zu bestimmen, wie Endbenutzer an die verschiedenen Endpunkte weitergeleitet werden. Diese werden als Methoden für das Datenverkehrsrouting bezeichnet. Die Methode für das Datenverkehrsrouting wird auf jede empfangene DNS-Abfrage angewendet, um zu bestimmen, welcher Endpunkt in der DNS-Antwort zurückgegeben werden soll.

Drei Methoden für das Datenverkehrsrouting sind in Traffic Manager verfügbar:

- **Priorität**: Wählen Sie „Priorität“ aus, wenn Sie einen primären Dienstendpunkt für sämtlichen Datenverkehr verwenden möchten. Stellen Sie Backups bereit, falls der primäre Endpunkt nicht verfügbar ist. Weitere Informationen finden Sie unter [Prioritätsmethode für das Datenverkehrsrouting](#priority-traffic-routing-method).

- **Gewichtet**: Wählen Sie „Gewichtet“, wenn Sie Datenverkehr über eine Gruppe von Endpunkten verteilen möchten – gleichmäßig oder gemäß einer von Ihnen definieren Gewichtung. Weitere Informationen finden Sie unter [Gewichtete Methode für das Datenverkehrsrouting](#weighted-traffic-routing-method).

- **Leistung**: Wählen Sie „Leistung“ aus, wenn sich Ihre Endpunkte an unterschiedlichen geografischen Standorten befinden und Endbenutzer an den „nächstgelegenen“ Endpunkt (im Hinblick auf die geringste Netzwerklatenz) weitergeleitet werden sollen. Weitere Informationen finden Sie unter [Leistungsorientierte Methode für das Datenverkehrsrouting](#performance-traffic-routing-method).

> [AZURE.NOTE] Alle Traffic Manager-Profile umfassen eine kontinuierliche Überwachung der Endpunktintegrität und automatisches Endpunktfailover. Dies wird für alle Methoden für das Datenverkehrsrouting unterstützt. Weitere Informationen finden Sie unter [Traffic Manager-Endpunktüberwachung](traffic-manager-monitoring.md).

Ein einzelnes Traffic Manager-Profil kann nur eine einzige Methode für das Datenverkehrsrouting verwenden. Sie können jederzeit eine andere Methode für das Datenverkehrsrouting für Ihr Profil auswählen. Änderungen werden innerhalb einer Minute angewendet, und es entstehen keine Ausfallzeiten. Methoden für das Datenverkehrsrouting können durch Verwendung geschachtelter Traffic Manager-Profile kombiniert werden. So können Sie ausgefeilte und flexible Konfigurationen für das Datenverkehrsrouting erstellen, um die Anforderungen größerer und komplexerer Anwendungen zu erfüllen. Weitere Informationen finden Sie unter [Geschachtelte Traffic Manager-Profile](traffic-manager-nested-profiles.md).

## Prioritätsmethode für das Datenverkehrsrouting

Wenn Organisationen die Zuverlässigkeit ihrer Dienste gewährleisten möchten, stellen sie häufig einen oder mehrere Backupdienste bereit, für den Fall, dass der primäre Dienst ausfällt. Mithilfe der prioritätsbasierten Methode für das Datenverkehrsrouting können Azure-Kunden dieses Failovermuster problemlos implementieren.

![Prioritätsbasierte Methode für das Datenverkehrsrouting in Azure Traffic Manager][1]

Das Traffic Manager-Profil wird mit einer priorisierten Liste von Dienstendpunkten konfiguriert. Standardmäßig wird der gesamte Endbenutzerdatenverkehr an den primären Endpunkt (mit der höchsten Priorität) gesendet. Wenn der primäre Endpunkt nicht verfügbar ist (basierend auf dem konfigurierten Endpunktstatus [aktiviert oder nicht aktiviert] und der kontinuierlichen Endpunktüberwachung) werden Benutzer an den zweiten Endpunkt verwiesen. Wenn weder der primäre noch der sekundäre Endpunkt verfügbar sind, wird der Datenverkehr an den dritten Endpunkt gesendet usw.

Die Konfiguration der Endpunktprioritäten wird in den APIs von ARM (sowie dem neuen Azure-Portal) und ASM (sowie dem klassischen Portal) unterschiedlich ausgeführt:

- In den ARM-APIs wird die Endpunktpriorität mithilfe der für jeden Endpunkt definierten Prioritätseigenschaft explizit konfiguriert. Diese Eigenschaft muss einen Wert zwischen 1 und 1000 annehmen können, wobei niedrigere Werte eine höhere Priorität darstellen. Der gleiche Prioritätswert kann nicht für mehrere Endpunkte konfiguriert werden. Die Eigenschaft ist optional. Wird sie nicht angegeben, wird eine Standardpriorität basierend auf der Endpunktreihenfolge verwendet.

- In den ASM-APIs wird die Endpunktpriorität implizit konfiguriert, basierend auf der Reihenfolge, in der die Endpunkte in der Profildefinition aufgeführt sind. Sie können die Failoverreihenfolge auch im klassischen Azure-Portal auf der Konfigurationsseite für das Profil konfigurieren.

## Gewichtete Methode für das Datenverkehrsrouting

Eine gängige Methode, um sowohl hohe Verfügbarkeit als auch maximale Dienstnutzung sicherzustellen, besteht darin, eine Gruppe von Endpunkten bereitzustellen und den Datenverkehr auf alle Endpunkte zu verteilen – entweder gleichmäßig oder anhand einer vordefinierten Gewichtung. Dies wird durch die gewichtete Methode für das Datenverkehrsrouting unterstützt.

![Gewichtete Methode für das Datenverkehrsrouting in Azure Traffic Manager][2]

Bei der gewichteten Methode für das Datenverkehrsrouting wird jedem Endpunkt im Rahmen der Traffic Manager-Profilkonfiguration eine Gewichtung zugewiesen. Jede Gewichtung ist eine Ganzzahl zwischen 1 und 1000. Dieser Parameter ist optional. Wenn er nicht angegeben wird, wird die Standardgewichtung 1 verwendet.
  
Der Endbenutzerdatenverkehr wird auf alle verfügbaren Dienstendpunkte verteilt (basierend auf dem konfigurierten Endpunktstatus [aktiviert oder nicht aktiviert] und der kontinuierlichen Endpunktüberwachung). Für jede empfangene DNS-Abfrage wird nach dem Zufallsprinzip ein verfügbarer Endpunkt ausgewählt. Die Wahrscheinlichkeit basiert hierbei auf der Gewichtung, die diesem und den anderen verfügbaren Endpunkten zugewiesen wurde.

Die Verwendung der gleichen Gewichtung für alle Endpunkte führt zu einer gleichmäßigen Verteilung des Datenverkehrs. Dies eignet sich ideal zur Sicherstellung einer konsistenten Auslastung über eine Gruppe identischer Endpunkte hinweg. Wenn für bestimmte Endpunkte eine höhere (oder niedrigere) Gewichtung verwendet wird, werden diese Endpunkte häufiger (oder seltener) in den DNS-Antworten zurückgegeben und empfangen daher mehr (oder weniger) Datenverkehr. Dadurch wird eine Reihe nützlicher Szenarien ermöglicht:

- Allmähliches Anwendungsupgrade: Weisen Sie einen Prozentwert des Datenverkehrs zu, der an einen neuen Endpunkt weitergeleitet wird, und erhöhen Sie den Datenverkehr dann im Lauf der Zeit allmählich auf 100 %.

- Anwendungsmigration zu Azure: Erstellen Sie ein Profil mit Azure- und externen Endpunkten, und geben Sie die Gewichtung des Datenverkehrs an, der an jeden Endpunkt weitergeleitet wird.

- Erweiterung in die Cloud für zusätzliche Kapazität: Erweitern Sie eine lokale Bereitstellung schnell in die Cloud, indem Sie diese hinter einem Traffic Manager-Profil zur Verfügung stellen. Wenn Sie zusätzliche Kapazität in der Cloud benötigen, können Sie weitere Endpunkte hinzufügen oder aktivieren. Geben Sie dann an, welcher Teil des Datenverkehrs an jeden Endpunkt gesendet wird.

Ein gewichtetes Datenverkehrsrouting kann über das neue Azure-Portal konfiguriert werden, nicht jedoch über das klassische Portal. Dieses Routing kann sowohl über ARM als auch ASM mithilfe von Azure PowerShell, der Azure-Befehlszeilenschnittstelle und der Azure-REST-APIs konfiguriert werden.

Hinweis: DNS-Antworten werden sowohl von Clients als auch von den rekursiven DNS-Servern zwischengespeichert, die von diesen Clients für ihre DNS-Abfragen verwendet werden. Es ist wichtig, die potenziellen Auswirkungen dieser Zwischenspeicherung auf gewichtete Datenverkehrsverteilungen zu verstehen. Wenn die Anzahl von Clients und rekursiven DNS-Servern hoch ist – wie es bei Anwendungen mit Internetzugriff in der Regel der Fall ist –, funktioniert die Verteilung des Datenverkehrs wie erwartet. Wenn die Anzahl von Clients oder rekursiven DNS-Servern jedoch gering ist, kann diese Zwischenspeicherung die Verteilung des Datenverkehrs massiv verzerren. Hier finden Sie einige häufige Szenarien, in denen dies auftreten kann:

- Entwicklungs- und Testumgebungen
- Kommunikation zwischen Anwendungen
- Anwendungen mit einer kleinen Benutzerbasis, die eine gemeinsame rekursive DNS-Infrastruktur nutzen, beispielsweise die Mitarbeiter einer Organisation.

Diese Auswirkungen der DNS-Zwischenspeicherung gelten für alle DNS-basierten Systeme für das Datenverkehrsrouting, nicht nur für Azure Traffic Manager. In einigen Fällen lässt sich das Problem möglicherweise durch eine explizite Bereinigung des DNS-Cache umgehen. In anderen Fällen eignet sich eine alternative Methode für das Datenverkehrsrouting besser.

## Leistungsorientierte Methode für das Datenverkehrsrouting

Die Reaktionsfähigkeit vieler Anwendungen kann verbessert werden, indem Sie Endpunkte an mindestens zwei Standorten auf der ganzen Welt bereitstellen und Endbenutzer an den Standort weiterleiten, der ihnen am nächsten liegt. Dies ist der Zweck der leistungsorientierten Methode für das Datenverkehrsrouting.

![Leistungsorientiere Methode für das Datenverkehrsrouting in Azure Traffic Manager][3]

Um die Reaktionsfähigkeit zu maximieren, ist der nächstgelegene Endpunkt nicht unbedingt derjenige, der geografisch am nächsten liegt. Stattdessen bestimmt diese Methode für das Datenverkehrsrouting, welcher Endpunkt dem Endbenutzer hinsichtlich der Netzwerklatenz am nächsten liegt. Dies wird durch eine Internetlatenztabelle ermittelt, welche die Roundtripzeit zwischen IP-Adressbereichen und den einzelnen Azure-Rechenzentren anzeigt.

Traffic Manager untersucht jede eingehende DNS-Anforderung und ermittelt die IP-Quelladresse dieser Anforderung in der Internetlatenztabelle. So wird die Latenz zwischen dieser IP-Adresse und jedem Azure-Rechenzentrum bestimmt. Traffic Manager wählt aus, welcher der verfügbaren Endpunkte (basierend auf dem konfigurierten Endpunktstatus [aktiviert oder nicht aktiviert] und der kontinuierlichen Endpunktüberwachung) die geringste Latenz aufweist, und gibt diesen Endpunkt in der DNS-Antwort zurück. Daher wird der Endbenutzer an den Endpunkt weitergeleitet, der die geringste Latenz und damit die beste Leistung bietet.

Wie unter [Funktionsweise von Traffic Manager](traffic-manager-how-traffic-manager-works.md) erläutert, empfängt Traffic Manager DNS-Abfragen nicht direkt von Endbenutzern, sondern vom rekursiven DNS-Dienst, für dessen Verwendung die Endbenutzer konfiguriert wurden. Daher handelt es sich bei der IP-Adresse, die zur Ermittlung des nächstgelegenen Endpunkts verwendet wird, nicht um die IP-Adresse eines Endbenutzers, sondern um die IP-Adresse des zugehörigen rekursiven DNS-Diensts. In der Praxis lässt sich diese IP-Adresse für diesen Zweck gut verwenden.

Um Änderungen im globalen Internet sowie die Hinzufügung neuer Azure-Regionen zu berücksichtigen, aktualisiert Traffic Manager die verwendete Internetlatenztabelle regelmäßig. Schwankungen der Leistung oder Auslastung im Internet können dabei jedoch nicht in Echtzeit einberechnet werden.

Die leistungsorientierte Methode für das Datenverkehrsrouting berücksichtigt nicht die Auslastung eines bestimmten Dienstendpunkts. Traffic Manager überwacht jedoch Ihre Endpunkte und schließt diese nicht in die DNS-Abfrageantworten ein, wenn sie nicht verfügbar sind.

Beachten Sie Folgendes:

- Wenn Ihr Profil mehrere Endpunkte in der gleichen Azure-Region enthält, wird der an diese Region geleitete Datenverkehr gleichmäßig auf die verfügbaren Endpunkte verteilt (basierend auf dem konfigurierten Endpunktstatus [aktiviert oder nicht aktiviert] und der kontinuierlichen Endpunktüberwachung). Wenn Sie eine andere Verteilung des Datenverkehrs innerhalb einer Region bevorzugen, können Sie dies mithilfe von [geschachtelten Traffic Manager-Profilen](traffic-manager-nested-profiles.md) erreichen.

- Wenn alle Endpunkte in einer bestimmten Azure-Region heruntergestuft wurden (basierend auf der kontinuierlichen Endpunktüberwachung), wird der an diese Endpunkte gerichtete Datenverkehr auf alle anderen verfügbaren Endpunkte verteilt, die im Profil angegeben sind – nicht auf den oder die nächstgelegenen Endpunkt(e). Auf diese Weise können Sie kaskadierende Fehler vermeiden, die auftreten können, wenn der nächstgelegene Endpunkt überlastet ist. Wenn Sie die Failoversequenz für Endpunkte lieber definieren möchten, können Sie dies in [geschachtelten Traffic Manager-Profilen](traffic-manager-nested-profiles.md) tun.

- Wenn die leistungsorientierte Methode für das Datenverkehrsrouting mit externen oder geschachtelten Endpunkten verwendet wird, müssen Sie den Standort dieser Endpunkte angeben. Wählen Sie die Ihrer Bereitstellung am nächsten gelegene Azure-Region – als Optionen sind die Azure-Regionen verfügbar, da sie die Standorte sind, die von der Internetlatenztabelle unterstützt werden.

- Der Algorithmus, mit dem der für einen bestimmten Benutzer zurückzugebende Endpunkt ausgewählt wird, ist deterministisch – es findet keine zufällige Auswahl statt. Wiederholte DNS-Abfragen des gleichen Clients werden an den gleichen Endpunkt geleitet. Allerdings sollten Sie sich bei der leistungsorientierten Methode für das Datenverkehrsrouting nicht darauf verlassen, dass ein bestimmter Benutzer immer an eine bestimmte Bereitstellung weitergeleitet wird (dies ist z.B. dann erforderlich, wenn die Benutzerdaten für diesen Benutzer nur an einem Ort gespeichert sind). Dies liegt daran, dass Benutzer, wenn sie unterwegs sind, üblicherweise verschiedene rekursive DNS-Server verwenden und daher an einen anderen Endpunkt weitergeleitet werden. Auch Updates der Internetlatenztabelle können sich auswirken.

- Wenn die Internetlatenztabelle aktualisiert wird, stellen Sie möglicherweise fest, dass einige Clients an einen anderen Endpunkt weitergeleitet werden. Dies reflektiert ein genaueres Routing basierend auf aktuellen Latenzdaten, und die Anzahl der davon betroffenen Benutzer dürfte gering sein. Diese Updates sind wichtig, um angesichts der kontinuierlichen Entwicklung des Internets die Genauigkeit der leistungsorientierten Methode für das Datenverkehrsrouting sicherzustellen.


## Nächste Schritte

Erfahren Sie, wie Sie mithilfe der [Traffic Manager-Endpunktüberwachung](traffic-manager-monitoring.md) hoch verfügbare Anwendungen entwickeln.

Informationen zum [Erstellen eines Traffic Manager-Profils](traffic-manager-manage-profiles.md)


<!--Image references-->
[1]: ./media/traffic-manager-routing-methods/priority.png
[2]: ./media/traffic-manager-routing-methods/weighted.png
[3]: ./media/traffic-manager-routing-methods/performance.png

<!---HONumber=AcomDC_0824_2016-->
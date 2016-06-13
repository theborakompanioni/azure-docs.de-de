<properties 
   pageTitle="Geschachtelte Traffic Manager-Profile | Microsoft Azure"
   description="Dieser Artikel erläutert das Feature der geschachtelten Profile von Azure Traffic Manager."
   services="traffic-manager"
   documentationCenter=""
   authors="jtuliani"
   manager="carmonm"
   editor="tysonn" />
<tags 
   ms.service="traffic-manager"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="05/25/2016"
   ms.author="jtuliani" />

# Geschachtelte Traffic Manager-Profile

Traffic Manager umfasst eine Reihe von Methoden für das Datenverkehrsrouting, mit denen Sie steuern können, auf welche Weise Traffic Manager den Endpunkt auswählt, der den Datenverkehr jedes Endbenutzers empfangen soll. Diese werden unter [Traffic Manager-Methoden für das Datenverkehrsrouting](traffic-manager-routing-methods.md) beschrieben und ermöglichen es Traffic Manager, die häufigsten Anforderungen an das Datenverkehrsrouting zu erfüllen.

Jedes Traffic Manager-Profil gibt eine einzelne Methode für das Datenverkehrsrouting an. Es kann jedoch vorkommen, dass komplexere Anwendungen ein detaillierteres Datenverkehrsrouting erfordern, als es von einem einzelnen Traffic Manager-Profil bereitgestellt werden kann.

Um diese komplexen Anwendungen zu unterstützen, können Traffic Manager-Profile kombiniert bzw. *geschachtelt* werden, sodass eine einzelne Anwendung von mehreren Methoden für das Datenverkehrsrouting profitieren kann. Mithilfe geschachtelter Profile können Sie flexiblere und leistungsfähigere Schemas für das Datenverkehrsrouting erstellen, um die Anforderungen größerer, komplexerer Bereitstellungen zu erfüllen.

Darüber können Sie mit geschachtelten Profilen in bestimmten Fällen das Standardverhalten von Traffic Manager außer Kraft setzen – z.B. beim leistungsorientierten Routing des Datenverkehrs innerhalb einer Region oder während eines Failovers.

Dieser Artikel erläutert anhand einer Reihe von Beispielen, wie geschachtelte Traffic Manager-Profile in verschiedenen Szenarien verwendet werden können. Zum Abschluss werden einige häufig gestellte Fragen zu geschachtelten Profilen beantwortet.

## Beispiel 1: Kombinieren des leistungsorientierten Datenverkehrsroutings mit dem gewichteten Datenverkehrsrouting

Angenommen, Ihre Anwendung wurde in mehreren Azure-Region bereitgestellt: USA (Westen), Europa (Westen) und Asien (Osten). Sie verwenden die leistungsorientierte Traffic Manager-Methode für das Datenverkehrsrouting, um den Datenverkehr an die Region zu verteilen, die dem Benutzer am nächsten ist.

![Einzelnes Traffic Manager-Profil][1]

Angenommen, Sie möchten ein Update Ihres Diensts mit einer kleiner Anzahl von Benutzern testen, bevor Sie es in größerem Rahmen einführen. Dafür möchten Sie die gewichtete Methode für das Datenverkehrsrouting verwenden, mit der ein kleiner Prozentsatz des Datenverkehrs an die Testbereitstellung geleitet wird. Mit einem einzigen Profil können Sie das gewichtete und das leistungsbasierte Datenverkehrsrouting nicht miteinander kombinieren. Mit geschachtelten Profilen können Sie beide Methoden verwenden.

So funktioniert es: Angenommen, Sie möchten die neue Bereitstellung in Europa, Westen, testen. Sie richten die Testbereitstellung neben der vorhandenen Produktionsbereitstellung ein und erstellen ein Traffic Manager-Profil nur mit diesen beiden Endpunkten und der gewichteten Methode für das Datenverkehrsrouting. Sie können dieses untergeordnete Profil als Endpunkt zum übergeordneten Profil hinzufügen, das weiterhin die leistungsorientierte Methode für das Datenverkehrsrouting verwendet und auch die anderen globalen Bereitstellungen als Endpunkte enthält.

Das folgende Diagramm veranschaulicht dieses Beispiel:

![Geschachtelte Traffic Manager-Profile][2]

In dieser Anordnung wird der Datenverkehr, der über das übergeordnete Profil weitergeleitet wird, normal auf die Region verteilt. Innerhalb der Region Europa, Westen, wird der Datenverkehr gemäß den zugeordneten Gewichtungen an die Produktions- bzw. die Testbereitstellung weitergeleitet.

Beachten Sie Folgendes: Wenn das übergeordnete Profil die leistungsorientierte Methode für das Datenverkehrsrouting verwendet, muss der Standort jedes Endpunkts bekannt sein. Bei geschachtelten ebenso wie bei externen Endpunkten muss dieser Standort im Rahmen der Endpunktkonfiguration angegeben werden. Wählen Sie die Ihrer Bereitstellung am nächsten gelegene Azure-Region – als Optionen sind die Azure-Regionen verfügbar, da sie die Standorte sind, die von der Internetlatenztabelle unterstützt werden. Weitere Informationen finden Sie unter [Leistungsorientiere Traffic Manager-Methode für das Datenverkehrsrouting](traffic-manager-routing-methods.md#performance-traffic-routing-method).

## Beispiel 2: Endpunktüberwachung in geschachtelten Profilen

Traffic Manager überwacht aktiv die Integrität der einzelnen Dienstendpunkte. Wenn ein Endpunkt als fehlerhaft erkannt wird, leitet Traffic Manager die Benutzer an alternative Endpunkte weiter und stellt so die Gesamtverfügbarkeit Ihres Diensts sicher. Dieses Verhalten für Endpunktüberwachung und -failover gilt für alle Methoden für das Datenverkehrsrouting. Weitere Informationen finden Sie unter [Traffic Manager-Endpunktüberwachung](traffic-manager-monitoring.md).

Bei geschachtelten Profilen gelten einige besondere Regeln für die Endpunktüberwachung. Wenn ein übergeordnetes Profil mit einem untergeordneten Profil als geschachteltem Endpunkt konfiguriert wird, führt das übergeordnete Profil keine direkten Integritätsprüfungen für das untergeordnete Profil aus. Stattdessen wird die Integrität des Endpunkts des untergeordneten Profils verwendet, um die Gesamtintegrität des untergeordneten Profils zu berechnen. Diese Informationen werden in der Hierarchie des geschachtelten Profils weitergegeben, um die Integrität des geschachtelten Endpunkts innerhalb des übergeordneten Profils zu bestimmen. So wird bestimmt, ob das übergeordnete Profil Datenverkehr an das untergeordnete Profil weiterleitet. Details dazu, wie genau die Integrität des geschachtelten Endpunkts im übergeordneten Profil anhand der Integrität des untergeordneten Profils berechnet wird, finden Sie [unten](#faq) in diesem Artikel.

Zurück zu Beispiel 1: Nehmen Sie an, in der Produktionsbereitstellung in Europa, Westen, tritt ein Fehler auf. Standardmäßig leitet das untergeordnete Profil sämtlichen Datenverkehr an die Testbereitstellung weiter. Wenn auch dabei ein Fehler auftritt, legt das übergeordnete Profil Folgendes fest: Da alle untergeordneten Endpunkte fehlerhaft sind, sollte das untergeordnete Profil keinen Datenverkehr empfangen, und für den gesamten Datenverkehr in Europa, Westen, wird ein Failover in die anderen Regionen durchgeführt.

![Failover eines geschachtelten Profils (Standardverhalten)][3]

Möglicherweise sind Sie mit dieser Anordnung zufrieden. Möglicherweise machen Sie sich aber auch Gedanken, dass die Testbereitstellung nicht als Failoverziel für sämtlichen Datenverkehr in Europa, Westen, verwendet werden soll. Sie möchten lieber ein Failover in die anderen Regionen durchführen, wenn in der Produktionsbereitstellung in Europa, Westen, ein Fehler auftritt – *unabhängig* von der Integrität der Testbereitstellung. Dies ist auch möglich: Bei der Konfiguration des untergeordneten Profils als Endpunkt im übergeordneten Profil können Sie den MinChildEndpoints-Parameter angeben. Dieser bestimmt die Mindestanzahl von Endpunkten, die im untergeordneten Profil verfügbar sein müssen. Unterhalb dieses Schwellenwerts (der standardmäßig auf 1 festgelegt ist) betrachtet das übergeordnete Profil das gesamte untergeordnete Profil als nicht verfügbar und leitet den Datenverkehr stattdessen an die anderen Endpunkte im übergeordneten Profil weiter.

Das Beispiel unten veranschaulicht Folgendes: Wenn MinChildEndpoints auf 2 festgelegt ist und bei einer der Bereitstellungen in Europa, Westen, ein Fehler auftritt, legt das übergeordnete Profil fest, dass das untergeordnete Profil keinen Datenverkehr empfangen sollte, und Benutzer werden an die anderen Regionen weitergeleitet.

![Failover eines geschachtelten Profils mit MinChildEndpoints=2][4]

Beachten Sie Folgendes: Wenn das untergeordnete Profil die prioritätsbasierte Methode für das Datenverkehrsrouting verwendet, wird der gesamte Datenverkehr an dieses Profil von einem einzigen Endpunkt empfangen. Daher ist es in diesem Fall wenig sinnvoll, MinChildEndpoints auf einen anderen Wert als 1 festzulegen.

## Beispiel 3: Priorisierte Failoverregionen im leistungsorientierten Datenverkehrsrouting

Wenn ein einziges Profil vorhanden ist, das die leistungsorientierte Methode für das Datenverkehrsrouting verwendet, und in einem Endpunkt (z.B. Europa, Westen) ein Fehler auftritt, wird der gesamte Datenverkehr für diesen Endpunkt stattdessen an die anderen Endpunkte in allen Regionen verteilt. Dies ist das Standardverhalten der leistungsorientierten Methode für das Datenverkehrsrouting, das dazu konzipiert wurde, eine Überlastung des nächstgelegenen Endpunkts und dadurch eine kaskadierende Serie von Ausfällen zu vermeiden.

![Leistungsorientiertes Datenverkehrsrouting mit Standardfailover][5]

Angenommen, Sie möchten für den Datenverkehr nach Europa, Westen, ein Failover in die Region USA, Westen, durchführen und den Datenverkehr nur dann an andere Regionen weiterleiten, wenn beide Endpunkte nicht verfügbar sind. Hierzu können Sie ein untergeordnetes Profil erstellen, das die prioritätsbasierte Methode für das Datenverkehrsrouting verwendet, wie hier gezeigt:

![Leistungsorientiertes Datenverkehrsrouting mit bevorzugtem Failover][6]

Da der Endpunkt Europa, Westen, eine höhere Priorität aufweist als der Endpunkt USA, Westen, wird der gesamte Datenverkehr an den Endpunkt Europa, Westen, gesendet, wenn beide Endpunkte online sind. Wenn in Europa, Westen, ein Fehler auftritt, wird der Datenverkehr nach USA, Westen, weitergeleitet. Nur wenn auch in USA, Westen, ein Fehler auftritt, wird der Datenverkehr aus Europa, Westen, nach Asien, Osten, weitergeleitet.

Sie können dieses Muster für alle Regionen wiederholen und alle drei Endpunkte im übergeordneten Profil durch drei untergeordnete Profile ersetzen, von denen jedes eine priorisierte Failoversequenz bereitstellt.

## Beispiel 4: Steuern des leistungsorientierten Datenverkehrsroutings zwischen mehreren Endpunkten in der gleichen Region

Angenommen, die leistungsorientierte Methode für das Datenverkehrsrouting wird in einem Profil mit mehr als einem Endpunkt in einer bestimmten Region (z.B. USA, Westen) verwendet. Standardmäßig wird für diese Region bestimmter Datenverkehr gleichmäßig auf allen verfügbaren Endpunkte in dieser Region verteilt.

![Leistungsorientiertes Datenverkehrsrouting mit regionsinterner Verteilung des Datenverkehrs (Standardverhalten)][7]

Diese Standardeinstellung kann mithilfe von geschachtelten Traffic Manager-Profilen geändert werden. Anstatt mehrere Endpunkte in USA, Westen, hinzuzufügen, können diese Endpunkte in ein separates untergeordnetes Profil eingeschlossen werden, und das untergeordnete Profil kann dem übergeordneten Profil als einziger Endpunkt in USA, Westen, hinzugefügt werden. Mit den Einstellungen im untergeordneten Profil lässt sich die Verteilung des Datenverkehrs nach USA, Westen, steuern, indem (beispielsweise) prioritätsbasiertes oder gewichtetes Datenverkehrsrouting innerhalb dieser Region verwendet wird.

![Leistungsorientiertes Datenverkehrsrouting mit benutzerdefinierter regionsinterner Verteilung des Datenverkehrs][8]

## Beispiel 5: Überwachungseinstellungen pro Endpunkt

Angenommen, Sie verwenden Traffic Manager, um den Datenverkehr nahtlos zwischen einer älteren lokalen Website und einer neuen cloudbasierten Version zu migrieren, die in Azure gehostet wird. Für die ältere Website möchten Sie die Startseite (Pfad „/“) verwenden, um die Integrität der Website zu überwachen. Für die neue cloudbasierte Version implementieren Sie jedoch ein benutzerdefinierte Überwachungsseite, die zusätzliche Überprüfungen (Pfad „/monitor.aspx“) umfasst.

![Traffic Manager-Endpunktüberwachung (Standardverhalten)][9]

Die Überwachungseinstellungen in einem Traffic Manager-Profil gelten für alle Endpunkte innerhalb des Profils. Aus diesem Grund mussten Sie bisher auf beiden Seiten den gleichen Pfad verwenden. Mit geschachtelten Traffic Manager-Profilen können Sie jetzt ein untergeordnetes Profil pro Website verwenden, um unterschiedliche Überwachungseinstellungen für jede Website zu definieren:

![Traffic Manager-Endpunktüberwachung mit separaten Einstellungen für jeden Endpunkt][10]

## Häufig gestellte Fragen

### Wie konfiguriere ich geschachtelte Profile?

Geschachtelte Traffic Manager-Profile können mit ARM- und ASM-REST-APIs (Azure Resource Manager bzw. Azure Service Management), PowerShell-Cmdlets und plattformübergreifenden Befehlen der Azure-Befehlszeilenschnittstelle konfiguriert werden. Sie werden auch im Azure-Portal unterstützt, nicht jedoch im klassischen Portal.

### Wie viele Schachtelungsebenen werden in Traffic Manager unterstützt?
Sie können Profile bis zu 10 Ebenen schachteln. Schleifen sind nicht zulässig.

### Kann ich andere Endpunkttypen mit geschachtelten untergeordneten Profilen im gleichen Traffic Manager-Profil kombinieren?

Ja. Es gibt keine Einschränkungen hinsichtlich der Kombination von Endpunkten verschiedener Typen innerhalb eines Profils.

### Wie wird das Abrechnungsmodell auf geschachtelte Profile angewendet?

Die Verwendung geschachtelter Profile wirkt sich nicht negativ auf die Preise aus.

Die Abrechnung für Traffic Manager besteht aus zwei Komponenten: Integritätsprüfungen der Endpunkte und Millionen von DNS-Abfragen (ausführliche Informationen finden Sie auf der [Seite mit der Preisübersicht](https://azure.microsoft.com/pricing/details/traffic-manager/)). So wird die Abrechnung auf geschachtelte Profile angewendet:

- Überprüfungen der Endpunktintegrität: Wenn ein untergeordnetes Profil als Endpunkt in einem übergeordneten Profil konfiguriert wird, fallen keine Gebühren an. Endpunkte im untergeordneten Profil, die die zugrunde liegenden Dienste überwachen, werden wie üblich abgerechnet.

- DNS-Abfragen: Jede Abfrage wird nur einmal gezählt. Die Abfrage eines übergeordneten Profils, die einen Endpunkt aus einem untergeordneten Profil zurückgibt, wird nur im übergeordneten Profil berechnet.

### Wirken sich geschachtelte Profile auf die Leistung aus?

Nein, durch geschachtelte Profile entstehen keine Leistungseinbußen.

Die Traffic Manager-Namenserver durchlaufen bei der Verarbeitung jeder DNS-Abfrage intern die Profilhierarchie, sodass die DNS-Abfrage eines übergeordneten Profils eine DNS-Antwort mit einem Endpunkt aus einem untergeordneten Profil erhalten kann.

Daher wird nur ein einziger CNAME-Eintrag verwendet – der gleiche wie bei Verwendung eines einzigen Traffic Manager-Profils. Eine Kette von CNAME-Einträgen – einer für jedes Profil in der Hierarchie – ist **nicht** erforderlich, daher entstehen keine Leistungseinbußen.

### Wie berechnet Traffic Manager die Integrität eines geschachtelten Endpunkts in einem übergeordneten Profil, basierend auf der Integrität des untergeordneten Profils?

Wenn ein übergeordnetes Profil mit einem untergeordneten Profil als geschachteltem Endpunkt konfiguriert wird, führt das übergeordnete Profil keine direkten Integritätsprüfungen für das untergeordnete Profil aus. Stattdessen wird die Integrität des Endpunkts des untergeordneten Profils verwendet, um die Gesamtintegrität des untergeordneten Profils zu berechnen. Diese Informationen werden in der Hierarchie des geschachtelten Profils weitergegeben, um die Integrität des geschachtelten Endpunkts innerhalb des übergeordneten Profils zu bestimmen. So wird bestimmt, ob das übergeordnete Profil Datenverkehr an das untergeordnete Profil weiterleitet.

Die folgende Tabelle beschreibt das Verhalten von Traffic Manager-Integritätsprüfungen für einen geschachtelten Endpunkt in einem übergeordneten Profil, das auf ein untergeordnetes Profil verweist.

|Überwachungsstatus von untergeordneten Profilen|Überwachungsstatus von übergeordneten Endpunkten|Hinweise|
|---|---|---|
|Deaktiviert. Das untergeordnete Profil wurde vom Benutzer deaktiviert.|Beendet|Der Status des übergeordneten Endpunkts ist "Beendet", nicht "Deaktiviert". Der Status "Deaktiviert" zeigt lediglich an, dass Sie den Endpunkt im übergeordneten Profil deaktiviert haben.|
|Heruntergestuft. Der Status mindestens eines Endpunkts wurde heruntergestuft.|Online: Die Anzahl von Onlineendpunkten im untergeordneten Profil ist mindestens gleich dem Wert von MinChildEndpoints. CheckingEndpoint: Die Anzahl von Online- plus CheckingEndpoint-Endpunkten im untergeordneten Profil ist mindestens gleich dem Wert von MinChildEndpoints. Heruntergestuft: andere Fälle.|Der Datenverkehr wird an einen Endpunkt mit dem Status „CheckingEndpoint“ weitergeleitet. Wenn MinChildEndpoints zu hoch festgelegt wurde, wird der Endpunkt immer heruntergestuft.|
|Online. Mindestens ein untergeordneter Profilendpunkt weist den Status „Online“ auf und keiner den Status „Heruntergestuft“.|Siehe oben.||
|CheckingEndpoints. Mindestens ein untergeordneter Profilendpunkt weist „CheckingEndpoint“ auf, keiner ist „Online“ oder „Heruntergestuft“.|Wie oben.||
|Inaktiv. Alle untergeordneten Profilendpunkte sind „Deaktiviert“ oder „Beendet“, oder dies ist ein Profil ohne Endpunkte.|Beendet||


## Nächste Schritte

Weitere Informationen zur [Funktionsweise von Traffic Manager](traffic-manager-how-traffic-manager-works.md)

Informationen zum [Erstellen eines Traffic Manager-Profils](traffic-manager-manage-profiles.md)

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

<!---HONumber=AcomDC_0601_2016-->
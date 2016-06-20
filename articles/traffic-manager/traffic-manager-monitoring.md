<properties 
   pageTitle="Traffic Manager-Endpunktüberwachung und -failover | Microsoft Azure"
   description="In diesem Artikel wird beschrieben, wie Traffic Manager die Endpunktüberwachung und das automatische Endpunktfailover verwendet, um Kunden die Bereitstellung von Anwendungen mit hoher Verfügbarkeit zu ermöglichen."
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
   ms.date="06/04/2016"
   ms.author="jtuliani" />

# Traffic Manager-Endpunktüberwachung und -failover

Azure Traffic Manager umfasst eine integrierte Endpunktüberwachung und ein automatisches Endpunktfailover. Dies ermöglicht Ihnen die Bereitstellung von Anwendungen mit hoher Verfügbarkeit, die auch Endpunktausfälle überstehen, z.B. auch Ausfälle von Azure-Regionen.

Hierzu werden regelmäßig Anforderungen an jeden Endpunkt gesendet und die Antworten überprüft. Wenn ein Endpunkt keine gültige Antwort zurücksendet, wird er als „Heruntergestuft“ markiert und nicht mehr in DNS-Antworten einbezogen. Diese geben stattdessen einen anderen verfügbaren Endpunkt zurück. Endbenutzer-Datenverkehr wird so von ausgefallenen Endpunkten ferngehalten und auf verfügbare Endpunkte umgeleitet.

Die Endpunkt-Integritätsprüfungen werden für Endpunkte mit dem Status „Heruntergestuft“ weiterhin durchgeführt, damit sie automatisch wieder in den Kreislauf eingefügt werden können, nachdem die Integrität wiederhergestellt wurde.

## Konfigurieren der Endpunktüberwachung

Zum Konfigurieren der Endpunktüberwachung müssen Sie die folgenden Einstellungen in Ihrem Traffic Manager-Profil angeben:

- **Protokoll** – Wählen Sie HTTP oder HTTPS aus. Beachten Sie, dass bei der HTTPS-Überwachung nicht überprüft wird, ob Ihr SSL-Zertifikat gültig ist. Es wird nur überprüft, ob es vorhanden ist.
- **Port** – Wählen Sie den Port aus, der für die Anforderung verwendet wird. Die Standardports lauten 80 für HTTP und 443 für HTTPS.
- **Pfad** – Geben Sie den relativen Pfad und den Namen der Webseite oder Datei an, auf die bei der Integritätsprüfung zugegriffen werden soll. Beachten Sie, dass ein Schrägstrich "/" ein gültiger Eintrag für den relativen Pfad ist und voraussetzt, dass sich die Datei im Stammverzeichnis befindet (Standard).

Um die Integrität der einzelnen Endpunkte zu überprüfen, sendet der Traffic Manager eine GET-Anforderung an den Endpunkt, indem er das Protokoll, den Port und den angegebenen relativen Pfad verwendet.

Häufig wird in der Anwendung eine benutzerdefinierte Seite implementiert, z.B. „/health.aspx“, die dann als Pfad für die Traffic Manager-Endpunktüberwachung konfiguriert wird. Innerhalb dieser Seite können Sie alle erforderlichen anwendungsspezifischen Überprüfungen durchführen, z.B. das Überprüfen der Verfügbarkeit einer Back-End-Datenbank, bevor entweder HTTP 200 (falls der Dienst fehlerfrei ist) oder ein anderer Statuscode zurückgegeben wird.

Die Einstellungen für die Endpunktüberwachung werden auf Traffic Manager-Profilebene und nicht pro Endpunkt konfiguriert. Daher werden dieselben Einstellungen verwendet, um die Integrität aller Endpunkte zu überprüfen. Falls Sie für Endpunkte unterschiedliche Überwachungseinstellungen verwenden müssen, können Sie dies mit [geschachtelten Traffic Manager-Profilen](traffic-manager-nested-profiles.md#example-5-per-endpoint-monitoring-settings) erreichen.

## Endpunkt- und Profilstatus

Traffic Manager-Profile und -Endpunkte können vom Benutzer aktiviert und deaktiviert werden. Statusänderungen können auch aufgrund von Endpunkt-Integritätsprüfungen erfolgen. Mit den folgenden Parametern wird das Verhalten ausführlich beschrieben.

### Endpunktstatus

Der Endpunktstatus ist eine vom Benutzer gesteuerte Einstellung, die leicht aktiviert und deaktiviert werden kann. Der Status des zugrunde liegenden Diensts (der ggf. weiter ausgeführt wird) wird hierdurch nicht beeinträchtigt, sondern die Verfügbarkeit dieses Endpunkts wird aus Traffic Manager-Sicht gesteuert. Wenn ein Endpunkt deaktiviert ist, wird seine Integrität nicht überprüft und es werden keine Daten in einer DNS-Antwort zurückgegeben.

### Profilstatus

Der Profilstatus ist eine vom Benutzer gesteuerte Einstellung, bei der das Profil leicht aktiviert und deaktiviert werden kann. Während der Endpunktstatus einen einzelnen Endpunkt betrifft, wirkt sich der Profilstatus auf das gesamte Profil aus, einschließlich aller Endpunkte. Wenn Endpunkte deaktiviert sind, wird ihre Integrität nicht überprüft, und es werden keine Endpunkte in einer DNS-Antwort zurückgegeben (stattdessen gehen bei DNS-Abfragen Antworten vom Typ „NXDOMAIN“ ein).

### Überwachungsstatus von Endpunkten

Der Überwachungsstatus von Endpunkten ist eine vom Traffic Manager generierte Einstellung, die nicht vom Benutzer festgelegt werden kann. Hierbei wird der aktuelle Status des Endpunkts angezeigt, der die fortlaufende Endpunktüberwachung widerspiegelt. Außerdem werden andere Informationen wie der Endpunktstatus angezeigt. In der folgenden Tabelle sind die möglichen Werte des Überwachungsstatus von Endpunkten aufgeführt. (Ausführlichere Informationen zur Berechnung des Überwachungsstatus von Endpunkten finden Sie unter [Geschachtelte Traffic Manager-Profile](traffic-manager-nested-profiles.md).)

|Profilstatus|Endpunktstatus|Überwachungsstatus von Endpunkten (API und Portal)|Hinweise|
|---|---|---|---|
|Deaktiviert|Aktiviert|Inaktiv|Das Profil wurde vom Benutzer deaktiviert. Der Endpunktstatus kann immer noch „Aktiviert“ lauten, aber der Profilstatus hat Vorrang. Endpunkte in Profilen mit dem Status „Deaktiviert“ werden nicht überwacht. In DNS-Antworten wird kein Endpunkt zurückgegeben (stattdessen eine Antwort vom Typ „NXDOMAIN“).|
|&lt;any&gt;|Deaktiviert|Deaktiviert|Der Endpunkt wurde vom Benutzer deaktiviert. Deaktivierte Endpunkte werden nicht überwacht. Sie sind nicht für die Aufnahme in DNS-Antworten verfügbar und erhalten somit auch keinen Datenverkehr.|
|Aktiviert|Aktiviert|Online|Endpunkt wird überwacht und ist fehlerfrei. Er ist für die Aufnahme in DNS-Antworten verfügbar und kann Datenverkehr erhalten.|
|Aktiviert|Aktiviert|Heruntergestuft|Bei Integritätsprüfungen im Rahmen der Endpunktüberwachung werden Fehler erkannt. Der Endpunkt ist nicht für die Aufnahme in DNS-Antworten verfügbar und erhält somit auch keinen Datenverkehr.|
|Aktiviert|Aktiviert|CheckingEndpoint|Endpunkt wird überwacht, die Ergebnisse der ersten Überprüfung wurden aber noch nicht empfangen. Dies ist ein vorübergehender Status, wenn Sie dem Profil gerade einen neuen Endpunkt hinzugefügt haben oder nur einen Endpunkt oder ein Profil aktiviert haben. Endpunkte mit diesem Status sind für die Aufnahme in DNS-Antworten verfügbar und können Datenverkehr erhalten.|
|Aktiviert|Aktiviert|Beendet|Der Clouddienst oder die Web-App, auf den bzw. die der Endpunkt zeigt, wird nicht ausgeführt. Überprüfen Sie die Einstellungen des Clouddiensts oder der Web-App. Beendete Endpunkte werden nicht überwacht. Sie sind nicht für die Aufnahme in DNS-Antworten verfügbar und erhalten somit auch keinen Datenverkehr.|

### Überwachungsstatus von Profilen

Der Überwachungsstatus von Profilen ist das Ergebnis einer Kombination aus dem Überwachungsstatus von Endpunkten für alle Endpunkte im Profil und dem konfigurierten Profilstatus. Die möglichen Werte sind in der folgenden Tabelle beschrieben:

|Profilstatus (wie konfiguriert)|Überwachungsstatus von Endpunkten|Profilüberwachungsstatus (API und Portal)|Hinweise|
|---|---|---|---|
|Deaktiviert|&lt;any&gt; oder ein Profil ohne definierte Endpunkte.|Deaktiviert|Das Profil wurde vom Benutzer deaktiviert.|
|Aktiviert|Der Status mindestens eines Endpunkts lautet "Heruntergestuft".|Heruntergestuft|Das ist eine Kennzeichnung, die Kundenaktion erforderlich macht. Überprüfen Sie die einzelnen Endpunkt-Statuswerte, um zu ermitteln, für welche Endpunkte weitere Aufmerksamkeit erforderlich ist.|
|Aktiviert|Der Status mindestens eines Endpunkts lautet "Online". Keine Endpunkte sind "Heruntergestuft".|Online|Der Dienst akzeptiert Datenverkehr, und es ist keine Kundenaktion erforderlich.|
|Aktiviert|Der Status mindestens eines Endpunkts lautet "CheckingEndpoint". Es sind keine Endpunkte "Online" oder "Heruntergestuft".|CheckingEndpoints|Übergangsstatus. Tritt normalerweise auf, wenn ein Profil gerade erstellt oder aktiviert wurde und die Endpunktintegrität zum ersten Mal überprüft wird.|
|Aktiviert|Der Status aller im Profil definierten Endpunkte ist "Deaktiviert" oder "Beendet" oder im Profil wurden keine Endpunkte definiert.|Inaktiv|Es sind keine Endpunkte aktiv, das Profil ist jedoch weiterhin aktiviert.|

## Endpunktfailover und -failback

Stellen Sie sich ein Szenario vor, bei dem ein Traffic Manager-Endpunkt ausfällt, der bisher fehlerfrei war. Dieser Ausfall wird von Traffic Manager erkannt, und der Endpunkt wird aus der Rotation entfernt. Zu einem späteren Zeitpunkt wird der Endpunkt wiederhergestellt. Dies wird von Traffic Manager ebenfalls erkannt, und der Endpunkt wird wieder in die Rotation eingefügt.

>[AZURE.NOTE] Traffic Manager geht davon aus, dass nur ein Endpunkt online ist, wenn die Antwortnachricht "200 OK" lautet. Wenn einer der folgenden Fälle eintritt, wird dies als fehlgeschlagene Prüfung angesehen:

>- Eine andere Antwort als „200“ wird empfangen (z.B. ein anderer 2xx-Code oder eine 301/302-Umleitung)
>- Anforderung der Clientauthentifizierung
>- Timeout (Timeout-Schwellenwert beträgt 10 Sekunden)
>- Es kann keine Verbindung hergestellt werden

>Weitere Informationen zur Behandlung von Fehler bei Überprüfungen finden Sie unter [Problembehandlung beim Status "Heruntergestuft" in Azure Traffic Manager](traffic-manager-troubleshooting-degraded.md).

Auf der folgenden Zeitachse ist der Ablauf der Schritte ausführlich dargestellt.

![Sequenz für Traffic Manager-Endpunktfailover und -failback](./media/traffic-manager-monitoring/timeline.png)

1. **GET** – Das Traffic Manager-Überwachungssystem führt einen GET-Befehl für den Pfad und die Datei aus, die Sie in den Überwachungseinstellungen angegeben haben. Dies wird für jeden Endpunkt wiederholt.
2. **200 OK** – Das Überwachungssystem erwartet, dass innerhalb von zehn Sekunden eine HTTP 200 OK-Nachricht zurückgegeben wird. Wenn diese Nachricht empfangen wird, wird davon ausgegangen, dass der Dienst verfügbar ist.
3. **30 Sekunden zwischen Überprüfungen** – Die Integritätsprüfung des Endpunkts wird alle 30 Sekunden wiederholt.
4. **Dienst nicht verfügbar** – Der Dienst ist nicht verfügbar. Traffic Manager kann den Status erst bei der nächsten Integritätsprüfung ermitteln.
5. **Zugriffsversuche auf die Überwachungsdatei (4 Versuche)** – Das Überwachungssystem führt einen GET-Befehl aus, erhält aber nicht innerhalb von maximal zehn Sekunden eine Antwort (alternativ hierzu wird ggf. eine andere Antwort als 200 empfangen). Es werden dann drei weitere Versuche in Intervallen von 30 Sekunden durchgeführt. Wenn einer der Versuche erfolgreich ist, wird die Anzahl der Versuche zurückgesetzt.
6. **Markiert heruntergestuft** – Nach dem vierten aufeinanderfolgenden Fehler kennzeichnet das Überwachungssystem den nicht verfügbaren Endpunkt als „Heruntergestuft“. 
7. **Datenverkehr wird an andere Endpunkte umgeleitet** – Die DNS-Namensserver von Traffic Manager werden aktualisiert, und der Endpunkt wird von Traffic Manager nicht mehr als Antwort auf DNS-Abfragen zurückgegeben. Neue Verbindungen werden deshalb an andere verfügbare Endpunkte umgeleitet. Vorherige DNS-Antworten, die diesen Endpunkt enthalten, werden von rekursiven DNS-Servern und DNS-Clients ggf. weiterhin zwischengespeichert. Dies kann dazu führen, dass einige Clients versuchen, eine Verbindung mit diesem Endpunkt herzustellen. Wenn diese Caches ablaufen, führen Clients neue DNS-Abfragen durch und werden an andere Endpunkte umgeleitet. Die Cachedauer wird über die TTL-Einstellung im Traffic Manager-Profil gesteuert, z.B. 30 Sekunden. 
8. **Integritätsprüfungen werden fortgesetzt** – Traffic Manager fährt mit der Integritätsprüfung für den Endpunkt fort, während er sich im Status „Heruntergestuft“ befindet. Auf diese Weise kann erkannt werden, wann die Integrität des Endpunkts wiederhergestellt ist.
9. **Dienst ist wieder online** – Der Dienst ist wieder verfügbar. Der Endpunkt hat in Traffic Manager weiterhin den Status „Heruntergestuft“, bis das Überwachungssystem die nächste Integritätsprüfung durchführt.
10. **Datenverkehr zum Dienst wiederhergestellt** – Traffic Manager sendet einen GET-Befehl und erhält die Antwort „200 OK“, die darauf hinweist, dass die Integrität des Diensts wiederhergestellt wurde. Die Traffic Manager-Namensserver werden erneut aktualisiert und beginnen mit dem Angeben des DNS-Namens des Diensts in DNS-Antworten. Der Datenverkehr kehrt wieder zum Endpunkt zurück, wenn zwischengespeicherte DNS-Antworten anderer Endpunkte ablaufen und vorhandene Verbindungen mit anderen Endpunkten beendet werden.

>[AZURE.NOTE] Da der Traffic Manager auf DNS-Ebene verwendet wird, kann er vorhandene Verbindungen mit Endpunkten nicht beeinflussen. Beim Weiterleiten von Datenverkehr zwischen Endpunkten (entweder durch das Ändern der Profileinstellungen oder beim Failover oder Failback) leitet Traffic Manager neue Verbindungen an verfügbare Endpunkte weiter. Andere Endpunkte erhalten unter Umständen über vorhandene Verbindungen aber weiterhin Datenverkehr, bis diese Sitzungen beendet werden. Um diesen Datenverkehr aus den vorhandenen Verbindungen zu beseitigen, sollte die Sitzungsdauer für Anwendungen verringert werden, die für einen Endpunkt jeweils verwendet wird.

## Heruntergestufte Endpunkte

Wenn ein Endpunkt heruntergestuft wurde, wird er nicht mehr als Antwort auf DNS-Abfragen zurückgegeben (Informationen zu einer Ausnahme dieser Regel finden Sie im Hinweis unten). Stattdessen wird ein alternativer Endpunkt ausgewählt und zurückgegeben. Die Auswahl des alternativen Endpunkts richtet sich nach der konfigurierten Datenverkehr-Routingmethode:

- **Priorität** – Endpunkte bilden eine mit Prioritäten versehene Liste. Der erste verfügbare Endpunkt der Liste wird immer zurückgegeben. Wenn dieser Endpunkt heruntergestuft wird, wird der nächste verfügbare Endpunkt zurückgegeben.
- **Gewichtung** – Alle verfügbaren Endpunkte können zurückgegeben werden. Sie werden basierend auf ihrer zugewiesenen Gewichtung und den Gewichtungen der anderen verfügbaren Endpunkte zufällig ausgewählt. Wenn ein Endpunkt heruntergestuft wird, wird ein anderer Endpunkt aus dem Pool mit den verbleibenden verfügbaren Endpunkten ausgewählt.
- **Leistung** – Der Endpunkt, der dem Endbenutzer am nächsten liegt, wird zurückgegeben (mit Ausnahme von deaktivierten/beendeten Endpunkten). Wenn dieser Endpunkt nicht verfügbar ist, wird der zurückgegebene Endpunkt zufällig aus allen anderen verfügbaren Endpunkten ausgewählt. (Auf diese Weise können Sie kaskadierende Fehler vermeiden, die auftreten können, wenn der nächstgelegene Endpunkt überlastet ist. Sie können alternative Failoverpläne für das Datenverkehrsrouting vom Typ „Leistung“ mit [geschachtelten Traffic Manager-Profilen](traffic-manager-nested-profiles.md#example-4-controlling-performance-traffic-routing-between-multiple-endpoints-in-the-same-region) konfigurieren.)

Weitere Informationen finden Sie unter [Traffic Manager-Methoden für das Datenverkehrsrouting](traffic-manager-routing-methods.md).

>[AZURE.NOTE] Was passiert, wenn alle Traffic Manager-Endpunkte (mit Ausnahme von Endpunkten mit dem Status „Deaktiviert“ oder „Beendet“) die Integritätsprüfung nicht bestehen und als „Heruntergestuft“ markiert werden?

>Der häufigste Grund hierfür ist ein Fehler in der Konfiguration des Diensts (z.B. eine ACL-Blockierung der Traffic Manager-Integritätsprüfungen) oder ein Fehler in der Konfiguration des Traffic Manager-Profils (z.B. ein fehlerhafter Überwachungspfad).

>In diesem Fall wählt Traffic Manager die beste Lösung und *reagiert, als ob alle heruntergestuften Endpunkte „online“ sind*. Dies ist besser als die Alternative, bei der für die DNS-Antwort keine Endpunkte zurückgegeben werden.

>Wenn die Traffic Manager-Integritätsprüfungen nicht richtig konfiguriert sind, führt dieses Verhalten dazu, dass es aufgrund des Datenverkehrsroutings so aussieht, als ob Traffic Manager richtig funktioniert. Allerdings wird das Endpunktfailover nicht durchgeführt, wenn ein Endpunkt ausfällt. Dies wirkt sich auf die Gesamtverfügbarkeit der Anwendung aus. Um sicherzustellen, dass dies nicht geschieht, sollten Sie dafür sorgen, dass für das Profil nicht der Status „Heruntergestuft“, sondern der Status „Online“ angezeigt wird. Der Status „Online“ weist darauf hin, dass die Traffic Manager-Integritätsprüfungen wie gewünscht durchgeführt werden.

Weitere Informationen zur Behandlung von Fehlern bei Integritätsprüfungen finden Sie unter [Problembehandlung beim Status „Heruntergestuft“ in Azure Traffic Manager](traffic-manager-troubleshooting-degraded.md).
 
## Häufig gestellte Fragen

### Ist Traffic Manager selbst gegen Ausfälle von Azure-Regionen resistent?

Da Azure Traffic Manager über integrierte Integritätsprüfungen verfügt und das automatische Failover zwischen Regionen ermöglicht, ist Azure Traffic Manager eine wichtige Komponente zur Bereitstellung hoch verfügbarer Anwendungen in Azure. Dies umfasst auch Anwendungen, die den Ausfall einer gesamten Azure-Region überstehen.

Deshalb ist klar, dass auch Traffic Manager selbst eine außergewöhnlich hohe Verfügbarkeit aufweisen muss, einschließlich der Resilienz gegenüber dem Ausfall einer Region.

Einige Teile von Traffic Manager werden unter Azure ausgeführt, aber sie sind über Regionen verteilt und so konzipiert, dass sie auch einen vollständigen Ausfall jeder Azure-Region überstehen. Diese Resilienz gilt für alle Traffic Manager-Komponenten: DNS-Namensserver, API, Speicherebene und Endpunkt-Überwachungsdienst.

Auch im unwahrscheinlichen Fall des Totalausfalls einer ganzen Azure-Region sollte Traffic Manager daher weiterhin normal funktionieren. Kunden mit Anwendungen, die in mehreren Azure-Regionen bereitgestellt werden, können sich darauf verlassen, dass Traffic Manager Datenverkehr an eine verfügbare Instanz ihrer Anwendung weiterleitet.

### Wie wirkt sich die Wahl des Ressourcengruppenstandorts auf Traffic Manager aus?

Traffic Manager ist ein einzelner globaler Dienst. Es ist kein regionaler Dienst. Die Wahl des Ressourcengruppenstandorts spielt für Traffic Manager-Profile, die in der Ressourcengruppe bereitgestellt werden, keine Rolle.

Für Azure Resource Manager ist es erforderlich, dass für alle Ressourcengruppen ein „Standort“ angegeben wird, mit dem der Standardstandort für in dieser Ressourcengruppe bereitgestellte Ressourcen bestimmt wird. Sie werden nach diesem Standort gefragt, wenn Sie im Rahmen der Erstellung eines Traffic Manager-Profils über das Azure-Portal eine neue Ressourcengruppe erstellen.

Für alle Traffic Manager-Profile wird als Standort „global“ verwendet. Da dies der einzige akzeptierte Wert ist, wird er im Azure-Portal, in PowerShell oder CLI-Umgebungen nicht verfügbar gemacht. Hiermit wird die Standardeinstellung der Ressourcengruppe überschrieben.

### Wie kann ich den aktuellen Integritätsstatus jedes Endpunkts bestimmen?

Der aktuelle Überwachungsstatus jedes Endpunkts und das Gesamtprofil werden im Azure-Verwaltungsportal angezeigt. Diese Informationen sind auch über die [REST-API](https://msdn.microsoft.com/library/azure/mt163667.aspx) von Traffic Manager, [PowerShell-Cmdlets](https://msdn.microsoft.com/library/mt125941.aspx) und die [plattformübergreifende Azure-Befehlszeilenschnittstelle](../xplat-cli-install.md) verfügbar.

Derzeit ist es nicht möglich, Verlaufsinformationen zur Integrität von Endpunkten anzuzeigen, und Sie können auch keine Warnungen über Änderungen der Integrität von Endpunkten konfigurieren.

### Kann ich HTTPS-Endpunkte überwachen?

Ja. Traffic Manager unterstützt Tests per HTTPS. Konfigurieren Sie in der Überwachungskonfiguration einfach „HTTPS“ als Protokoll. Beachten Sie Folgendes:

- Die serverseitigen Zertifikate werden nicht überprüft.
- Serverseitige SNI-Zertifikate werden nicht unterstützt.
- Clientzertifikate werden nicht unterstützt.

### Welcher „Hostheader“ wird für die Integritätsprüfungen für Endpunkte verwendet?

Als „Hostheader“ in HTTP/S-Integritätsprüfungen wird jeweils der DNS-Name verwendet, der einem Endpunkt zugeordnet ist. Er wird als „Ziel“ des Endpunkts verfügbar gemacht, und zwar im Azure-Portal, in [PowerShell-Cmdlets](https://msdn.microsoft.com/library/mt125941.aspx), in der [plattformübergreifenden Azure-Befehlszeilenschnittstelle](../xplat-cli-install.md) und in der [REST-API](https://msdn.microsoft.com/library/azure/mt163667.aspx).

Dieser Wert ist Teil der Endpunktkonfiguration. Der im Hostheader verwendete Wert kann nicht separat von der Zieleigenschaft („target“) angegeben werden.


## Nächste Schritte

Informationen zur [Funktionsweise von Traffic Manager](traffic-manager-how-traffic-manager-works.md)

Informationen zu von Traffic Manager unterstützten [Methoden für das Datenverkehrsrouting](traffic-manager-routing-methods.md)

Informationen zum [Erstellen eines Traffic Manager-Profils](traffic-manager-manage-profiles.md)

[Problembehandlung beim Status „Heruntergestuft“](traffic-manager-troubleshooting-degraded.md) auf einem Traffic Manager-Endpunkt
 

<!---HONumber=AcomDC_0608_2016-->
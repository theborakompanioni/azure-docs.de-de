<properties
   pageTitle="Traffic Manager-Endpunktüberwachung und -failover | Microsoft Azure"
   description="In diesem Artikel wird beschrieben, wie Traffic Manager die Endpunktüberwachung und das automatische Endpunktfailover verwendet, um Azure-Kunden bei der Bereitstellung von Anwendungen mit hoher Verfügbarkeit zu unterstützen."
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
   ms.date="07/01/2016"
   ms.author="jtuliani" />

# Traffic Manager-Endpunktüberwachung und -failover

Azure Traffic Manager umfasst eine integrierte Endpunktüberwachung und ein automatisches Endpunktfailover. Dieses Feature unterstützt Sie bei der Bereitstellung von Anwendungen mit hoher Verfügbarkeit, die auch Endpunktausfälle überstehen, z.B. auch Ausfälle von Azure-Regionen.

Traffic Manager sendet regelmäßig Anforderungen an jeden Endpunkt und überprüft die Antworten. Wenn ein Endpunkt keine gültige Antwort bereitstellt, zeigt Traffic Manager den Status des Endpunkts als „Heruntergestuft“ an. Der Endpunkt wird in DNS-Antworten nicht mehr einbezogen, und diese geben stattdessen einen alternativen, verfügbaren Endpunkt zurück. Auf diese Weise wird der Datenverkehr der Benutzer von ausgefallenen auf verfügbare Endpunkte umgeleitet.

Die Endpunkt-Integritätsprüfungen werden für heruntergestufte Endpunkte weiterhin durchgeführt, damit sie automatisch wieder in den Kreislauf eingefügt werden können, sobald die Integrität wiederhergestellt wurde.

## Konfigurieren der Endpunktüberwachung

Zum Konfigurieren der Endpunktüberwachung müssen Sie die folgenden Einstellungen in Ihrem Traffic Manager-Profil angeben:

- **Protokoll**. Wählen Sie HTTP oder HTTPS aus. Beachten Sie, dass bei der HTTPS-Überwachung nicht überprüft wird, ob Ihr SSL-Zertifikat gültig ist. Es wird nur überprüft, ob es vorhanden ist.
- **Port**. Wählen Sie den Port aus, der für die Anforderung verwendet wird. Die Standardports lauten 80 für HTTP und 443 für HTTPS.
- **Pfad**. Geben Sie den relativen Pfad und den Namen der Webseite oder Datei an, auf die bei der Integritätsprüfung zugegriffen werden soll. Beachten Sie, dass der Schrägstrich („/“) ein gültiger Eintrag für den relativen Pfad ist und bedeutet, dass sich die Datei im Stammverzeichnis (Standardwert) befindet.

Um die Integrität der einzelnen Endpunkte zu überprüfen, sendet Traffic Manager eine GET-Anforderung mit Port, Protokoll und angegebenem relativem Pfad an jeden Endpunkt.

Üblicherweise wird hierfür eine benutzerdefinierte Seite innerhalb der Anwendung implementiert, z.B. „/health.aspx“. Konfigurieren Sie diese Seite als Pfad für die Endpunktüberwachung durch Traffic Manager. Innerhalb dieser Seite können Sie alle erforderlichen anwendungsspezifischen Überprüfungen durchführen, z.B. die Verfügbarkeitsüberprüfung für eine Datenbank, bevor entweder HTTP 200 (falls der Dienst fehlerfrei ist) oder ein anderer Statuscode zurückgegeben wird.

Die Einstellungen für die Endpunktüberwachung werden auf Traffic Manager-Profilebene und nicht pro Endpunkt konfiguriert. Daher werden dieselben Einstellungen verwendet, um die Integrität aller Endpunkte zu überprüfen. Falls Sie für verschiedene Endpunkte unterschiedliche Überwachungseinstellungen verwenden müssen, können Sie dies mit [geschachtelten Traffic Manager-Profilen](traffic-manager-nested-profiles.md#example-5-per-endpoint-monitoring-settings) erreichen.

## Endpunkt- und Profilstatus

Sie können Traffic Manager-Profile und -Endpunkte aktivieren und deaktivieren. Der Endpunktstatus kann sich auch aufgrund von automatisierten Traffic Manager-Einstellungen und -Prozessen ändern. Die folgenden Parameter beschreiben dieses Verhalten.

### Endpunktstatus

Der Endpunktstatus ist eine benutzergesteuerte Einstellung, mit der Sie einen bestimmten Endpunkt ganz einfach aktivieren oder deaktivieren können. Dies wirkt sich nicht auf den Status des zugrunde liegenden Diensts aus (der möglicherweise noch ausgeführt wird). Stattdessen steuert diese Einstellung die Verfügbarkeit eines bestimmten Endpunkts aus Traffic Manager-Sicht. Wenn der Status eines Endpunkts „Deaktiviert“ lautet, überprüft Traffic Manager die Integrität des Endpunkts nicht, und der Endpunkt wird in DNS-Antworten nicht einbezogen.

### Profilstatus

Der Profilstatus ist eine benutzergesteuerte Einstellung. Sie können diese Einstellung nutzen, um ein Profil ganz einfach zu aktivieren oder zu deaktivieren. Während der Endpunktstatus einen einzelnen Endpunkt betrifft, wirkt sich der Profilstatus auf das gesamte Profil aus, einschließlich aller Endpunkte. Für Endpunkte in einem Profil, die den Status „Deaktiviert“ aufweisen, werden keine Integritätsprüfungen durchgeführt, und diese Endpunkte werden in einer DNS-Antwort nicht einbezogen (es wird eine NXDOMAIN-Antwort zurückgegeben).

### Überwachungsstatus von Endpunkten

Der Überwachungsstatus von Endpunkten ist eine Einstellung, die von Traffic Manager generiert wird und den aktuellen Status eines Endpunkts anzeigt. Sie können diese Einstellung nicht manuell ändern. Der Überwachungsstatus von Endpunkten spiegelt die fortlaufende Endpunktüberwachung wider und zeigt weitere Informationen, wie z.B. den Endpunktstatus. In der folgenden Tabelle sind die möglichen Werte des Überwachungsstatus von Endpunkten aufgeführt. (Ausführlichere Informationen zur Berechnung des Überwachungsstatus bei geschachtelten Endpunkten finden Sie unter [Geschachtelte Traffic Manager-Profile](traffic-manager-nested-profiles.md).)

|Profilstatus|Endpunktstatus|Überwachungsstatus von Endpunkten (API und Portal)|Hinweise|
|---|---|---|---|
|Deaktiviert|Aktiviert|Inaktiv|Das Profil wurde vom Benutzer deaktiviert. Der Endpunktstatus kann immer noch „Aktiviert“ lauten, aber der Profilstatus („Deaktiviert“) hat Vorrang. Endpunkte in deaktivierten Profilen werden nicht überwacht und in DNS-Antworten nicht einbezogen, und es wird eine NXDOMAIN-Antwort zurückgegeben.|
|&lt;any&gt;|Deaktiviert|Deaktiviert|Der Endpunkt wurde vom Benutzer deaktiviert. Ein Endpunkt mit dem Status „Deaktiviert“ wird nicht überwacht. Der Endpunkt kann nicht in DNS-Antworten einbezogen werden und daher auch keinen Datenverkehr empfangen.|
|Aktiviert|Aktiviert|Online|Der Endpunkt wird überwacht und ist fehlerfrei. Der Endpunkt kann in DNS-Antworten einbezogen werden und Datenverkehr empfangen.|
|Aktiviert|Aktiviert|Heruntergestuft|Bei Integritätsprüfungen im Rahmen der Endpunktüberwachung werden Fehler erkannt. Der Endpunkt kann nicht in DNS-Antworten einbezogen werden und daher auch keinen Datenverkehr empfangen.|
|Aktiviert|Aktiviert|CheckingEndpoint|Der Endpunkt wird überwacht, die Ergebnisse der ersten Überprüfung wurden aber noch nicht empfangen. Dies ist ein vorübergehender Status, der üblicherweise eintritt, wenn Sie dem Profil gerade einen neuen Endpunkt hinzugefügt haben oder wenn Sie gerade einen Endpunkt oder ein Profil aktiviert haben. Ein Endpunkt in diesem Status kann in DNS-Antworten einbezogen werden und Datenverkehr empfangen.|
|Aktiviert|Aktiviert|Beendet|Der Clouddienst oder die Web-App, auf den bzw. die der Endpunkt zeigt, wird nicht ausgeführt. Überprüfen Sie die Einstellungen des Clouddiensts oder der Web-App. Ein Endpunkt mit dem Status „Beendet“ wird nicht überwacht. Der Endpunkt kann nicht in DNS-Antworten einbezogen werden und daher auch keinen Datenverkehr empfangen.|

### Überwachungsstatus von Profilen

Der Überwachungsstatus von Profilen ist das Ergebnis einer Kombination aus den Überwachungsstatuswerten von Endpunkten für alle Endpunkte im Profil und dem konfigurierten Profilstatus. Die möglichen Werte werden in der folgenden Tabelle beschrieben.

|Profilstatus (wie konfiguriert)|Überwachungsstatus von Endpunkten|Profilüberwachungsstatus (API und Portal)|Hinweise|
|---|---|---|---|
|Deaktiviert|&lt;any&gt; oder ein Profil ohne definierte Endpunkte.|Deaktiviert|Das Profil wurde vom Benutzer deaktiviert.|
|Aktiviert|Der Status mindestens eines Endpunkts lautet „Heruntergestuft“.|Heruntergestuft|Das ist eine Kennzeichnung, die Kundenaktion erforderlich macht. Überprüfen Sie die einzelnen Endpunkt-Statuswerte, um zu ermitteln, für welche Endpunkte weitere Aufmerksamkeit erforderlich ist.|
|Aktiviert|Der Status mindestens eines Endpunkts lautet „Online“. Kein Endpunkt weist den Status „Heruntergestuft“ auf.|Online|Der Dienst akzeptiert Datenverkehr, und es ist keine Kundenaktion erforderlich.|
|Aktiviert|Der Status mindestens eines Endpunkts lautet „CheckingEndpoint“. Es befinden sich keine Endpunkte in den Status „Online“ oder „Heruntergestuft“.|CheckingEndpoints|Übergangsstatus. Tritt normalerweise auf, wenn ein Profil gerade erstellt oder aktiviert wurde und die Endpunktintegrität zum ersten Mal überprüft wird.|
|Aktiviert|Der Status aller im Profil definierten Endpunkte ist „Deaktiviert“ oder „Beendet“, oder im Profil wurden keine Endpunkte definiert.|Inaktiv|Es sind keine Endpunkte aktiv, das Profil ist jedoch weiterhin aktiviert.|

## Endpunktfailover und -failback

Stellen Sie sich ein Szenario vor, in dem ein zuvor fehlerfreier Endpunkt ausfällt. Traffic Manager erkennt den Fehler, und der Endpunkt wird aus dem Kreislauf genommen. Später wird der Endpunkt wiederhergestellt. Traffic Manager erkennt die Wiederherstellung, und der Endpunkt wird wieder in den Kreislauf aufgenommen.

>[AZURE.NOTE] Traffic Manager betrachtet einen Endpunkt nur dann als online, wenn die Antwortnachricht „200 OK“ lautet. Wenn einer der folgenden Fälle eintritt, betrachtet Traffic Manager dies als Fehler der Endpunkt-Integritätsprüfung:

>- Eine andere Antwort als „200“ wird empfangen (z.B. ein anderer 2xx-Code oder eine 301/302-Umleitung)
>- Anforderung der Clientauthentifizierung
>- Timeout (Timeout-Schwellenwert beträgt 10 Sekunden)
>- Es kann keine Verbindung hergestellt werden

>Weitere Informationen zur Behandlung von Fehlern bei Integritätsprüfungen finden Sie unter [Problembehandlung beim Status „Heruntergestuft“ in Azure Traffic Manager](traffic-manager-troubleshooting-degraded.md).

Auf der folgenden Zeitachse ist der Ablauf der Schritte ausführlich dargestellt.

![Sequenz für Traffic Manager-Endpunktfailover und -failback](./media/traffic-manager-monitoring/timeline.png)

1. **GET**. Das Traffic Manager-Überwachungssystem führt eine GET-Anforderung für den Pfad und die Datei aus, die Sie in den Überwachungseinstellungen angegeben haben. Dies wird für jeden Endpunkt wiederholt.
2. **200 OK**. Das Überwachungssystem erwartet, dass innerhalb von zehn Sekunden eine HTTP 200 OK-Nachricht zurückgegeben wird. Wenn diese Nachricht empfangen wird, wird der Dienst als verfügbar betrachtet.
3. **30 Sekunden zwischen Überprüfungen**. Die Endpunkt-Integritätsprüfung wird alle 30 Sekunden wiederholt.
4. **Dienst nicht verfügbar**. Der Dienst ist nicht verfügbar. Traffic Manager kann den Status erst bei der nächsten Integritätsprüfung ermitteln.
5. **Zugriff auf die Überwachungsdatei wird versucht (vier Versuche)**. Das Überwachungssystem führt eine GET-Anforderung aus, erhält aber nicht innerhalb des Zeitlimits von zehn Sekunden eine Antwort (alternativ hierzu wird ggf. eine andere Antwort als 200 empfangen). Es werden dann in Abständen von jeweils 30 Sekunden drei weitere Versuche durchgeführt. Wenn einer der Versuche erfolgreich ist, wird die Anzahl der Versuche zurückgesetzt.
6. **Status auf „Heruntergestuft“ festgelegt**. Nach dem vierten erfolglosen Versuch in Folge markiert das Überwachungssystem den Status des nicht verfügbaren Endpunkts als „Heruntergestuft“.
7. **Datenverkehr wird an andere Endpunkte umgeleitet**. Die DNS-Namenserver in Traffic Manager werden aktualisiert, und Traffic Manager gibt den Endpunkt in Antworten auf DNS-Abfragen nicht mehr zurück. Neue Verbindungen werden an andere, verfügbare Endpunkte umgeleitet. Vorherige DNS-Antworten, die diesen Endpunkt enthalten, werden von rekursiven DNS-Servern und DNS-Clients möglicherweise weiterhin zwischengespeichert. Dies kann dazu führen, dass einige Clients versuchen, eine Verbindung mit diesem Endpunkt herzustellen. Wenn diese Caches ablaufen, führen Clients neue DNS-Abfragen durch und werden an andere Endpunkte umgeleitet. Die Cachedauer wird über die TTL-Einstellung im Traffic Manager-Profil gesteuert, z.B. 30 Sekunden.
8. **Integritätsprüfungen werden fortgesetzt**. Traffic Manager fährt mit der Integritätsprüfung für den Endpunkt fort, während dieser sich im Status „Heruntergestuft“ befindet. Auf diese Weise kann erkannt werden, wann die Integrität des Endpunkts wiederhergestellt ist.
9. **Service wird wieder online geschaltet**. Der Dienst wird verfügbar. Der Endpunkt hat in Traffic Manager weiterhin den Status „Heruntergestuft“, bis das Überwachungssystem die nächste Integritätsprüfung durchführt.
10. **Datenverkehr an den Dienst wird wieder aufgenommen**. Traffic Manager sendet eine GET-Anforderung und empfängt eine 200 OK-Statusantwort. Dies weist darauf hin, dass sich der Dienst wieder in einem fehlerfreien Zustand befindet. Die Traffic Manager-Namensserver werden aktualisiert und beginnen damit, den DNS-Namen des Diensts in DNS-Antworten wieder anzugeben. Der Datenverkehr wird wieder an den Endpunkt geleitet, wenn zwischengespeicherte DNS-Antworten mit Angabe anderer Endpunkte ablaufen und vorhandene Verbindungen mit anderen Endpunkten beendet werden.

>[AZURE.NOTE] Da Traffic Manager auf DNS-Ebene arbeitet, kann er vorhandene Verbindungen mit Endpunkten nicht beeinflussen. Beim Weiterleiten von Datenverkehr zwischen Endpunkten (entweder durch geänderte Profileinstellungen oder beim Failover oder Failback) leitet Traffic Manager neue Verbindungen an verfügbare Endpunkte weiter. Andere Endpunkte erhalten jedoch möglicherweise über vorhandene Verbindungen weiterhin Datenverkehr, bis diese Sitzungen beendet werden. Um diesen Datenverkehr aus den vorhandenen Verbindungen zu beseitigen, sollte die Sitzungsdauer für Anwendungen verringert werden, die für einen Endpunkt jeweils verwendet wird.

## Heruntergestufte Endpunkte

Wenn ein Endpunkt den Status „Heruntergestuft“ aufweist, wird er nicht mehr in Antworten auf DNS-Abfragen zurückgegeben (Informationen zu einer Ausnahme dieser Regel finden Sie im Hinweis am Ende dieses Abschnitts). Stattdessen wird ein alternativer Endpunkt ausgewählt und zurückgegeben. Die Auswahl des alternativen Endpunkts richtet sich nach der konfigurierten Datenverkehr-Routingmethode:

- **Priorität**. Endpunkte bilden eine mit Prioritäten versehene Liste. Der erste verfügbare Endpunkt der Liste wird immer zurückgegeben. Wenn ein Endpunkt sich im Status „Heruntergestuft“ befindet, wird der nächste verfügbare Endpunkt zurückgegeben.
- **Gewichtet**. Alle verfügbaren Endpunkte können zurückgegeben werden. Sie werden basierend auf der ihnen zugewiesenen Gewichtung und den Gewichtungen der anderen verfügbaren Endpunkte zufällig ausgewählt. Wenn ein Endpunkt sich im Status „Heruntergestuft“ befindet, wird ein anderer Endpunkt aus dem Pool mit den verbleibenden verfügbaren Endpunkten ausgewählt.
- **Leistung**. Der Endpunkt, der dem Endbenutzer am nächsten liegt, wird zurückgegeben (mit Ausnahme von Endpunkten mit dem Status „Deaktiviert“ oder „Beendet“). Wenn dieser Endpunkt nicht verfügbar ist, wird der zurückgegebene Endpunkt zufällig aus allen anderen verfügbaren Endpunkten ausgewählt. (Auf diese Weise können Sie kaskadierende Fehler vermeiden, die auftreten können, wenn der nächstgelegene Endpunkt überlastet ist. Sie können alternative Failoverpläne für das leistungsorientierte Datenverkehrsrouting mit [geschachtelten Traffic Manager-Profilen](traffic-manager-nested-profiles.md#example-4-controlling-performance-traffic-routing-between-multiple-endpoints-in-the-same-region) konfigurieren.)

Weitere Informationen finden Sie unter [Traffic Manager-Methoden für das Datenverkehrsrouting](traffic-manager-routing-methods.md).

>[AZURE.NOTE] Was passiert, wenn alle Traffic Manager-Endpunkte (mit Ausnahme von Endpunkten mit dem Status „Deaktiviert“ oder „Beendet“) die Integritätsprüfung nicht bestehen und den Status „Heruntergestuft“ anzeigen?

>Der häufigste Grund hierfür ist ein Fehler in der Konfiguration des Diensts (z.B. eine ACL-Blockierung [Access Control List, Zugriffssteuerungsliste] der Traffic Manager-Integritätsprüfungen) oder ein Fehler in der Konfiguration des Traffic Manager-Profils (z.B. ein fehlerhafter Überwachungspfad).

>In diesem Fall wählt Traffic Manager die beste Lösung und *antwortet so, als befänden sich alle heruntergestuften Endpunkte im Status „Online“*. Dies ist besser als die Alternative, bei der für die DNS-Antwort keinerlei Endpunkte zurückgegeben würden.

>Wenn die Traffic Manager-Integritätsprüfungen nicht richtig konfiguriert sind, führt dieses Verhalten dazu, dass es aufgrund des Datenverkehrsroutings so aussieht, *als ob* Traffic Manager richtig funktioniert. Allerdings wird in diesem Fall kein Endpunktfailover durchgeführt, wenn ein Endpunkt ausfällt. Dies wirkt sich auf die Gesamtverfügbarkeit der Anwendung aus. Um sicherzustellen, dass dies nicht geschieht, stellen Sie sicher, dass für das Profil nicht der Status „Heruntergestuft“, sondern der Status „Online“ angezeigt wird. Der Status „Online“ weist darauf hin, dass die Traffic Manager-Integritätsprüfungen wie gewünscht durchgeführt werden.

Weitere Informationen zur Behandlung von Fehlern bei Integritätsprüfungen finden Sie unter [Problembehandlung beim Status „Heruntergestuft“ in Azure Traffic Manager](traffic-manager-troubleshooting-degraded.md).

## Häufig gestellte Fragen

### Ist Traffic Manager selbst gegen Ausfälle von Azure-Regionen resistent?

Da Traffic Manager integrierte Integritätsprüfungen durchführt und das automatische Failover zwischen Regionen ermöglicht, ist er eine wichtige Komponente bei der Bereitstellung hoch verfügbarer Anwendungen in Azure. Dies umfasst auch Anwendungen, die den Ausfall einer gesamten Azure-Region überstehen.

Aus diesem Grund muss auch Traffic Manager selbst eine außergewöhnlich hohe Verfügbarkeit aufweisen, einschließlich Resilienz gegenüber dem Ausfall einer Region.

Einige Komponenten von Traffic Manager werden zwar in Azure ausgeführt, sind aber über Regionen verteilt und so konzipiert, dass sie auch den vollständigen Ausfall einer Azure-Region überstehen. Diese Resilienz gilt für alle Traffic Manager-Komponenten: DNS-Namensserver, API, Speicherebene und Endpunkt-Überwachungsdienst.

Auch im unwahrscheinlichen Fall des Totalausfalls einer ganzen Azure-Region sollte Traffic Manager daher weiterhin normal funktionieren. Kunden mit Anwendungen, die in mehreren Azure-Regionen bereitgestellt sind, können sich darauf verlassen, dass Traffic Manager den Datenverkehr an eine verfügbare Instanz ihrer Anwendung weiterleitet.

### Wie wirkt sich die Wahl des Ressourcengruppenstandorts auf Traffic Manager aus?

Traffic Manager ist ein einzelner globaler Dienst. Es ist kein regionaler Dienst. Die Wahl des Ressourcengruppenstandorts spielt für Traffic Manager-Profile, die in der Ressourcengruppe bereitgestellt werden, keine Rolle.

Für Azure Resource Manager ist es erforderlich, dass für alle Ressourcengruppen ein Standort angegeben wird, mit dem der Standardstandort für die in dieser Ressourcengruppe bereitgestellten Ressourcen bestimmt wird. Sie werden nach diesem Standort gefragt, wenn Sie während der Erstellung eines Traffic Manager-Profils über das Azure-Portal eine neue Ressourcengruppe erstellen.

Für alle Traffic Manager-Profile wird als Standort **global** verwendet. Da dies der einzige akzeptierte Wert ist, kann auf diese Einstellung nicht über das Azure-Portal, über PowerShell oder über die Azure-Befehlszeilenschnittstelle zugegriffen werden. Hiermit wird die Standardeinstellung der Ressourcengruppe überschrieben.

### Wie kann ich den aktuellen Integritätsstatus jedes Endpunkts bestimmen?

Der aktuelle Überwachungsstatus jedes Endpunkts sowie das Gesamtprofil werden im Azure-Portal angezeigt. Diese Informationen sind auch über die [REST-API](https://msdn.microsoft.com/library/azure/mt163667.aspx) von Traffic Manager, [PowerShell-Cmdlets](https://msdn.microsoft.com/library/mt125941.aspx) und die [plattformübergreifende Azure-Befehlszeilenschnittstelle](../xplat-cli-install.md) verfügbar.

Es ist nicht möglich, Verlaufsinformationen zur Integrität von Endpunkten anzuzeigen, und Sie können auch keine Warnungen bei Änderungen der Integrität von Endpunkten konfigurieren.

### Kann ich HTTPS-Endpunkte überwachen?

Ja. Traffic Manager unterstützt Tests per HTTPS. Konfigurieren Sie in der Überwachungskonfiguration einfach **HTTPS** als Protokoll. Beachten Sie Folgendes:

- Die serverseitigen Zertifikate werden nicht überprüft.
- Serverseitige SNI-Zertifikate werden nicht unterstützt.
- Clientzertifikate werden nicht unterstützt.

### Welcher Hostheader wird für die Integritätsprüfungen für Endpunkte verwendet?

Als Hostheader in HTTP- und HTTPS-Integritätsprüfungen wird jeweils der DNS-Name verwendet, der einem Endpunkt zugeordnet ist. Er wird im Azure-Portal, in [PowerShell-Cmdlets](https://msdn.microsoft.com/library/mt125941.aspx), in der [plattformübergreifenden Azure-Befehlszeilenschnittstelle](../xplat-cli-install.md) und in der [REST-API](https://msdn.microsoft.com/library/azure/mt163667.aspx) als Ziel des Endpunkts verfügbar gemacht.

Dieser Wert ist Teil der Endpunktkonfiguration. Der im Hostheader verwendete Wert kann nicht separat von der Zieleigenschaft („target“) angegeben werden.


## Nächste Schritte

Informieren Sie sich über die [Funktionsweise von Traffic Manager](traffic-manager-how-traffic-manager-works.md).

Informieren Sie sich über die von Traffic Manager unterstützten [Methoden für das Datenverkehrsrouting](traffic-manager-routing-methods.md).

Informieren Sie sich über das [Erstellen eines Traffic Manager-Profils](traffic-manager-manage-profiles.md).

[Problembehandlung beim Status „Heruntergestuft“](traffic-manager-troubleshooting-degraded.md) auf einem Traffic Manager-Endpunkt.

<!---HONumber=AcomDC_0706_2016-->
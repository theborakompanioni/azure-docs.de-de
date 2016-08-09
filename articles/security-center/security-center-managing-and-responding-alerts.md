<properties
   pageTitle="Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center | Microsoft Azure"
   description="In diesem Dokument erfahren Sie, wie Sie Azure Security Center-Funktionen verwenden, um Sicherheitswarnungen zu verwalten und auf diese zu reagieren."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.topic="hero-article"
   ms.devlang="na"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/21/2016"
   ms.author="yurid"/>

# Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center
In diesem Dokument erfahren Sie, wie Sie Azure Security Center verwenden, um Sicherheitswarnungen zu verwalten und auf diese zu reagieren.

## Was sind Sicherheitswarnungen?
Security Center erfasst, analysiert und vereinigt automatisch Protokolldaten von Ihren Azure-Ressourcen, vom Netzwerk und von verbundenen Partnerlösungen, z.B. Lösungen zum Schutz von Firewalls und Endpunkten, um echte Bedrohungen zu erkennen und falsch positive Ergebnisse zu reduzieren. Eine Liste mit priorisierten Sicherheitswarnungen wird im Security Center zusammen mit den Informationen angezeigt, die Sie zum schnellen Untersuchen des Problems benötigen. Außerdem sind Empfehlungen zum Reagieren auf einen Angriff vorhanden.

> [AZURE.NOTE] Weitere Informationen zur Funktionsweise der Security Center-Erkennungsfunktionen finden Sie unter [Azure Security Center-Erkennungsfunktionen](security-center-detection-capabilities.md).

Microsoft-Sicherheitsexperten analysieren ständig neue weltweite Bedrohungen, z. B. neue Angriffsmuster und Trends, die für die unternehmenseigenen Verbraucher- und Unternehmensprodukte und Onlinedienste auftreten. Für Security Center können so die Erkennungsalgorithmen aktualisiert werden, wenn neue Sicherheitsrisiken und Exploits erkannt werden, und Kunden können über die sich verändernde Bedrohungslage informiert werden. Beispiele für Arten von Bedrohungen, die mit Security Center erkannt werden können:

- **Brute-Force-Erkennung über Netzwerkdaten**: Hierbei werden Machine Learning-Modelle verwendet, die typische Muster von Netzwerkdatenverkehr für Ihre Anwendungen verstehen. Dies ermöglicht eine effektivere Erkennung von Zugriffsversuchen, die nicht von berechtigten Benutzern durchgeführt werden, sondern zu unlauteren Zwecken.
- **Brute-Force-Erkennung über Endpunktdaten**: Basiert auf der Analyse von Computerprotokollen. Dies ermöglicht eine Unterscheidung zwischen fehlerhaften und erfolgreichen Versuchen.
- **Virtuelle Maschinen, die mit schädlichen IPs kommunizieren**: Hierbei wird Netzwerkdatenverkehr mit der globalen Bedrohungsintelligenz von Microsoft verglichen. Es werden Computer erkannt, die gefährdet sind und mit Command-and-Control-Servern (C&C) kommunizieren (und umgekehrt).
- **Gefährdete VMs**: Basiert auf der Verhaltensanalyse von Computerprotokollen und der Korrelation mit anderen Signalen. Es werden anomale Ereignisse erkannt, die mit hoher Wahrscheinlichkeit das Ergebnis einer Computermanipulation und Ausnutzung von Sicherheitslücken sind.

## Verwalten von Sicherheitswarnungen

Sie können die aktuellen Warnungen prüfen, indem Sie sich die Kachel **Sicherheitswarnungen** ansehen. Öffnen Sie das Azure-Portal, und gehen Sie folgendermaßen vor, wenn Sie weitere Details zu jeder Warnung anzeigen möchten:

1. Auf dem Security Center-Dashboard sehen Sie die Kachel **Sicherheitswarnungen**.

    ![Kachel „Sicherheitswarnungen“ in Security Center](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig1-ga.png)

2.  Klicken Sie auf die Kachel, um das Blatt **Sicherheitswarnungen** zu öffnen, das weitere Details zu den Warnungen enthält (siehe Abbildung unten).

    ![Das Blatt „Sicherheitswarnungen“ in Security Center](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig2-ga.png)

Im unteren Teil dieses Blatts sind die Details für die einzelnen Warnungen zu sehen. Zum Sortieren klicken Sie auf die Spalte, nach der Sie sortieren möchten. Die Spalten sind wie folgt definiert:

- **Warnung**: Eine kurze Erläuterung der Warnung.
- **Anzahl**: Eine Liste aller Warnungen dieses bestimmten Typs, die an einem bestimmten Tag erkannt wurden.
- **Erkannt von**: Der Dienst, der die Warnung ausgelöst hat.
- **Datum**: Das Datum, an dem das Ereignis aufgetreten ist.
- **Status**: Der aktuelle Status für diese Warnung. Es gibt zwei Arten von Zuständen:
    - **Aktiv**: Die Sicherheitswarnung wurde erkannt.
    - **Verworfen**: Die Sicherheitswarnung wurde vom Benutzer verworfen. Dieser Status wird üblicherweise für Warnungen verwendet, die untersucht, aber gemindert oder als harmlos eingestuft wurden.

- **Schweregrad**: Gibt den Schweregrad an („Hoch“, „Mittel“ oder „Niedrig“).

### Filtern von Warnungen

Sie können Warnungen nach Datum, Status und Schweregrad filtern. Das Filtern von Warnungen kann nützlich für Szenarien sein, in denen Sie den Bereich der angezeigten Warnungen einschränken müssen. Es könnte beispielsweise sein, dass Sie während der Untersuchung einer möglichen Sicherheitsverletzung im System die Sicherheitswarnungen überprüfen möchten, die in den letzten 24 Stunden aufgetreten sind.

1. Klicken Sie auf dem Blatt **Sicherheitswarnungen** auf **Filter**. Das Blatt **Filter** wird geöffnet. Sie können Werte für Datum, Status und Schweregrad auswählen, die Sie anzeigen möchten.

	![Filtern von Warnungen in Security Center](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig3-ga.png)

2. 	Nach dem Untersuchen einer Sicherheitswarnung stellen Sie unter Umständen fest, dass es sich um falsch positive Warnung für Ihre Umgebung handelt oder ein erwartetes Verhalten für eine bestimmte Ressource darstellt. Falls Sie feststellen, dass eine Sicherheitswarnung nicht zutrifft, können Sie die Warnung immer verwerfen und dann aus der Ansicht herausfiltern. Es gibt zwei Möglichkeiten, eine Sicherheitswarnung zu verwerfen. Klicken Sie mit der rechten Maustaste auf eine Warnung, und wählen Sie **Verwerfen**, oder zeigen Sie auf ein Element, klicken Sie auf die drei Punkte, die rechts angezeigt werden, und wählen Sie **Verwerfen**. Sie können verworfene Sicherheitswarnungen anzeigen, indem Sie auf **Filter** klicken und **Verworfen** auswählen.

	![Verwerfen von Warnungen in Security Center](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig4-ga.png)

### Reagieren auf Sicherheitswarnungen

Wählen Sie eine Sicherheitswarnung aus, um weitere Informationen zu den Ereignissen zu erhalten, die die Warnung ausgelöst haben, sowie zu den Schritten, die Sie als Reaktion auf den Angriff ausführen müssen (falls zutreffend). Sicherheitswarnungen werden nach Typ und Datum gruppiert. Wenn Sie auf eine Sicherheitswarnung klicken, wird ein Blatt mit einer Liste der gruppierten Warnungen geöffnet.

![Reagieren auf Sicherheitswarnungen in Azure Security Center](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig5-ga.png)

In diesem Fall bezieht sich die ausgelöste Warnung auf eine verdächtige RDP-Aktivität (Remote Desktop Protocol). Die erste Spalte zeigt, welche Ressourcen angegriffen wurden. In der zweiten Spalte sehen Sie , wie häufig die Ressource angegriffen wurde. Die dritte Spalte enthält die Uhrzeit des Angriffs. Die vierte Spalte zeigt den Status der Warnung und die fünfte Spalte den Schweregrad des Angriffs. Klicken Sie nach dem Überprüfen dieser Informationen auf die Ressource, die angegriffen wurde. Ein neues Blatt wird geöffnet.

![Vorschläge für die Bearbeitung von Sicherheitswarnungen in Azure Security Center](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig6-ga.png)

Im Feld **Beschreibung** auf diesem Blatt finden Sie weitere Informationen zu diesem Ereignis. Dank dieser zusätzlichen Details erhalten Sie Informationen dazu, wodurch die Sicherheitswarnung ausgelöst wurde, sowie die Zielressource, die IP-Quelladresse (falls zutreffend) und Empfehlungen zur Lösung. In einigen Fällen ist die IP-Quelladresse leer (nicht verfügbar), da nicht alle Windows-Sicherheitsereignisprotokolle die IP-Adresse enthalten.

> [AZURE.NOTE] Die von Security Center vorgeschlagene Wiederherstellung variiert je nach Sicherheitshinweis. In einigen Fällen müssen Sie möglicherweise weitere Azure-Funktionen verwenden, um die empfohlenen Lösungen zu implementieren. Beispielsweise kann die Abhilfe für diesen Angriff darin bestehen, die IP-Adresse, die den Angriff generiert, durch Verwendung einer [Netzwerk-ACL](../virtual-network/virtual-networks-acl.md) oder einer Regel für die [Netzwerksicherheitsgruppe](../virtual-network/virtual-networks-nsg.md) auf eine Negativliste zu setzen.

## Sicherheitswarnungen nach Typ
Die gleichen Schritte, die zum Zugreifen auf die Warnung zur verdächtigen RDP-Aktivität verwendet wurden, können auch zum Zugreifen auf andere Arten von Warnungen ausgeführt werden. Hier sind einige weitere Beispiele für Security Center-Warnungen angegeben:

### Potenzielle Einschleusung von SQL-Befehlen
Eine Einschleusung von SQL-Befehlen ist ein Angriff, bei dem Schadcode in Zeichenfolgen eingefügt wird, die später zur Analyse und Ausführung an eine Instanz von SQL Server übergeben werden. Jedes Verfahren, bei dem SQL-Anweisungen erstellt werden, sollte auf Sicherheitsrisiken in Bezug auf Einschleusungen überprüft werden, da SQL Server alle syntaktisch gültigen Abfragen ausführt, die empfangen werden.

![Warnung vor der Einschleusung von SQL-Befehlen](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig9.png)

Diese Warnung enthält Informationen, mit denen Sie die angegriffene Ressource, die Erkennungsdauer und den Status des Angriffs identifizieren können. Außerdem ist ein Link zu weiteren Untersuchungsschritten angegeben.

### Erkennung von verdächtigem ausgehendem Datenverkehr

Für Netzwerkgeräte ist die Ermittlung und Profilerstellung nahezu genauso wie für andere Arten von Systemen möglich. Angreifer beginnen in der Regel mit der Portüberwachung bzw. Port-Sweeping. Im folgenden Beispiel ist es zu verdächtigem SSH-Datenverkehr von einer VM gekommen, für den die Gefahr eines Brute-Force-SSH-Angriffs oder Port-Sweeping-Angriffs gegen eine externe Ressource besteht.

![Warnung vor verdächtigem ausgehendem Datenverkehr](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig-10.png)

Diese Warnung enthält Informationen, mit denen Sie die für die Initiierung verwendete Ressource, den kompromittierten Computer, die Erkennungsdauer und das verwendete Protokoll und den Port identifizieren können. Auf diesem Blatt ist auch eine Liste mit Lösungsschritten angegeben, die Sie ausführen können, um das Problem zu beseitigen.

### Netzwerkkommunikation mit einem schädlichen Computer
 
Durch die Nutzung von Microsoft Threat Intelligence-Feeds kann Azure Security Center kompromittierte Computer erkennen, die mit schädlichen IP-Adressen kommunizieren. Häufig ist dies ein Befehls- und Steuerungszentrum (Command and Control Center). In diesem Fall hat Azure Security Center erkannt, dass für die Kommunikation die Schadsoftware „Pony Loader“ verwendet wurde (auch als [Fareit](https://www.microsoft.com/security/portal/threat/encyclopedia/entry.aspx?Name=PWS:Win32/Fareit.AF) bezeichnet).

![Warnung vor Netzwerkkommunikation mit einem schädlichen Computer](./media/security-center-managing-and-responding-alerts/security-center-managing-and-responding-alerts-fig9-ga.png)

Diese Warnung enthält Informationen, mit denen Sie die für den Angriff verwendete Ressource, die Ziel-IP-Adresse, die IP-Adresse des Angreifers und die Erkennungsdauer identifizieren können.

> [AZURE.NOTE] Aus Datenschutzgründen wurden die realen IP-Adressen aus diesem Screenshot entfernt.


## Weitere Informationen

In diesem Dokument haben Sie erfahren, wie Sie Sicherheitsrichtlinien in Security Center konfigurieren können. Weitere Informationen zu Security Center finden Sie in den folgenden Quellen:

- [Planungs- und Betriebshandbuch für Azure Security Center](security-center-planning-and-operations-guide.md)
- [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center](security-center-managing-and-responding-alerts.md)
- [Überwachen der Sicherheitsintegrität in Azure Security Center](security-center-monitoring.md): Erfahren Sie, wie Sie die Integrität Ihrer Azure-Ressourcen überwachen.
- [Überwachen von Partnerlösungen mit Azure Security Center](security-center-partner-solutions.md): Erfahren Sie, wie der Integritätsstatus Ihrer Partnerlösungen überwacht wird.
- [Azure Security Center – Häufig gestellte Fragen](security-center-faq.md): Hier finden Sie häufig gestellte Fragen zur Verwendung des Diensts.
- [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) (Blog zur Azure-Sicherheit): Hier finden Sie Blogbeiträge zur Sicherheit und Compliance von Azure.

<!---HONumber=AcomDC_0803_2016-->
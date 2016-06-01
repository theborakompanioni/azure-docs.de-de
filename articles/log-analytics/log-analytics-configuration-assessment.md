<properties
	pageTitle="Konfigurationsbewertungslösung in Log Analytics | Microsoft Azure"
	description="Die Konfigurationsbewertungslösung in Log Analytics bietet ausführliche Informationen über den aktuellen Zustand der System Center Operations Manager-Serverinfrastruktur unter Verwendung von Operations Manager-Agents oder einer Operations Manager-Verwaltungsgruppe."
	services="log-analytics"
	documentationCenter=""
	authors="bandersmsft"
	manager="jwhit"
	editor=""/>

<tags
	ms.service="log-analytics"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="05/04/2016"
	ms.author="banders"/>

# Konfigurationsbewertungslösung in Log Analytics

Die Konfigurationsbewertungslösung in Log Analytics hilft Ihnen dabei, potenzielle Probleme bei der Serverkonfiguration durch Warnungen und Empfehlungen zu ermitteln.

Diese Lösung erfordert System Center Operations Manager. Die Konfigurationsbewertung ist nicht verfügbar, wenn Sie nur direkt verbundene Agents verwenden.

Für das Anzeigen von Informationen in der Konfigurationsbewertungslösung ist das Silverlight-Plug-In für Ihren Browser erforderlich.

>[AZURE.NOTE] Die Konfigurationsbewertungslösung ist nur für Arbeitsbereiche in der Region „USA, Osten“ verfügbar, er wird auch nicht in anderen Regionen eingeführt werden. Die Funktionen der Konfigurationsbewertungslösung werden durch Workload-spezifische Bewertungen ersetzt, einschließlich Active Directory- und SQL Server-Bewertung.

![Kachel „Konfigurationsbewertung“](./media/log-analytics-configuration-assessment/oms-config-assess-tile.png)

Konfigurationsdaten werden von überwachten Servern gesammelt und dann zur Verarbeitung an den OMS-Dienst in der Cloud gesendet. Auf die empfangenen Daten wird Logik angewendet, und der Clouddienst zeichnet die Daten auf. Verarbeitete Daten für die Server werden für die folgenden Bereiche angezeigt:

- **Alerts**: zeigt die konfigurationsbezogenen, proaktiven Warnungen, die für die überwachten Server ausgelöst wurden. Diese werden durch Regeln vom Microsoft-Kundendienst und -Support mithilfe von bewährten Methoden aus dem Einsatz erstellt.
- **Knowledge Recommendations:** zeigt die Artikel aus der Microsoft Knowledge Base an, die für die in Ihrer Infrastruktur vorgefundenen Workloads empfohlen werden. Diese Artikel werden aufgrund Ihrer Konfiguration mithilfe von maschinellem Lernen automatisch vorgeschlagen.
- **Servers and Workloads Analyzed:** zeigt die Server und Workloads an, die von OMS überwacht werden.

![Dashboard „Konfigurationsbewertung“](./media/log-analytics-configuration-assessment/oms-config-assess-dash01.png)

### Technologien, die mit der Konfigurationsbewertung analysiert werden können

Mit der Konfigurationsbewertung von OMS werden die folgenden Workloads analysiert:

- Windows Server 2012 und Microsoft Hyper-V Server 2012
- Windows Server 2008 und Windows Server 2008 R2, einschließlich:
    - Active Directory
	- Hyper-V-Host
	- Allgemeines Betriebssystem
- SQL Server 2008 und höher
    - SQL Server-Datenbankmodul
- Microsoft SharePoint 2010
- Microsoft Exchange Server 2010 und Microsoft Exchange Server 2013
- Microsoft Lync Server 2013 und Lync Server 2010
- System Center 2012 SP1 – Virtual Machine Manager

Für SQL Server werden die folgenden 32-Bit- und 64-Bit-Editionen für Analysezwecke unterstützt:

- SQL Server 2016 – alle Editionen
- SQL Server 2014 – alle Editionen
- SQL Server 2008 und 2008 R2 – alle Editionen

Das SQL Server-Datenbankmodul wird auf allen unterstützten Editionen analysiert. Darüber hinaus wird bei der Ausführung in der WOW64-Implementierung die 32-Bit-Edition von SQL Server unterstützt.

## Installieren und Konfigurieren der Lösung
Verwenden Sie die folgenden Informationen zum Installieren und Konfigurieren der Lösung.

- Operations Manager ist für die Konfigurationsbewertungslösung erforderlich.
- Sie benötigen einen Operations Manager-Agent auf jedem Computer, auf dem die Konfiguration bewertet werden soll.
- Fügen Sie mithilfe des unter [Hinzufügen von Log Analytics-Lösungen aus dem Lösungskatalog](log-analytics-add-solutions.md) beschriebenen Verfahrens Ihrem OMS-Arbeitsbereich die Konfigurationsbewertungslösung hinzu. Es ist keine weitere Konfiguration erforderlich.

## Details zur Datensammlung für die Konfigurationsbewertung

Die folgende Tabelle zeigt die Datensammlungsmethoden und andere Details dazu, wie Daten für die Konfigurationsbewertung gesammelt werden.

| Plattform | Direkt-Agent | SCOM-Agent | Azure Storage | SCOM erforderlich? | Daten von SCOM-Agent über Verwaltungsgruppe gesendet | Sammlungshäufigkeit |
|---|---|---|---|---|---|---|
|Windows|![Nein](./media/log-analytics-configuration-assessment/oms-bullet-red.png)|![Ja](./media/log-analytics-configuration-assessment/oms-bullet-green.png)|![Nein](./media/log-analytics-configuration-assessment/oms-bullet-red.png)| ![Ja](./media/log-analytics-configuration-assessment/oms-bullet-green.png)|![Ja](./media/log-analytics-configuration-assessment/oms-bullet-green.png)| Zweimal pro Tag|


## Warnungen zur Konfigurationsbewertung
Sie können Warnungen der Konfigurationsbewertung auf der Seite „Warnungen“ anzeigen und verwalten. Warnungen informieren Sie über das gefundene Problem, die Ursache und Möglichkeiten zum Beheben des Problems. Sie enthalten auch Informationen über die Konfigurationseinstellungen in Ihrer Umgebung, die Leistungsprobleme verursachen können.

![Warnungsansicht](./media/log-analytics-configuration-assessment/oms-config-assess-alerts01.png)

>[AZURE.NOTE] Die Warnungen zur Konfigurationsbewertung unterscheiden sich von den anderen Warnungen in Log Analytics. Das Anzeigen von Warnungen erfordert das Silverlight-Plug-In für Ihren Browser.

Wenn Sie ein Element oder eine Kategorie von Elementen auf der Seite „Warnungen“ auswählen, sehen Sie eine Liste von Servern oder Workloads mit Warnungen zu den einzelnen Element.

![Warnungsliste](./media/log-analytics-configuration-assessment/oms-config-assess-alerts-view-config.png)

Jede Warnung enthält einen Link zu einem Artikel in der Microsoft Knowledge Base. Diese Artikel bieten enthalten Informationen zu der Warnung.

>[AZURE.TIP] Standardmäßig beträgt die maximale Anzahl angezeigter Warnungen 2.000. Um weitere Warnungen anzuzeigen, klicken Sie auf die Benachrichtigungsleiste über der Liste der Warnungen.

Sie können auf beliebige Elemente in der Liste klicken, um den KB-Artikel anzuzeigen, mit dem Sie möglicherweise die Ursache des Problems beheben können, das die Warnung generiert hat.

![KB-Artikel](./media/log-analytics-configuration-assessment/oms-config-assess-alerts-details-kb.png)

Sie können Warnungsregeln festlegen, um bestimmte Regeln oder Klassen von Regeln zu ignorieren.

![Verwalten von Warnungsregeln](./media/log-analytics-configuration-assessment/oms-config-assess-alert-rules.png)

## Wissensempfehlungen
Bei der Anzeige von Knowledge Base-Empfehlungen erhalten Sie Protokollsuchergebnisse mit Microsoft KB-Artikeln, die für Ihre Workloads und Computer empfohlen werden und zusätzliche Informationen zur Warnung bereitstellen.

![Suchergebnisse für Knowledge Base-Empfehlungen](./media/log-analytics-configuration-assessment/oms-config-assess-knowledge-recommendations.png)

## Analysierte Server und Workloads
Wenn Sie Knowledge Base-Empfehlungen anzeigen, erhalten Sie Protokollsuchergebnisse mit sämtlichen Servern und Workloads, die OMS durch Operations Manager bekannt sind.

![Server und Workloads](./media/log-analytics-configuration-assessment/oms-config-assess-servers-workloads.png)

## Nächste Schritte

- Verwenden Sie die [Protokollsuche in Log Analytics](log-analytics-log-searches.md), um ausführliche Daten zur Konfigurationsbewertung anzuzeigen.

<!---HONumber=AcomDC_0518_2016-->
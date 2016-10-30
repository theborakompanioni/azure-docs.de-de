<properties 
   pageTitle="Vergleich von Microsoft-Überwachungsprodukten | Microsoft Azure"
   description="Microsoft Operations Management Suite (OMS) ist eine cloudbasierte IT-Verwaltungslösung von Microsoft, die Sie beim Verwalten und Schützen Ihrer lokalen Infrastruktur und Cloudinfrastruktur unterstützt.  Dieser Artikel enthält Informationen zu den verschiedenen Diensten von OMS und Links zu Seiten mit ausführlicheren Informationen."
   services="operations-management-suite"
   documentationCenter=""
   authors="bwren"
   manager="jwhit"
   editor="tysonn" />
<tags 
   ms.service="operations-management-suite"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="10/27/2016"
   ms.author="bwren" />


# <a name="microsoft-monitoring-product-comparison"></a>Vergleich von Microsoft-Überwachungsprodukten

Dieser Artikel enthält einen Vergleich zwischen System Center Operations Manager (SCOM) und Log Analytics in Operations Management Suite (OMS). Dabei geht es um die Architektur, die Logik der Überwachung von Ressourcen und die Durchführung von Analysen für die gesammelten Daten.  Auf diese Weise sollen Ihnen die Grundlagen zu den Unterschieden und den relativen Stärken vermittelt werden.  

## <a name="basic-architecture"></a>Grundlegende Architektur
### <a name="system-center-operations-manager"></a>System Center Operations Manager

Alle SCOM-Komponenten werden in Ihrem Rechenzentrum installiert.  [Agents werden auf Windows- und Linux-Computern installiert](http://technet.microsoft.com/library/hh551142.aspx) , die mit SCOM verwaltet werden.  Agents stellen eine Verbindung mit [Verwaltungsservern](https://technet.microsoft.com/library/hh301922.aspx) her, die mit der SCOM-Datenbank und dem Data Warehouse kommunizieren.  Für Agents wird die Domänenauthentifizierung verwendet, um die Verbindung mit Verwaltungsservern herzustellen.  Von außerhalb einer vertrauenswürdigen Domäne kann eine Authentifizierung per Zertifikat durchgeführt oder eine Verbindung mit einem [Gatewayserver](https://technet.microsoft.com/library/hh212823.aspx)hergestellt werden.

Für SCOM sind zwei SQL-Datenbanken erforderlich, eine für Betriebsdaten und ein anderes Data Warehouse zur Unterstützung der Berichterstellung und Datenanalyse.  Auf einem [Berichtsserver](https://technet.microsoft.com/library/hh298611.aspx) wird SQL Reporting Services ausgeführt, um Berichte für Daten des Data Warehouse zu erstellen. 

SCOM kann Cloudressourcen mithilfe von Management Packs für Produkte wie [Azure](https://www.microsoft.com/download/details.aspx?id=38414), [Office 365](https://www.microsoft.com/download/details.aspx?id=43708) und [AWS](http://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/AWSManagementPack.html) überwachen.  Bei diesen Management Packs werden ein oder mehrere lokale Agents als Proxys zum Ermitteln von Cloudressourcen und Ausführen von Workflows zum Messen der Leistung und Verfügbarkeit eingesetzt.  Proxy-Agents werden auch verwendet, um [Netzwerkgeräte und andere externe Ressourcen zu überwachen](https://technet.microsoft.com/library/hh212935.aspx) .

Die Betriebskonsole ist eine Windows-Anwendung, die eine Verbindung mit einem der Verwaltungsserver herstellt und es dem Administrator ermöglicht, gesammelte Daten anzuzeigen und zu analysieren und die SCOM-Umgebung zu konfigurieren.  Eine webbasierte Konsole kann auf einem beliebigen IIS-Server gehostet werden und ermöglicht die Datenanalyse über einen Browser.

![SCOM-Architektur](media/operations-management-suite-monitoring-product-comparison/scom-architecture.png)

### <a name="log-analytics"></a>Log Analytics

Die meisten OMS-Komponenten sind in der Azure-Cloud angeordnet, damit Sie sie mit minimalem Kosten- und Administrationsaufwand bereitstellen und verwalten können.  Alle von Log Analytics gesammelten Daten werden im OMS-Repository gespeichert.

Mit Log Analytics können Daten von einer von drei Quellen gesammelt werden:

- Physische und virtuelle Computer, auf denen Windows und der [Microsoft Monitoring Agent (MMA)](https://technet.microsoft.com/library/mt484108.aspx) oder Linux und der [Operations Management Suite-Agent für Linux](https://technet.microsoft.com/library/mt622052.aspx) ausgeführt werden.  Diese Computer können lokale Computer oder virtuelle Computer in Azure oder einer anderen Cloud sein.
- Azure Storage-Konto mit Daten der [Azure-Diagnose](../cloud-services/cloud-services-dotnet-diagnostics.md) , die über eine Azure-Workerrolle oder -Webrolle oder einen virtuellen Azure-Computer erfasst wurden.
- Verbindung mit einer SCOM-Verwaltungsgruppe ([Herstellen einer Verbindung von System Center Operations Manager mit OMS](https://technet.microsoft.com/library/mt484104.aspx)).  Bei dieser Konfiguration kommunizieren die Agents mit SCOM-Verwaltungsservern, mit denen die Daten für die SCOM-Datenbank bereitgestellt werden, wo sie dann an den OMS-Datenspeicher weitergegeben werden.
Administratoren analysieren erfasste Daten und konfigurieren Log Analytics mit dem OMS-Portal, das in Azure gehostet wird und über jeden Browser zugänglich ist.  Mobile Apps zum Zugreifen auf diese Daten sind für die Standardplattformen verfügbar.

![Log Analytics-Architektur](media/operations-management-suite-monitoring-product-comparison/log-analytics-architecture.png)

### <a name="integrating-scom-and-log-analytics"></a>Integration von SCOM und Log Analytics

Wenn SCOM als Datenquelle für Log Analytics verwendet wird, können Sie die Features beider Produkte in einer Hybrid-Umgebung für die Überwachung nutzen.  Sie können vorhandene SCOM-Agents über die von OMS verwaltete Betriebskonsole konfigurieren und weiterhin Management Packs von SCOM ausführen.  
Daten aus einer verbundenen SCOM-Verwaltungsgruppe werden mit einer von vier Methoden für Log Analytics bereitgestellt:

- Ereignisse und Leistungsdaten werden vom Agent erfasst und für SCOM bereitgestellt.  Verwaltungsserver in SCOM stellen die Daten dann für Log Analytics bereit.
- Einige Ereignisse, z.B. IIS-Protokolle und Sicherheitsereignisse, werden vom Agent weiterhin direkt für Log Analytics bereitgestellt.
- Bei einigen Lösungen wird für den Agent zusätzliche Software verfügbar gemacht, oder es wird erzwungen, dass Software zum Sammeln zusätzlicher Daten installiert wird.  Diese Daten werden normalerweise direkt an Log Analytics gesendet.
- Einige Lösungen erfassen Daten direkt von SCOM-Verwaltungsservern, die ursprünglich nicht vom Agent stammen.  Mit der [Lösung „Alert Management“](https://technet.microsoft.com/library/mt484092.aspx) werden Warnungen von SCOM nach ihrer Erstellung erfasst.

## <a name="monitoring-logic"></a>Überwachungslogik
Für SCOM und Log Analytics werden ähnliche Daten genutzt, die von Agents erfasst werden. Es bestehen aber grundlegende Unterschiede in Bezug darauf, wie die Logik für die Datenerfassung definiert und implementiert wird und wie die erfassten Daten analysiert werden.

### <a name="operations-manager"></a>Operations Manager
Die Überwachungslogik für SCOM wird in Form von [Management Packs](https://technet.microsoft.com/library/hh457558.aspx) implementiert. Diese enthalten Logik zum Ermitteln von zu überwachenden Komponenten, zum Messen der Integrität dieser Komponenten und zum Sammeln von zu analysierenden Daten.  Das Überwachen von Daten kann so einfach wie das Erfassen eines Ereignisses oder Leistungsindikators sein, oder es kann eine komplexe Logik verwendet werden, die in einem Skript implementiert wird.  Management Packs mit einer vollständigen Überwachungslogik sind für viele verschiedene [Anwendungen von Microsoft und Drittanbietern](http://go.microsoft.com/fwlink/?LinkId=82105) sowie für Hardware und Netzwerkgeräte verfügbar.  Sie können für benutzerdefinierte Anwendungen [eigene Management Packs erstellen](http://aka.ms/mpauthor).

Management Packs enthalten mehrere Workflows, die jeweils eine bestimmte Überwachungsfunktion erfüllen, z.B. das Nehmen von Stichproben für einen Leistungsindikator, Überprüfen des Zustands eines Diensts oder Ausführen eines Skripts.  Jeder Workflow wird separat ausgeführt und definiert seine eigenen Ergebnisse, z.B. die Zieldatenbank für Schreibvorgänge und die Generierung einer Warnung. 

Sie können Details des Workflows außer Kraft setzen, z.B. die Ausführungshäufigkeit, den Schwellenwert für Fehler und den Schweregrad der generierten Warnung.  Sie können auch zusätzliche Funktionen bereitstellen, indem Sie eigene Workflows hinzufügen.

![Außerkraftsetzungen](media/operations-management-suite-monitoring-product-comparison/scom-overrides.png)

Management Packs werden in der Operations Manager-Datenbank installiert und von Verwaltungsservern automatisch an Agents verteilt.  Jeder Agent lädt automatisch Management Packs herunter und lädt Workflows, die für die installierten Anwendungen relevant sind.  Vom Agent erfasste Daten werden wieder für den Verwaltungsserver bereitgestellt, damit sie in die SCOM-Datenbank und das Data Warehouse eingefügt werden können.  Mit der Betriebskonsole können Sie diese Daten über benutzerdefinierte Ansichten, Dashboards und im Management Pack enthaltene Berichte anzeigen und analysieren.

Die Verteilung von Management Packs ist im folgenden Diagramm dargestellt.

![Management Pack-Datenfluss](media/operations-management-suite-monitoring-product-comparison/scom-mpflow.png)

### <a name="log-analytics"></a>Log Analytics
#### <a name="event-and-performance-collection"></a>Sammlung von Ereignissen und Leistungsdaten
Log Analytics sammelt Ereignisse und Leistungsindikatoren von Agent-Systemen, indem Quellen wie das Windows-Ereignisprotokoll, IIS-Protokolle und Syslog verwendet werden.  Sie können Kriterien definieren, nach denen Daten über das Log Analytics-Portal gesammelt werden, und dann Protokollabfragen erstellen, um die erfassten Daten zu analysieren.  Einige standardmäßige Kriterien werden definiert, wenn Sie den OMS-Arbeitsbereich erstellen, und Sie können zusätzliche Daten für bestimmte Anwendungen definieren. 

![Ereignisprotokolle in Log Analytics definieren](media/operations-management-suite-monitoring-product-comparison/log-analytics-definedata.png)

SCOM verfügt über viele ausführliche Workflows, bei denen normalerweise bestimmte Kriterien für Daten und die jeweils durchzuführende Aktion definiert sind, während Log Analytics allgemeinere Kriterien für die Datenerfassung enthält.  Protokollabfragen und -lösungen liefern eher zielgerichtete Kriterien zum Analysieren und Behandeln bestimmter Daten in der Cloud nach deren Erfassung.

#### <a name="solutions"></a>Lösungen
Lösungen umfassen zusätzliche Logik für die Datenerfassung und -analyse.  Sie können im Lösungskatalog Lösungen auswählen, um sie Ihrem OMS-Abonnement hinzuzufügen.

![Lösungskatalog](media/operations-management-suite-monitoring-product-comparison/log-analytics-solutiongallery.png)

Lösungen werden hauptsächlich in der Cloud ausgeführt und ermöglichen die Analyse von Ereignissen und Leistungsindikatoren, die im OMS-Repository erfasst werden.  Es können auch zusätzliche Daten definiert werden, die gesammelt und mit Protokollabfragen oder über eine zusätzliche Benutzeroberfläche analysiert werden können, die von der Lösung im OMS-Dashboard bereitgestellt wird. 

Mit der [Change Tracking-Lösung](https://technet.microsoft.com/library/mt484099.aspx) werden beispielsweise Konfigurationsänderungen auf Agent-Systemen erkannt und Ereignisse in das OMS-Repository geschrieben, die über mehrere grafische Ansichten zur Zusammenfassung der erkannten Änderungen analysiert werden können.  Sie können über die Übersicht auf Protokollabfragen zugreifen, unter denen die ausführlichen Daten angezeigt werden, die von der Lösung erfasst wurden.


Sie können auswählen, welche Lösungen Sie dem Abonnement hinzufügen möchten, aber Sie können derzeit noch keine eigenen Lösungen erstellen.  Sie können die Ereignisse und Leistungsindikatoren für die Erfassung auswählen und basierend auf Ihren eigenen Protokollabfragen benutzerdefinierte Ansichten erstellen.

Im folgenden Diagramm ist die Überwachungslogik für Log Analytics zusammengefasst.

![Datenfluss der Log Analytics-Lösung](media/operations-management-suite-monitoring-product-comparison/log-analytics-solution-flow.png)

## <a name="health-monitoring"></a>Systemüberwachung
### <a name="operations-manager"></a>Operations Manager
SCOM kann die unterschiedlichen Komponenten einer Anwendung modellieren und die Integrität für jede Komponente in Echtzeit darstellen.  So können Sie nicht nur erkannte Fehler und die Leistung in Abhängigkeit der Zeit anzeigen, sondern auch jederzeit die Integrität einer Anwendung oder eines Systems und der dazugehörigen Komponenten überprüfen.  Da die Zeiträume bekannt sind, zu denen eine Anwendung verfügbar ist, unterstützt das Integritätsmodul in SCOM auch Vereinbarungen zum Servicelevel (SLAs), bei denen die Verfügbarkeit einer Anwendung in Abhängigkeit der Zeit analysiert und gemeldet wird.

In der Ansicht unten wird beispielsweise die Echtzeitintegrität von SQL-Datenbankmodulen angezeigt, die mit SCOM überwacht werden.  Die Integrität der einzelnen Datenbanken für ein Datenbankmodul ist hier in der unteren Hälfte der Ansicht dargestellt.

![Zustandsansicht](media/operations-management-suite-monitoring-product-comparison/scom-state-view.png)

Der Integritäts-Explorer für ein Datenbankmodul ist unten mit den Überwachungseinheiten („Monitors“) dargestellt, die zum Bestimmen der allgemeinen Integrität verwendet werden.  Diese Überwachungseinheiten werden im SQL Management Pack definiert und für alle SQL-Datenbankmodule ausgeführt, die von SCOM erkannt werden.

![Integritäts-Explorer](media/operations-management-suite-monitoring-product-comparison/scom-health-explorer.png)

Komponenten auf mehreren Systemen können kombiniert werden, um die Integrität einer verteilten Anwendung zu messen.  Dies kann besonders für Branchenanwendungen nützlich sein, die mehrere verteilte Komponenten enthalten.  Sie können ein Modell erstellen, mit dem die Integrität jeder Komponente und somit die Gesamtverfügbarkeit der Anwendung gemessen wird.

Active Directory ist ein Beispiel für ein Management Pack, mit dem ein Modell zum Analysieren der verteilten Komponenten bereitgestellt wird.  Im folgenden Beispieldiagramm sind die Integrität der Gesamtumgebung und die Beziehung zwischen Gesamtstrukturen, Domänen und Domänencontrollern dargestellt.  Jede dieser Komponenten hat Unterkomponenten und mehrere Überwachungseinheiten, die dem obigen SQL-Beispiel ähneln.

![SCOM-Diagrammansicht](media/operations-management-suite-monitoring-product-comparison/scom-diagram-view.png)

### <a name="log-analytics"></a>Log Analytics
Die OMS enthält kein allgemeines Modul zum Modellieren von Anwendungen oder Messen der Echtzeitintegrität.  Einzelne Lösungen können die Gesamtintegrität bestimmter Dienste anhand von gesammelten Daten bewerten und eine benutzerdefinierte Logik zum Durchführen von Echtzeitanalysen auf dem Agent installieren.  Da Lösungen in der Cloud mit Zugriff auf das OMS-Repository ausgeführt werden, ermöglichen sie häufig eine eingehendere Analyse als mit Management Packs. 

Mit [AD Assessment- und SQL Assessment-Lösungen](https://technet.microsoft.com/library/mt484102.aspx) werden erfasste Daten analysiert, und es wird eine Bewertung für verschiedene Aspekte der Umgebung erstellt.  Dies umfasst auch Empfehlungen für Verbesserungen, die vorgenommen werden können, um die Verfügbarkeit und Leistung der Umgebung zu verbessern.

![AD Assessment-Lösung](media/operations-management-suite-monitoring-product-comparison/log-analytics-ad-assessment.png)

## <a name="data-analysis"></a>Datenanalyse
SCOM und Log Analytics verfügen jeweils über unterschiedliche Features zum Analysieren erfasster Daten.  SCOM enthält Ansichten und Dashboards in der Betriebskonsole zum Analysieren von kürzlich erstellten Daten in vielen verschiedenen Formaten und Berichte zum Darstellen von Daten aus dem Data Warehouse in Tabellenform.  Mit Log Analytics wird eine vollständige Sprache und Schnittstelle für Protokollabfragen bereitgestellt, um Daten im OMS-Repository zu analysieren.  Wenn SCOM als Datenquelle für Log Analytics verwendet wird, enthält das Repository von SCOM erfasste Daten, sodass mit den Log Analytics-Tools Daten aus beiden Systemen analysiert werden können.

### <a name="operations-manager"></a>Operations Manager

#### <a name="views"></a>Ansichten
Mit den Ansichten in der Betriebskonsole können Sie unterschiedliche Datentypen, die von SCOM erfasst wurden, in unterschiedlichen Formaten anzeigen. Dies sind normalerweise Tabellen für Ereignisse, Warnungen und Zustandsdaten und Liniendiagramme für Leistungsdaten.  Über Ansichten wird nur eine minimale Analyse oder Konsolidierung der Daten durchgeführt, aber Sie können Daten nach bestimmten Kriterien filtern. 

![Ansichten](media/operations-management-suite-monitoring-product-comparison/scom-views.png)

Management Packs stellen in der Regel mehrere Ansichten zur Unterstützung der überwachten Anwendung bzw. des Systems bereit.  Dies können Zustandsansichten für die verschiedenen Objekte sein, die vom Management Pack ermittelt werden, Ansichten mit Warnungen zu erkannten Problemen und Ansichten für Leistungsindikatoren.

Ansichten sind besonders gut für die Analyse des aktuellen Zustands der Umgebung geeignet, z.B. offene Warnungen und der Integritätszustand der überwachten Systeme und Objekte.  Sie können auf ausführliche Ereignis- oder Leistungsdaten zu einer bestimmten Warnung zugreifen, um eine Diagnose zur zugrunde liegenden Ursache zu stellen. Außerdem können Sie die Leistung und Integrität verschiedener Komponenten einer Anwendung anzeigen, um die aktuelle Integrität zu bewerten.

#### <a name="dashboards"></a>Dashboards
Für Dashboards in der Betriebskonsole werden meist die gleichen Daten wie für Ansichten verwendet, aber sie sind besser anpassbar und können umfassendere Visualisierungen enthalten.  Es sind standardmäßige Dashboards verfügbar, die Sie leicht für Ihre eigenen Zwecke anpassen können.  Sie können auch ein PowerShell-Widget verwenden, mit dem von einer PowerShell-Abfrage zurückgegebene Daten angezeigt werden können.

![Dashboard](media/operations-management-suite-monitoring-product-comparison/scom-dashboard.png)

Entwickler haben die Möglichkeit, den Dashboards, die in den Management Packs enthalten sind, benutzerdefinierte Komponenten hinzuzufügen.  Diese können speziell für eine bestimmte Anwendung gelten, z.B. das Dashboard im SQL Management Pack, wie unten dargestellt.  Dieses Dashboard kann auch als Vorlage für benutzerdefinierte Versionen verwendet werden.

![SQL-Dashboard](media/operations-management-suite-monitoring-product-comparison/scom-sql-dashboard.png)

#### <a name="reports"></a>Berichte
Mit Berichten in SCOM werden Daten aus dem Data Warehouse in Tabellenform analysiert.  Sie können gedruckt und für die automatisierte Bereitstellung in unterschiedlichen Dateiformaten, z.B. PDF, CSV und Word, eingeplant werden.  Da für Berichte Daten aus dem Data Warehouse verwendet werden, sind sie besonders für die Analyse von langfristigen Trends geeignet.

Management Packs stellen normalerweise benutzerdefinierte Berichte für eine bestimmte Anwendung bereit.  Sie können auch eine Bibliothek mit generischen Berichten auswählen, die Sie für Ihre eigenen Anwendungen oder für die Durchführung von Ad-hoc-Analysen anpassen können.

Unten ist ein Beispiel für einen Leistungsbericht mit Daten angegeben, die mit dem Active Directory Management Pack erfasst wurden.

![Bericht](media/operations-management-suite-monitoring-product-comparison/scom-report.png)

### <a name="log-analytics"></a>Log Analytics
Log Analytics verfügt über eine [Abfragesprache](https://technet.microsoft.com/library/mt484120.aspx) , mit der Sie Analysen für die Daten mehrerer Anwendungen durchführen können, ohne eine benutzerdefinierte Ansicht oder einen Bericht erstellen zu müssen.  Da OMS in die Cloud implementiert ist, unterliegt die Leistung von Abfragen und der Datenanalyse keinen Hardwarebeschränkungen, sodass Abfragen für Millionen von Datensätzen schnell analysiert werden können. 

Abfragen in Log Analytics stellen auch die Grundlage für andere Funktionen dar.  Sie können eine Abfrage speichern, ihre Ergebnisse in Excel exportieren oder eine automatische regelmäßige Speicherung durchführen und eine Warnung generieren, wenn die Ergebnisse bestimmte Kriterien erfüllen.  

![Datenfluss von Protokollabfragen](media/operations-management-suite-monitoring-product-comparison/log-analytics-query-flow.png)

Unten ist ein Beispiel für eine Log Analytics-Abfrage angegeben.  In diesem Beispiel werden alle Ereignisse mit „started“ im Namen zurückgegeben und nach Ereignis-ID gruppiert.  Der Benutzer stellt einfach die Abfrage bereit, und Log Analytics generiert dann dynamisch die Benutzeroberfläche zum Durchführen der Analyse.  Wenn in der Liste ein Eintrag ausgewählt wird, werden die ausführlichen Ereignisdaten zurückgegeben.

![Protokollabfrage](media/operations-management-suite-monitoring-product-comparison/log-analytics-query.png)

Abfragen in Log Analytics ermöglichen nicht nur Ad-hoc-Analysen, sondern können auch zur späteren Nutzung gespeichert und dem [OMS-Dashboard](http://technet.microsoft.com/library/mt484090.aspx) hinzugefügt werden. Dies ist im folgenden Beispiel dargestellt.

![OMS-Dashboard](media/operations-management-suite-monitoring-product-comparison/log-analytics-dashboard.png)

## <a name="next-steps"></a>Nächste Schritte

- Stellen Sie [System Center Operations Manager (SCOM)](https://technet.microsoft.com/library/hh205987.aspx)bereit.
- Registrieren Sie sich für [Log Analytics](https://azure.microsoft.com/documentation/services/log-analytics).  



<!--HONumber=Oct16_HO2-->



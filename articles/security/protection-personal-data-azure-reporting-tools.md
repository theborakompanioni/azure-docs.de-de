---
title: Dokumentschutz personenbezogener Daten mit Azure-Berichtstools | Microsoft-Dokumentation
description: "Verwenden von Berichtsdiensten und -technologien von Azure für den Datenschutz personenbezogener Daten."
services: security
documentationcenter: na
author: barclayn
manager: MBaldwin
editor: TomSh
ms.assetid: 
ms.service: security
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/24/2017
ms.author: barclayn
ms.translationtype: HT
ms.sourcegitcommit: 1c730c65194e169121e3ad1d1423963ee3ced8da
ms.openlocfilehash: 0ec9ceb63c3e1872e9815a7895b624276fc46123
ms.contentlocale: de-de
ms.lasthandoff: 08/30/2017

---
# <a name="document-protection-of-personal-data-with-azure-reporting-tools"></a>Dokumentschutz personenbezogener Daten mit Azure-Berichtstools

In diesem Artikel erfahren Sie, wie mithilfe von Berichtsdiensten und -technologien von Azure der Datenschutz personenbezogener Daten sichergestellt werden kann.

## <a name="scenario"></a>Szenario

Ein großes Kreuzfahrtunternehmen mit Hauptsitz in den USA expandiert und bietet Routen im Mittelmeer, der Adria und der Ostsee sowie zu den Britischen Inseln an. Zu diesem Zweck hat das Unternehmen mehrere kleinere Kreuzfahrtunternehmen in Italien, Deutschland, Dänemark und Großbritannien erworben.

Das Unternehmen verwendet Microsoft Azure zum Verarbeiten und Speichern von Unternehmensdaten. Dies umfasst personenbezogene Informationen, z.B. Namen, Adressen, Telefonnummern und Kreditkarteninformationen, in seiner globalen Kundendatenbank. Dies umfasst außerdem herkömmliche Informationen der Personalverwaltung wie Adressen, Telefonnummern, Steueridentifikationsnummern und andere Informationen zu Unternehmensmitarbeitern an allen Standorten. Das Kreuzfahrtunternehmen verwaltet zudem eine umfangreiche Datenbank für Mitglieder des Bonus- und Treueprogramms, die personenbezogene Informationen enthält, um Beziehungen zu aktuellen und früheren Kunden nachzuverfolgen.

Unternehmensmitarbeiter greifen in den externen Geschäftsstellen des Unternehmens auf das Netzwerk zu. Und Reiseveranstalter weltweit haben Zugriff auf bestimmte Unternehmensressourcen.

## <a name="problem-statement"></a>Problembeschreibung

Das Unternehmen muss den Datenschutz der personenbezogenen Daten von Kunden und Mitarbeitern durch eine mehrschichtige Sicherheitsstrategie gewährleisten, die mithilfe von Verwaltungs- und Sicherheitsfunktionen von Azure eine strikte Kontrolle des Zugriffs auf die und der Verarbeitung der personenbezogenen Daten vorgibt. Das Unternehmen muss außerdem seine Schutzmaßnahmen internen und externen Prüfern darlegen können.

## <a name="company-goal"></a>Unternehmensziel

Als Teil seiner Defense-in-Depth-Sicherheitsstrategie besteht das Unternehmensziel darin, den gesamten Zugriff auf die und die Verarbeitung der personenbezogenen Daten nachzuverfolgen sowie sicherzustellen, dass die Dokumentation des angemessenen Datenschutzes personenbezogener Daten festgelegt und funktionsfähig ist.

## <a name="solutions"></a>Lösungen

Microsoft Azure bietet umfassende Überwachungs-, Protokollierungs- und Diagnosetools zum Nachverfolgen und Aufzeichnen von Aktivitäten und Ereignissen, die mit dem Zugriff auf und der Verarbeitung von personenbezogenen Daten, dem geografischen Datenfluss sowie dem Zugriff von Drittanbietern auf personenbezogene Daten verbunden sind. Da die Sicherheit der personenbezogenen Daten in der Cloud eine gemeinsame Verantwortung darstellt, stellt Microsoft Kunden zudem Folgendes bereit:

- Detaillierte Informationen zu seiner eigenen Verarbeitung von Kundendaten

- Sicherheitsmaßnahmen, die von Microsoft verwaltet werden

- Informationen dazu, wohin und wie Kundendaten gesendet werden

- Details zum Microsoft-Prozess zur Überprüfung des Datenschutzes

### <a name="azure-active-directory"></a>Azure Active Directory

[Azure Active Directory](https://azure.microsoft.com/services/active-directory/) ist der mehrinstanzenfähige cloudbasierte Verzeichnis- und Identitätsverwaltungsdienst von Microsoft. Über die Berichtsfunktionen des Diensts zur Anmeldung und Überwachung erhalten Sie detaillierte Aktivitätsinformationen zur Anmeldung und Anwendungsnutzung, sodass Sie den entsprechenden Zugriff auf personenbezogene Daten von Kunden und Mitarbeitern überwachen und sicherstellen können.

Es gibt zwei Arten von Aktivitätsberichten:

- Die [Berichte/Protokolle zu Überwachungsaktivitäten](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs) enthalten einen detaillierten Datensatz zu Systemaktivitäten und Systemaufgaben.

- Im [Bericht/Protokoll zu Anmeldeaktivitäten](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-sign-ins) wird angezeigt, wer die einzelnen im Überwachungsbericht aufgeführten Aktivitäten durchgeführt hat.

Durch Verwendung beider Berichte zusammen können Sie den Verlauf jeder durchgeführten Aufgabe sowie den jeweiligen Benutzer, der sie durchgeführt hat, nachverfolgen. Beide Berichtstypen lassen sich anpassen und können gefiltert werden.

#### <a name="how-do-i-access-the-audit-and-security-logs"></a>Wie greife ich auf die Überwachungs- und Sicherheitsprotokolle zu?

Auf die Überwachungs- und Sicherheitsprotokolle kann im Active Directory-Portal auf drei verschiedene Arten zugegriffen werden: über den Abschnitt **Aktivität** (wählen Sie entweder **Überwachungsprotokolle** oder **Anmeldungen** aus) oder über **Benutzer und Gruppen** oder **Unternehmensanwendungen** unter **Verwalten** in Active Directory. Auf Berichte kann auch über die Azure Active Directory- Berichterstellungs-API zugegriffen werden. 

1. Wählen Sie im Azure-Portal die Option **Azure Active Directory** aus.

2. Wählen Sie im Abschnitt **Aktivität** die Option **Überwachungsprotokolle** aus.

    ![](media/protection-personal-data-azure-reporting-tools/image001.png)

3. Sie können die Listenansicht anpassen, indem Sie auf der Symbolleiste auf **Spalten** klicken.

4.  Wählen Sie einen Eintrag in der Listenansicht aus, um alle verfügbaren Details dazu anzuzeigen.

    ![](media/protection-personal-data-azure-reporting-tools/image003.png)

Die Azure Active Directory-Berichterstellung beinhaltet zudem zwei Arten von Sicherheitsberichten: **Benutzer mit Risikomarkierung** und **Riskante Anmeldungen**, mit denen Sie potenzielle Risiken in Ihrer Azure-Umgebung überwachen können.

Weitere Informationen zum Berichterstellungsdienst finden Sie unter [Azure Active Directory-Berichterstellung](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-azure-portal).

Unter [Azure Active Directory-Aktivitätsberichte](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-azure-portal#activity-reports) finden Sie weitere Einzelheiten zu den in Azure Active Directory verfügbaren Berichten. Diese Website enthält weitere ausführliche Informationen zum Zugreifen auf und Verwenden von [Aktivitätsberichten „Überwachungsprotokolle“](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-audit-logs) und [Aktivitätsberichten „Anmeldungen“](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-activity-sign-ins) im Portal. Sie enthält zudem Informationen zu den Sicherheitsberichten des Typs [Benutzer mit Risikomarkierung](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-security-user-at-risk) und [Riskante Anmeldungen](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-security-risky-sign-ins).

Auf der Website [Referenz zur Azure Active Directory-Überwachungs-API](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-audit-reference) finden Sie weitere Informationen zum programmgesteuerten Herstellen einer Verbindung mit der Azure Directory-Berichterstellung.

### <a name="log-analytics"></a>Log Analytics

[Log Analytics](https://azure.microsoft.com/services/log-analytics/) kann [Daten von Azure Monitor erfassen](https://docs.microsoft.com/en-us/azure/log-analytics/log-analytics-azure-storage), mit anderen Daten abgleichen und zusätzliche Analysen bereitstellen. Azure Monitor sammelt und analysiert Überwachungsdaten für Ihre Azure-Umgebung. 

Analysetools in Log Analytics (wie Protokollsuche, Ansichten und Lösungen) berücksichtigen alle gesammelten Daten und bieten somit eine zentrale Analyse Ihrer gesamten Umgebung. Log Analytics kann Windows-Ereignisprotokolle, IIS-Protokolle und Syslog-Protokolle aggregieren und analysieren. Dies kann bei der Erkennung potenzieller Sicherheitsverletzungen helfen, durch die personenbezogene Daten für nicht autorisierte Benutzer offengelegt werden könnten.

#### <a name="how-do-i-use-log-analytics"></a>Wie verwende ich Log Analytics?

Auf Log Analytics können Sie über das OMS-Portal oder das Azure-Portal in jedem Webbrowser zugreifen. Log Analytics bietet eine Abfragesprache, mit der Sie Daten schnell abrufen und im Repository konsolidieren können. Sie können Protokollsuchvorgänge erstellen und speichern, um Daten direkt im Portal zu analysieren.

Führen Sie folgende Schritte aus, um einen Log Analytics-Arbeitsbereich im Azure-Portal zu erstellen:

1. Wählen Sie in der Liste der Dienste im Marketplace die Option **Log Analytics** aus.

2. Wählen Sie **Erstellen** aus. Geben Sie dann den Namen des OMS-Arbeitsbereichs ein, und wählen Sie Ihr Abonnement, die Ressourcengruppe, den Standort und den Tarif aus.

3. Klicken Sie auf **OK**, um eine Liste mit Ihren Arbeitsbereichen anzuzeigen.

4. Wählen Sie einen Arbeitsbereich aus, um dessen Details anzuzeigen.

    ![](media/protection-personal-data-azure-reporting-tools/image004.png)

In der [Dokumentation zu Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview) finden Sie weitere Informationen zu diesem Dienst.

Im Tutorial [Erste Schritte mit einem Log Analytics-Arbeitsbereich](https://docs.microsoft.com/azure/log-analytics/log-analytics-get-started) finden Sie Informationen zum Erstellen eines Auswertungsarbeitsbereichs und zu den Grundlagen der Verwendung des Diensts.

Spezifischere Informationen zum Herstellen einer Verbindung zur Verwendung von Log Analytics mit den oben beschriebenen Protokollen finden Sie auf folgenden Webseiten:

[Datenquellen für Windows-Ereignisprotokolle in Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-windows-events)

[IIS-Protokolle in Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-iis-logs)

[Syslog-Datenquellen in Log Analytics](https://docs.microsoft.com/azure/log-analytics/log-analytics-data-sources-syslog)

### <a name="azure-monitorazure-activity-log"></a>Azure Monitor/Azure-Aktivitätsprotokoll 

[Azure Monitor](https://azure.microsoft.com/services/monitor/) stellt grundlegende Infrastrukturmetriken und Protokolle für die meisten Dienste in Microsoft Azure bereit.
Durch Überwachung können Sie umfassende Erkenntnisse über Ihre Azure-Anwendungen gewinnen. Azure Monitor greift auf die Azure-Diagnoseerweiterung (Windows oder Linux) zurück, um die meisten Metriken und Protokolle auf Anwendungsebene zu sammeln. Das [Azure-Aktivitätsprotokoll](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview-activity-logs) ist eine der Ressourcen, die Sie mithilfe von Azure Monitor anzeigen können. Es verfolgt alle API-Aufrufe und enthält eine Fülle von Informationen zu Aktivitäten, die in [Azure Resource Manager](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview) erfolgen.
Sie können das Aktivitätsprotokoll (zuvor als Betriebs- oder Überwachungsprotokoll bezeichnet) nach Informationen zu Ihrer Ressource aus Sicht der Azure-Infrastruktur durchsuchen. 

Obwohl sich der Großteil der Informationen im Aktivitätsprotokoll auf Leistung und Dienstintegrität bezieht, enthält es auch Informationen im Hinblick auf den Schutz von Daten. Mit dem Aktivitätsprotokoll können Sie die Antworten auf die Fragen „Was“, „Wer“ und „Wann“ für alle Schreibvorgänge (PUT, POST, DELETE) ermitteln, die für die Ressourcen Ihres Azure-Abonnements durchgeführt wurden.

Es enthält beispielsweise einen Datensatz, wenn ein Administrator eine Netzwerksicherheitsgruppe löscht, die den Schutz personenbezogener Daten beeinträchtigen kann. Aktivitätsprotokolleinträge werden 90 Tage lang in Azure Monitor gespeichert.

#### <a name="how-do-i-use-the-data-collected-by-azure-monitor"></a>Wie verwende ich die von Azure Monitor gesammelten Daten?

Die Daten im Aktivitätsprotokoll und andere Ressourcen von Azure Monitor können auf verschiedene Weise verwendet werden.

- Sie können die Daten in Echtzeit an andere Speicherorte streamen.

- Sie können die Daten über einen längeren Zeitraum als durch den Standardwert vorgegeben speichern, indem Sie ein [Azure Storage-Konto](https://docs.microsoft.com/azure/storage/common/storage-introduction) verwenden und eine Aufbewahrungsrichtlinie festlegen.

- Sie können die Daten im [Azure-Portal](https://azure.microsoft.com/features/azure-portal/), mithilfe von [Azure Application Insights](https://azure.microsoft.com/services/application-insights/), [Microsoft Power BI](https://powerbi.microsoft.com/) oder Visualisierungstools von Drittanbietern in Grafiken und Diagrammen visualisieren.

- Sie können die Daten mithilfe der Azure Monitor-REST-API, mit CLI-Befehlen, mit [PowerShell](https://docs.microsoft.com/powershell/)-Cmdlets oder dem .NET SDK abfragen.

Wählen Sie zum Einstieg in Azure Monitor die Option **Weitere Dienste** im Azure-Portal aus.

1. Scrollen Sie im Abschnitt **Überwachung und Verwaltung** nach unten zu **Monitor**.

    ![](media/protection-personal-data-azure-reporting-tools/image005.png)

2.  Monitor wird in der Ansicht **Aktivitätsprotokoll** angezeigt.

    ![](media/protection-personal-data-azure-reporting-tools/image007.png)

Sie können Abfragen für allgemeine Filter erstellen und speichern und dann die wichtigsten Abfragen an ein Portaldashboard anheften. So können Sie jederzeit sehen, ob Ereignisse aufgetreten sind, die Ihre Kriterien erfüllen.

1. Sie können die Ansicht nach Ressourcengruppe, Zeitraum und Ereigniskategorie filtern.

    ![](media/protection-personal-data-azure-reporting-tools/image008.png)

2. Sie können dann Abfragen an ein Portaldashboard anheften, indem Sie auf die Schaltfläche **Anheften** klicken. So können Sie eine einzige Informationsquelle für operative Daten von Ihren Diensten erstellen. Der Abfragename und etliche Ergebnisse werden im Dashboard angezeigt.

Mit Monitor können Sie zudem Metriken für alle Azure-Ressourcen anzeigen, Diagnoseeinstellungen und Warnungen konfigurieren und das Protokoll durchsuchen. Weitere Informationen zum Verwenden von Azure Monitor und des Aktivitätsprotokolls finden Sie unter [Erste Schritte mit Azure Monitor](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-get-started).

### <a name="azure-diagnostics"></a>Azure-Diagnose 

Mit der Diagnosefunktion in Azure können Daten aus mehreren Quellen gesammelt werden. Die Windows-Ereignisprotokolle, die das Sicherheitsprotokoll enthalten, können besonders hilfreich beim Nachverfolgen und Dokumentieren des Schutzes personenbezogener Daten sein. Mit dem Sicherheitsprotokoll werden erfolgreiche und fehlerhafte Anmeldeereignisse sowie Berechtigungsänderungen, die Erkennung von Mustern für bestimmte Angriffstypen, Änderungen an sicherheitsbezogenen Richtlinien, Änderungen an der Sicherheitsgruppenmitgliedschaft und vieles mehr nachverfolgt.

Beispielsweise warnt Ereignis-ID 4695 vor der versuchten Aufhebung des Schutzes von zu überwachenden geschützten Daten. Dies betrifft die Data Protection-API (DPAPI), mit der Daten wie private Schlüssel gespeicherte Anmeldeinformationen und andere vertrauliche Informationen geschützt werden können.

#### <a name="how-do-i-enable-the-diagnostics-extension-for-windows-vms"></a>Wie aktiviere ich die Diagnoseerweiterung für virtuelle Windows-Computer?

Die Diagnoseerweiterung für einen virtuellen Windows-Computer können Sie mithilfe von PowerShell aktivieren, um Protokolldaten zu sammeln. Die hierfür erforderlichen Schritte hängen vom jeweils verwendeten Bereitstellungsmodell (Resource Manager-Modell oder klassisches Modell) ab. Wenn Sie die Diagnoseerweiterung auf einem bereits vorhandenen virtuellen Computer aktivieren möchten, der mithilfe des Resource Manager-Bereitstellungsmodells erstellt wurde, können Sie das PowerShell-Cmdlet [Set-AzureRMVMDiagnosticsExtension](https://docs.microsoft.com/powershell/module/azurerm.compute/set-azurermvmdiagnosticsextension?view=azurermps-4.3.1) verwenden.

   ![](media/protection-personal-data-azure-reporting-tools/image009.png)

*\$$diagnosticsconfig_path* ist der Pfad zu der Datei mit der XML-Diagnosekonfiguration. Ausführlichere Anweisungen zum Aktivieren der Azure-Diagnose für einen virtuellen Computer finden Sie unter [Aktivieren der Azure-Diagnose auf einer virtuellen Azure-Maschine unter Windows mithilfe von PowerShell](https://docs.microsoft.com/azure/virtual-machines/windows/ps-extensions-diagnostics).

Die Azure-Diagnoseerweiterung kann die gesammelten Daten an ein Azure Storage-Konto übertragen oder an Dienste wie Application Insights senden. Sie können dann die Daten für die Überwachung verwenden.

#### <a name="how-do-i-store-and-view-diagnostic-data"></a>Wie speichere und zeige ich Diagnosedaten an?

Bedenken Sie, dass Diagnosedaten nicht dauerhaft gespeichert werden, wenn sie nicht in den Microsoft Azure-Speicheremulator oder in Azure Storage übertragen werden. Führen Sie folgende Schritte aus, um Diagnosedaten in Azure Storage zu speichern und anzuzeigen:

1. Geben Sie ein Speicherkonto in der Datei „ServiceConfiguration.cscfg“ an. Die Azure-Diagnose kann abhängig vom Datentyp entweder den Blob-Dienst oder den Tabellenspeicherdienst verwenden. Windows-Ereignisprotokolle werden im Tabellenformat gespeichert.

2. Übertragen Sie die Daten. Die Anforderung zum Übertragen der Diagnosedaten kann über die Konfigurationsdatei erfolgen. Bei SDK 2.4 und früher können Sie die Anforderung auch programmgesteuert vornehmen.

3. Sie können die Daten mit dem [Azure-Speicher-Explorer](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer), dem [Server-Explorer](https://docs.microsoft.com/azure/vs-azure-tools-storage-resources-server-explorer-browse-manage) in Visual Studio oder mit [Azure Diagnostics Manager](https://www.cerebrata.com/products/azure-diagnostics-manager) in Azure Management Studio anzeigen.

Weitere Informationen zum Ausführen der einzelnen Schritte finden Sie unter [Speichern und Anzeigen von Diagnosedaten in Azure Storage](https://docs.microsoft.com/azure/cloud-services/cloud-services-dotnet-diagnostics-storage).

### <a name="azure-storage-analytics"></a>Azure-Speicheranalyse 

Die Speicheranalyse protokolliert ausführliche Informationen zu erfolgreichen und nicht erfolgreichen Anforderungen an einen Speicherdienst. Anhand dieser Informationen können einzelne Anforderungen überwacht werden, sodass der Zugriff auf die im Dienst gespeicherten personenbezogenen Daten dokumentiert werden kann. Die Storage Analytics-Protokollierung ist für Ihr Speicherkonto jedoch nicht standardmäßig aktiviert. Sie können sie im Azure-Portal aktivieren.

#### <a name="how-do-i-configure-monitoring-for-a-storage-account"></a>Wie konfiguriere ich die Überwachung für ein Speicherkonto?

Führen Sie folgende Schritte aus, um die Überwachung für ein Speicherkonto zu konfigurieren:

1. Wählen Sie im Azure-Portal die Option **Speicherkonten** und dann den Namen des Kontos aus, das überwacht werden soll.

    ![](media/protection-personal-data-azure-reporting-tools/image011.png)

2. Wählen Sie im Abschnitt **Überwachung** die Option **Diagnose** aus.

3.  Wählen Sie den **Typ** der zu überwachenden Metrikdaten für jeden Dienst (Blob-, Tabellen- und Dateidienst) aus. Um Azure Storage anzuweisen, Diagnoseprotokolle für Lese-, Schreib- und Löschanforderungen für den Blob-, Tabellen- und Warteschlangendienst zu speichern, wählen Sie **Blobprotokolle, Tabellenprotokolle** und **Warteschlangenprotokolle** aus.

    ![](media/protection-personal-data-azure-reporting-tools/image013.png)

4. Legen Sie mit dem Schieberegler unten die Richtlinie für die **Aufbewahrung** in Tagen (1 bis 365) fest. Die Standardeinstellung ist sieben Tage.

5. Wählen Sie **Speichern** aus, um die Konfigurationseinstellungen anzuwenden.

Protokolleinträge der Speicherprotokollierung enthalten die folgenden Informationen zu einzelnen Anforderungen:

- Zeitinformationen, z.B. Startzeit, End-to-End-Latenz und Serverlatenz

- Details zum Speichervorgang, z.B. der Vorgangstyp; der Schlüssel des Speicherobjekts, auf das der Client zugreift; Erfolg oder Fehler und der an den Client zurückgegebene HTTP-Statuscode

- Details zur Authentifizierung, z.B. der vom Client verwendete Authentifizierungstyp

- Parallelitätsinformationen, z.B. der ETag-Wert und der Zeitstempel der letzten Änderung

- Die Größe der Anforderungs- und Antwortnachrichten

Ausführlichere Anweisungen zum Aktivieren der Storage Analytics-Protokollierung finden Sie unter [Überwachen eines Speicherkontos im Azure-Portal](https://docs.microsoft.com/azure/storage/common/storage-monitor-storage-account).

### <a name="azure-security-center"></a>Azure Security Center 

[Azure Security Center](https://azure.microsoft.com/services/security-center/) überwacht den Sicherheitsstatus Ihrer Azure-Ressourcen, um Bedrohungen zu verhindern und zu erkennen sowie Empfehlungen zur entsprechenden Reaktion bereitzustellen. Mit diesem Dienst können Ihre Sicherheitsmaßnahmen zur Gewährleistung des Datenschutzes personenbezogener Daten auf verschiedene Weise dokumentiert werden.

Durch Überwachung der Sicherheitsintegrität können Sie die Einhaltung Ihrer Sicherheitsrichtlinien sicherstellen. Die Sicherheitsüberwachung ist eine proaktive Strategie, bei der Ihre Ressourcen überwacht werden, um Systeme zu erkennen, die nicht den Unternehmensstandards oder bewährten Methoden entsprechen. Sie können den Sicherheitsstatus der folgenden Ressourcen überwachen:

- Compute (virtuelle Computer und Clouddienste)

- Netzwerk (virtuelle Netzwerke)

- Speicher und Daten (Server- und Datenbanküberwachung und Bedrohungserkennung, TDE, Speicherverschlüsselung)

- Anwendungen (potenzielle Sicherheitsprobleme)

Sicherheitsprobleme in einer dieser Kategorien können eine Bedrohung des Datenschutzes personenbezogener Daten darstellen.

#### <a name="how-do-i-view-the-security-state-of-my-azure-resources"></a>Wie zeige ich den Sicherheitsstatus meiner Azure-Ressourcen an?

Security Center analysiert in regelmäßigen Abständen den Sicherheitsstatus der Azure-Ressourcen. Sie können alle erkannten potenziellen Sicherheitsrisiken im Abschnitt **Prävention** des Dashboards anzeigen.

   ![](media/protection-personal-data-azure-reporting-tools/image014.png)

1. Wählen Sie im Abschnitt **Prävention** die Kachel **Compute** aus. Hier wird eine **Übersicht** zusammen mit der Liste aller **virtuellen Computer** mit dem jeweiligen Sicherheitsstatus sowie die Liste **Clouddienste** der von Security Center überwachten Web- und Workerrollen angezeigt.

2. Wählen Sie auf der Registerkarte **Übersicht** eine Empfehlung aus, um weitere Informationen anzuzeigen.

3. Wählen Sie auf der Registerkarte **Virtuelle Computer** einen virtuellen Computer aus, um zusätzliche Details anzuzeigen.

Wenn die Datensammlung in Azure Security Center aktiviert ist, wird der Microsoft Monitoring Agent automatisch auf allen vorhandenen und neuen unterstützten virtuellen Computern bereitgestellt, die bereitgestellt werden. Die von diesem Agent gesammelten Daten werden entweder in einem vorhandenen [Log Analytics](https://azure.microsoft.com/services/log-analytics/)-Arbeitsbereich, der mit Ihrem Abonnement verknüpft ist, oder in einem neuen Arbeitsbereich gespeichert.

In Security Center werden [Berichte mit Bedrohungsinformationen](https://docs.microsoft.com/azure/security-center/security-center-threat-report) bereitgestellt. Diese Berichte enthalten nützliche Informationen zum Erkennen der Identität von Angreifern, der Ziele, der aktuellen und früheren Angriffskampagnen sowie zu verwendeten Taktiken, Tools und Prozeduren. Zudem sind Informationen zu Risikominderung und Beseitigung enthalten.

Der Hauptzweck dieser Berichte mit Bedrohungsinformationen besteht darin, dass Sie effektiv auf die unmittelbare Bedrohung reagieren und anschließend Maßnahmen ergreifen können, um das Problem zu minimieren. Die Informationen in den Berichten können zudem nützlich sein, wenn Sie die Reaktion auf einen Incident zu Berichts- und Überwachungszwecken dokumentieren.

Die Berichte mit Bedrohungsinformationen werden im PDF-Format angezeigt und lassen sich über einen Link im Feld **Berichte** des Blatts **Ausgeführter verdächtiger Prozess** für die einzelnen Sicherheitswarnungen in Azure Security Center aufrufen.

Weitere Informationen zum Anzeigen und Verwenden des Berichts mit Bedrohungsinformationen finden Sie unter [Azure Security Center-Bericht mit Bedrohungsinformationen](https://docs.microsoft.com/azure/security-center/security-center-threat-report).

## <a name="next-steps"></a>Nächste Schritte:

[Erste Schritte mit der Berichterstellungs-API von Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-reporting-api-getting-started-azure-portal)

[Was ist Log Analytics?](https://docs.microsoft.com/azure/log-analytics/log-analytics-overview)

[Überblick über die Überwachung in Microsoft Azure](https://docs.microsoft.com/azure/monitoring-and-diagnostics/monitoring-overview)

[Introduction to the Azure Activity Log](https://azure.microsoft.com/resources/videos/intro-activity-log/) (Video, Einführung in das Azure-Aktivitätsprotokoll)


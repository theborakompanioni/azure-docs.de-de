<properties
   pageTitle="Azure Security Center – Handbuch zur Problembehandlung | Microsoft Azure"
   description="In diesem Dokument wird die Problembehandlung in Azure Security Center beschrieben."
   services="security-center"
   documentationCenter="na"
   authors="YuriDio"
   manager="swadhwa"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="hero-article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="07/08/2016"
   ms.author="yurid"/>

# Azure Security Center – Handbuch zur Problembehandlung
Dieses Handbuch ist für IT-Experten, IT-Architekten, Informationssicherheitsanalysten und Cloudadministratoren konzipiert, deren Organisation Azure Security Center verwendet.

> [AZURE.NOTE] Die Informationen in diesem Dokument gelten für die Vorschauversion von Azure Security Center.

## Was ist Azure Security Center?
Azure Security Center unterstützt Sie beim Verhindern, Erkennen und Beheben von Bedrohungen durch größere Transparenz und bessere Kontrolle über die Sicherheit Ihrer Azure-Ressourcen. Es bietet integrierte Sicherheitsüberwachung und Richtlinienverwaltung für Ihre Abonnements, hilft bei der Erkennung von Bedrohungen, die andernfalls möglicherweise unbemerkt bleiben, und kann gemeinsam mit einem breiten Sektrum an Sicherheitslösungen verwendet werden.

## Handbuch zur Problembehandlung
In diesem Handbuch wird beschrieben, wie Sie Probleme mit Azure Security Center beheben. Der Großteil der Problembehandlung in Azure Security Center erfolgt, indem zuerst die Einträge im [Überwachungsprotokoll](https://azure.microsoft.com/updates/audit-logs-in-azure-preview-portal/) für die fehlerhafte Komponente geprüft werden. Über Überwachungsprotokolle können Sie Folgendes ermitteln:

- Welche Vorgänge durchgeführt werden
- Wer den Vorgang initiiert hat
- Wann der Vorgang durchgeführt wurde
- Status des Vorgangs
- Werte anderer Eigenschaften, mit denen Sie den Vorgang eventuell untersuchen können

Das Überwachungsprotokoll enthält alle Schreibvorgänge (PUT, POST, DELETE), die für Ihre Ressourcen durchgeführt werden, aber keine Lesevorgänge (GET).

## Installation des Überwachungs-Agents für die Problembehandlung in Windows

Der Überwachungs-Agent von Security Center wird zum Durchführen der Datensammlung verwendet. Nachdem die Datensammlung aktiviert und der Agent auf dem Zielcomputer richtig installiert wurde, sollten diese Prozesse ausgeführt werden:

- ASMAgentLauncher.exe – Azure-Überwachungs-Agent
- ASMMonitoringAgent.exe – Azure-Erweiterung für die Sicherheitsüberwachung
- ASMSoftwareScanner.exe – Azure-Scan-Manager

Die Azure-Erweiterung für Sicherheitsüberwachung sucht nach verschiedenen sicherheitsrelevanten Konfigurationen und erfasst Sicherheitsprotokolle des virtuellen Computers. Der Scan-Manager wird als Patch-Scanner verwendet.

Wenn die Installation erfolgreich durchgeführt wurde, sollte in den Überwachungsprotokollen für den virtuellen Zielcomputer ein Eintrag der folgenden Art angezeigt werden:

![Überwachungsprotokolle](./media/security-center-troubleshooting-guide/security-center-troubleshooting-guide-fig1.png)

Sie können auch weitere Informationen zum Installationsvorgang abrufen, indem Sie die Agent-Protokolle unter *%Systemlaufwerk%\\windowsazure\\logs* (Beispiel: C:\\WindowsAzure\\Logs) lesen.

[AZURE.NOTE] Wenn sich der Azure Security Center-Agent nicht richtig verhält, müssen Sie den virtuellen Zielcomputer neu starten, da kein Befehl zum Beenden und Starten des Agents vorhanden ist.

## Installation des Überwachungs-Agents für die Problembehandlung in Linux
Wenn Sie die Problembehandlung für eine VM-Agent-Installation auf einem Linux-System durchführen, sollten Sie sicherstellen, dass die Erweiterung nach „/var/lib/waagent/“ heruntergeladen wurde. Sie können den folgenden Befehl ausführen, um zu überprüfen, ob die Installation erfolgreich war:

`cat /var/log/waagent.log`

Die anderen Protokolldateien, die Sie im Rahmen der Problembehandlung prüfen können, sind:

- /var/log/mdsd.err
- /var/log/azure/

In einem funktionierenden System sollte eine Verbindung mit dem Prozess „mdsd“ unter TCP 29130 angezeigt werden. Dies ist das Syslog, das mit dem Prozess „mdsd“ kommuniziert. Sie können dieses Verhalten überprüfen, indem Sie den folgenden Befehl ausführen:

`netstat -plantu | grep 29130`

## Kontaktaufnahme mit dem Microsoft-Support

Einige Probleme können mit den Richtlinien in diesem Artikel identifiziert werden, während andere im öffentlichen Security Center-[Forum](https://social.msdn.microsoft.com/Forums/de-DE/home?forum=AzureSecurityCenter) dokumentiert sind. Falls Sie weitere Hilfe zur Problembehandlung benötigen, können Sie wie unten gezeigt über das Azure-Portal eine neue Supportanfrage erstellen:

![Microsoft-Support](./media/security-center-troubleshooting-guide/security-center-troubleshooting-guide-fig2.png)


## Nächste Schritte

In diesem Dokument haben Sie erfahren, wie Sie Sicherheitsrichtlinien in Azure Security Center konfigurieren können. Weitere Informationen zu Azure Security Center finden Sie in den folgenden Quellen:

- [Planungs- und Betriebshandbuch für Azure Security Center](security-center-planning-and-operations-guide.md) – Erfahren Sie, wie Sie Entwurfsüberlegungen zur Einführung von Azure Security Center planen und umsetzen können.
- [Überwachen der Sicherheitsintegrität in Azure Security Center](security-center-monitoring.md) – Erfahren Sie, wie Sie die Integrität Ihrer Azure-Ressourcen überwachen.
- [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center](security-center-managing-and-responding-alerts.md) – Erfahren Sie, wie Sie Sicherheitswarnungen verwalten und auf diese reagieren.
- [Monitoring partner solutions with Azure Security Center (Überwachen von Partnerlösungen mit Azure Security Center)](security-center-partner-solutions.md): Erfahren Sie, wie der Integritätsstatus Ihrer Partnerlösungen überwacht wird.
- [Häufig gestellte Fragen (FAQ) zu Security Center](security-center-faq.md) – Häufig gestellte Fragen zur Verwendung des Diensts.
- [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) – suchen Sie nach Blogbeiträgen über Azure-Sicherheit und -Compliance.

<!---HONumber=AcomDC_0713_2016-->
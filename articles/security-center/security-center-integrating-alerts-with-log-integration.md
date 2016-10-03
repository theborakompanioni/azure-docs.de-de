<properties
   pageTitle="Integrieren von Azure Security Center-Warnungen mithilfe der Azure-Protokollintegration (Vorschau) | Microsoft Azure"
   description="Dieser Artikel hilft beim Einstieg in die Integration von Security Center-Warnungen mithilfe der Azure-Protokollintegration."
   services="security-center"
   documentationCenter="na"
   authors="TerryLanfear"
   manager="MBaldwin"
   editor=""/>

<tags
   ms.service="security-center"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/08/2016"
   ms.author="terrylan"/>

# Integrieren von Security Center-Warnungen mithilfe der Azure-Protokollintegration (Vorschau)

Viele für den Sicherheitsbetrieb und die Bearbeitung von Incidents zuständige Teams nutzen eine SIEM-Lösung (Security Information and Event Management) als Ausgangspunkt für die Selektierung und Untersuchung von Sicherheitswarnungen. Mit der Azure-Protokollintegration können Kunden Azure Security Center-Warnungen sowie von der Azure-Diagnose und Azure-Überwachungsprotokollen gesammelte Sicherheitsereignisse für den virtuellen Computer nahezu in Echtzeit mit ihrer Protokollanalyse- oder SIEM-Lösung synchronisieren.

Die Azure-Protokollintegration funktioniert mit HP ArcSight, Splunk, IBM QRadar sowie weiteren Lösungen.

## Welche Protokolle kann ich integrieren?

Azure führt für jeden Dienst eine umfassende Protokollierung durch. Diese Protokolle sind wie folgt kategorisiert:

- **Steuerungs-/Verwaltungsprotokolle**, die Einblicke in die CREATE-, UPDATE- und DELETE-Vorgänge in Azure Resource Manager ermöglichen.
- **Datenebenenprotokolle**, die Einblicke in die Ereignisse ermöglichen, die bei Verwendung einer Azure-Ressource eintreten. Ein Beispiel hierfür sind Sicherheits- und Anwendungsprotokolle des Windows-Ereignisprotokolls auf einem virtuellen Computer.

Die Azure-Protokollintegration unterstützt derzeit die Integration von:

- Azure-VM-Protokollen
- Azure-Überwachungsprotokollen
- Azure Security Center-Warnungen

## Installieren der Azure-Protokollintegration

Laden Sie die [Azure-Protokollintegration](https://www.microsoft.com/download/details.aspx?id=53324) herunter.

Der Dienst „Azure-Protokollintegration“ sammelt Telemetriedaten des Computers, auf dem er installiert ist. Folgende Telemetriedaten werden erfasst:

- Ausnahmen, die während der Ausführung der Azure-Protokollintegration auftreten
- Metriken zu der Anzahl von verarbeiteten Abfragen und Ereignissen
- Statistiken zur Verwendung von Azlog.exe-Befehlszeilenoptionen

> [AZURE.NOTE] Sie können die Erfassung von Telemetriedaten ausschalten, indem Sie diese Option deaktivieren.

## Integrieren von Azure-Überwachungsprotokollen und Security Center-Warnungen

1. Öffnen Sie die Eingabeaufforderung, und wechseln Sie mit **cd** zu **c:\\Programme\\Microsoft Azure Log Integration**.

2. Führen Sie den Befehl **azlog createazureid** aus, um unter den Azure Active Directory-Mandanten (AD), die die Azure-Abonnements hosten, einen [Azure Active Directory-Dienstprinzipal](../active-directory/active-directory-application-objects.md) zu erstellen.

    Sie werden aufgefordert, Ihre Azure-Anmeldedaten einzugeben.

    > [AZURE.NOTE] Sie müssen der Besitzer des Abonnements oder ein Co-Administrator des Abonnements sein.

    Die Authentifizierung gegenüber Azure wird über Azure AD durchgeführt. Beim Erstellen eines Dienstprinzipals für die Azure-Protokollintegration wird die Azure AD-Identität erstellt, für die der Zugriff zum Lesen aus Azure-Abonnements gewährt wird.

3. Führen Sie den Befehl **azlog authorize <Abonnement-ID>** aus, um für den in Schritt 2 erstellten Dienstprinzipal den Zugriff „Leser“ für das Abonnement zu gewähren. Wenn Sie keine **Abonnement-ID** angeben, wird dem Dienstprinzipal der Zugriff „Leser“ für alle Abonnements gewährt, auf die Sie Zugriff haben.

    > [AZURE.NOTE] Unter Umständen werden Warnmeldungen angezeigt, wenn Sie den Befehl **authorize** direkt nach dem Befehl **createazureid** ausführen, da zwischen der Erstellung und der Verfügbarkeit des Azure AD-Kontos ein Latenzzeitraum anfällt. Wenn Sie nach dem Ausführen des Befehls **createazureid** ca. zehn Sekunden warten, um den Befehl **authorize** auszuführen, sollten diese Warnungen nicht angezeigt werden.

4. Überprüfen Sie die folgenden Ordner, um sicherzustellen, dass die JSON-Dateien des Überwachungsprotokolls vorhanden sind:

  - **c:\\Users\\azlog\\AzureResourceManagerJson**
  - **c:\\Users\\azlog\\AzureResourceManagerJsonLD**

5. Überprüfen Sie die folgenden Ordner, um sicherzustellen, dass darin bereits Security Center-Warnungen vorhanden sind:

  - **c:\\Users\\azlog\\AzureSecurityCenterJson**
  - **c:\\Users\\azlog\\AzureSecurityCenterJsonLD**

6. Verweisen Sie für den Standardconnector für die SIEM-Dateiweiterleitung auf den richtigen Ordner, um die Daten an die SIEM-Instanz zu übermitteln. Informationen hierzu finden Sie unter [SIEM-Konfigurationen](https://azsiempublicdrops.blob.core.windows.net/drops/ALL.htm) in Ihrer SIEM-Konfiguration.

Wenn Sie Fragen zur Azure-Protokollintegration haben, senden Sie eine E-Mail an [AzSIEMteam@microsoft.com](mailto:AzSIEMteam@microsoft.com)

## Nächste Schritte

Weitere Informationen zu Azure-Überwachungsprotokollen und Definitionen von Eigenschaften finden Sie unter:

- [Überwachen von Vorgängen mit dem Ressourcen-Manager](../resource-group-audit.md)
- [Auflisten der Verwaltungsereignisse in einem Abonnement](https://msdn.microsoft.com/library/azure/dn931934.aspx): Enthält Informationen zum Abrufen von Überwachungsprotokollereignissen.

Weitere Informationen zu Security Center finden Sie in den folgenden Quellen:

- [Verwalten von und Reagieren auf Sicherheitswarnungen in Azure Security Center](security-center-managing-and-responding-alerts.md): Erfahren Sie, wie Sie Sicherheitswarnungen verwalten und darauf reagieren.
- [Azure Security Center – Häufig gestellte Fragen](security-center-faq.md): Hier finden Sie häufig gestellte Fragen zur Verwendung des Diensts.
- [Azure Security Blog](http://blogs.msdn.com/b/azuresecurity/) (Blog zur Azure-Sicherheit): Hier finden Sie Neuigkeiten und Informationen zur Azure-Sicherheit.

<!---HONumber=AcomDC_0921_2016-->
<properties
   pageTitle="Erste Schritte mit der Azure-Protokollintegration | Microsoft Azure"
   description="Hier erfahren Sie, wie Sie den Azure-Protokollintegrationsdienst installieren und Protokolle aus Azure Storage sowie Azure-Überwachungsprotokolle und Azure Security Center-Warnungen integrieren."
   services="security"
   documentationCenter="na"
   authors="TomShinder"
   manager="MBaldwin"
   editor="TerryLanfear"/>

<tags
   ms.service="security"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/24/2016"
   ms.author="TomSh"/>

# Erste Schritte mit der Azure-Protokollintegration (Vorschau)

Mit der Azure-Protokollintegration können Sie nicht aufbereitete Protokolle aus Ihren Azure-Ressourcen in Ihre lokalen SIEM-Systeme (Security Information and Event Management, Sicherheitsinformationen und Ereignisverwaltung) integrieren. Diese Integration bietet ein einheitliches Dashboard für alle Ihre Ressourcen (ganz gleich ob lokal oder in der Cloud), sodass Sie Sicherheitsereignisse im Zusammenhang mit Ihren Anwendungen aggregieren, korrelieren, analysieren und entsprechende Warnungen ausgeben können.

In diesem Tutorial erfahren Sie Schritt für Schritt, wie Sie die Azure-Protokollintegration installieren und Protokolle aus Azure Storage sowie Azure-Überwachungsprotokolle und Azure Security Center-Warnungen integrieren. Die Bearbeitung dieses Tutorials dauert ungefähr eine Stunde.

## Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

- Einen Computer (lokal oder in der Cloud) zum Installieren des Azure-Protokollintegrationsdiensts. Auf diesem Computer müssen ein Windows-Betriebssystem (64 Bit) und .NET 4.5.1 installiert sein. Dieser Computer wird als **Azlog Integrator** bezeichnet.
- Azure-Abonnement. Falls Sie über kein Abonnement verfügen, können Sie sich für ein [kostenloses Konto](https://azure.microsoft.com/free/) registrieren.
- Aktivierte Azure-Diagnose für Ihre virtuellen Azure-Computer. Informationen zum Aktivieren der Diagnose für Cloud Services finden Sie unter [Aktivieren der Azure-Diagnose in Azure Cloud Services](../cloud-services/cloud-services-dotnet-diagnostics.md). Informationen zum Aktivieren der Diagnose für einen virtuellen Azure-Computer unter Windows finden Sie unter [Aktivieren der Azure-Diagnose auf einem virtuellen Azure-Computer unter Windows mithilfe von PowerShell](../virtual-machines/virtual-machines-windows-ps-extensions-diagnostics.md).
- Verbindung zwischen Azlog Integrator und Azure Storage sowie Authentifizierung und Autorisierung gegenüber dem Azure-Abonnement.
- Für Azure-VM-Protokolle muss der SIEM-Agent (beispielsweise Splunk Universal Forwarder, HP ArcSight Windows Event Collector-Agent oder IBM QRadar WinCollect) auf dem Azlog Integrator installiert sein.

## Überlegungen zur Bereitstellung

Bei hohem Ereignisaufkommen können mehrere Instanzen des Azlog Integrators ausgeführt werden. Der Lastenausgleich der Azure-Diagnose-Speicherkonten für Windows *(WAD)* und die Anzahl von Abonnements, die für die Instanzen bereitgestellt werden, müssen auf Ihrer Kapazität basieren.

Auf einem Computer mit acht Prozessoren (Kernen) kann eine einzelne Azlog Integrator-Instanz pro Tag etwa 24 Millionen Ereignisse (rund eine Million pro Stunde) verarbeiten.

Auf einem Computer mit vier Prozessoren (Kernen) kann eine einzelne Azlog Integrator-Instanz pro Tag etwa 1,5 Millionen Ereignisse (rund 62.500 pro Stunde) verarbeiten.

## Installieren der Azure-Protokollintegration

Laden Sie die [Azure-Protokollintegration](https://www.microsoft.com/download/details.aspx?id=53324) herunter.

Der Dienst „Azure-Protokollintegration“ sammelt Telemetriedaten des Computers, auf dem er installiert ist. Folgende Telemetriedaten werden erfasst:

- Ausnahmen, die während der Ausführung der Azure-Protokollintegration auftreten
- Metriken zu der Anzahl von verarbeiteten Abfragen und Ereignissen
- Statistiken zur Verwendung von Azlog.exe-Befehlszeilenoptionen

> [AZURE.NOTE] Sie können die Erfassung von Telemetriedaten ausschalten, indem Sie diese Option deaktivieren.

## Integrieren von Azure-VM-Protokollen aus Ihren Azure-Diagnose-Speicherkonten

1. Überprüfen Sie die oben angegebenen Voraussetzungen, um sicherzustellen, dass Ihr WAD-Speicherkonto Protokolle erfasst, und fahren Sie dann mit der Azure-Protokollintegration fort. Führen Sie die folgenden Schritte nicht aus, wenn Ihr WAD-Speicherkonto keine Protokolle erfasst.

2. Öffnen Sie die Eingabeaufforderung, und wechseln Sie mit **cd** zu **c:\\Programme\\Microsoft Azure Log Integration**.

3. Führen Sie den folgenden Befehl aus:

        azlog source add <FriendlyNameForTheSource> WAD <StorageAccountName> <StorageKey>

      Ersetzen Sie „StorageAccountName“ durch den Namen des Azure-Speicherkontos, das für den Empfang von Diagnoseereignissen Ihres virtuellen Computers konfiguriert ist.

        azlog source add azlogtest WAD azlog9414 fxxxFxxxxxxxxywoEJK2xxxxxxxxxixxxJ+xVJx6m/X5SQDYc4Wpjpli9S9Mm+vXS2RVYtp1mes0t9H5cuqXEw==

      Soll in der Ereignis-XML die Abonnement-ID angezeigt werden, fügen Sie am Ende des Anzeigenamens die Abonnement-ID an:

        azlog source add <FriendlyNameForTheSource>.<SubscriptionID> WAD <StorageAccountName> <StorageKey>

4. Warten Sie 30 bis 60 Minuten (kann bis zu einer Stunde dauern), und sehen Sie sich dann die aus dem Speicherkonto abgerufenen Ereignisse an. Öffnen Sie hierzu **Ereignisanzeige > Windows-Protokolle > Weitergeleitete Ereignisse** auf dem Azlog Integrator.

5. Vergewissern Sie sich, dass der auf dem Computer installierte SIEM-Standardconnector so konfiguriert ist, dass er Ereignisse aus dem Ordner **Weitergeleitete Ereignisse** an Ihre SIEM-Instanz weiterreicht. Überprüfen Sie die SIEM-spezifische Konfiguration, um die zu integrierenden Protokolle zu konfigurieren und anzuzeigen.

## Was, wenn im Ordner „Weitergeleitete Ereignisse“ keine Daten angezeigt werden?

Sollten im Ordner **Weitergeleitete Ereignisse** nach einer Stunde immer noch keine Daten angezeigt werden, gehen Sie wie folgt vor:

1. Überprüfen Sie den Computer, und vergewissern Sie sich, dass er auf Azure zugreifen kann. Öffnen Sie zum Testen der Konnektivität das [Azure-Portal](http://portal.azure.com) über den Browser.

2. Vergewissern Sie sich, dass das Benutzerkonto **azlog** über Schreibberechtigungen für den Ordner **Benutzer\\azlog** verfügt.

3. Vergewissern Sie sich, dass das Speicherkonto, das mithilfe des Befehls **azlog source add** hinzugefügt wurde, beim Ausführen des Befehls **azlog source list** aufgeführt wird.
4. Überprüfen Sie unter **Ereignisanzeige > Windows-Protokolle > Anwendung**, ob von der Azure-Protokollintegration möglicherweise Fehler gemeldet wurden.

Sollten die Ereignisse immer noch nicht angezeigt werden, führen Sie die folgenden Schritte aus:

1. Laden Sie den [Microsoft Azure-Speicher-Explorer](http://storageexplorer.com/) herunter.

2. Stellen Sie eine Verbindung mit dem Speicherkonto her, das mithilfe des Befehls **azlog source add** hinzugefügt wurde.

3. Navigieren Sie im Microsoft Azure-Speicher-Explorer zur Tabelle **WADWindowsEventLogsTable**, und überprüfen Sie, ob die Tabelle Daten enthält. Falls nicht, ist die Diagnose auf dem virtuellen Computer nicht ordnungsgemäß konfiguriert.

## Integrieren von Azure-Überwachungsprotokollen und Security Center-Warnungen

1. Öffnen Sie die Eingabeaufforderung, und wechseln Sie mit **cd** zu **c:\\Programme\\Microsoft Azure Log Integration**.

2. Führen Sie den folgenden Befehl aus:

        azlog createazureid

      Dabei werden Sie zur Angabe Ihrer Azure-Anmeldeinformationen aufgefordert. Anschließend wird in den Azure AD-Mandanten, die die Azure-Abonnements hosten, in denen der angemeldete Benutzer als Administrator, Co-Administrator oder Besitzer fungiert, ein [Azure Active Directory-Dienstprinzipal](../active-directory/active-directory-application-objects.md) erstellt. Handelt es sich bei dem angemeldeten Benutzer nur um einen Gastbenutzer des Azure AD-Mandanten, tritt bei dem Befehl ein Fehler auf. Die Authentifizierung gegenüber Azure wird über Azure Active Directory (AD) durchgeführt. Beim Erstellen eines Dienstprinzipals für die Azure-Protokollintegration wird die Azure AD-Identität erstellt, die Lesezugriff auf Azure-Abonnements erhält.

3. Führen Sie den folgenden Befehl aus:

        azlog authorize <SubscriptionID>

      Dadurch wird dem in Schritt 2 erstellten Dienstprinzipal Lesezugriff auf das Abonnement zugewiesen. Ohne Angabe einer Abonnement-ID wird versucht, die Dienstprinzipal-Leserrolle für alle Abonnements zuzuweisen, auf die Sie Zugriff haben.

        azlog authorize 0ee9d577-9bc4-4a32-a4e8-c29981025328

      > [AZURE.NOTE] Wenn Sie den Befehl **authorize** unmittelbar nach dem Befehl **createazureid** ausführen, werden unter Umständen Warnungen angezeigt. Nach der Erstellung des Azure AD-Kontos dauert es etwas, bis das Konto verwendungsbereit ist. Wenn Sie nach dem Ausführen des Befehls **createazureid** ca. zehn Sekunden warten, um den Befehl **authorize** auszuführen, sollten diese Warnungen nicht angezeigt werden.

4. Überprüfen Sie die folgenden Ordner, um sicherzustellen, dass die JSON-Dateien des Überwachungsprotokolls vorhanden sind:

  - **c:\\Users\\azlog\\AzureResourceManagerJson**
  - **c:\\Users\\azlog\\AzureResourceManagerJsonLD**

5. Überprüfen Sie die folgenden Ordner, um sicherzustellen, dass darin bereits Security Center-Warnungen vorhanden sind:

  - **c:\\Users\\azlog\\AzureSecurityCenterJson**
  - **c:\\Users\\azlog\\AzureSecurityCenterJsonLD**

6. Verweisen Sie für den Standardconnector für die SIEM-Dateiweiterleitung auf den richtigen Ordner, um die Daten an die SIEM-Instanz zu übermitteln. Ja nach verwendetem SIEM-Produkt benötigen Sie unter Umständen einige Feldzuordnungen.

Wenn Sie Fragen zur Azure-Protokollintegration haben, senden Sie eine E-Mail an [AzSIEMteam@microsoft.com](mailto:AzSIEMteam@microsoft.com)

## Nächste Schritte

In diesem Tutorial haben Sie gelernt, wie Sie die Azure-Protokollintegration installieren und Protokolle aus Azure Storage integrieren. Weitere Informationen finden Sie in den folgenden Artikeln:

- [Microsoft Azure Log Integration for Azure logs (Preview)](https://www.microsoft.com/download/details.aspx?id=53324) (Microsoft Azure-Protokoll-Integration für Azure-Protokolle (Vorschau)): Download Center für Details, Systemanforderungen und Installationsanweisungen für die Azure-Protokollintegration.
- [Introduction to Azure log integration](security-azure-log-integration-overview.md) (Einführung in die Azure-Protokollintegration): Dieses Dokument stellt die Azure-Protokollintegration, die wichtigsten Funktionen sowie die Funktionsweise vor.
- [Partner configuration steps](https://blogs.msdn.microsoft.com/azuresecurity/2016/08/23/azure-log-siem-configuration-steps/) (Konfigurationsschritte für Partner): In diesem Blogbeitrag erfahren Sie, wie die Azure-Protokollintegration für die Partnerlösungen Splunk, HP ArcSight und IBM QRadar konfiguriert wird.
- [Azure log Integration frequently asked questions (FAQ)](security-azure-log-integration-faq.md) (Häufig gestellte Fragen zur Azure-Protokollintegration): Hier finden Sie Antworten auf häufig gestellte Fragen zur Azure-Protokollintegration.
- [Integrieren von Security Center-Warnungen mithilfe der Azure-Protokollintegration (Vorschau)](../security-center/security-center-integrating-alerts-with-log-integration.md): In diesem Dokument erfahren Sie, wie Sie Azure Security Center-Warnungen sowie von der Azure-Diagnose und Azure-Überwachungsprotokollen erfasste Sicherheitsereignisse virtueller Computer mit Ihrer Protokollanalyse- oder SIEM-Lösung synchronisieren.
- [New features for Azure diagnostics and Azure Audit Logs](https://azure.microsoft.com/blog/new-features-for-azure-diagnostics-and-azure-audit-logs/) (Neue Features für Azure-Diagnose und Azure-Überwachungsprotokolle): In diesem Blogbeitrag werden Azure-Überwachungsprotokolle und andere Features vorgestellt, mit denen Sie sich einen Einblick in die Abläufe Ihrer Azure-Ressourcen verschaffen können.

<!---HONumber=AcomDC_0921_2016-->
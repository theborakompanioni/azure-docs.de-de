<properties
	pageTitle="Arbeiten mit Azure AD-Anwendungsproxyconnectors | Microsoft Azure"
	description="Erläutert das Erstellen und Verwalten von Connectorgruppen im Azure AD-Anwendungsproxy."
	services="active-directory"
	documentationCenter=""
	authors="kgremban"
	manager="femila"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="09/09/2016"
	ms.author="kgremban"/>


# Veröffentlichen von Anwendungen in getrennten Netzwerken und an getrennten Standorten mithilfe von Connectorgruppen – öffentliche Vorschau

> [AZURE.SELECTOR]
- [Azure-Portal](active-directory-application-proxy-connectors-azure-portal.md)
- [Klassisches Azure-Portal](active-directory-application-proxy-connectors.md)


Connectorgruppen sind unter anderem in folgenden Szenarien nützlich:

- Standorte mit mehreren verbundenen Rechenzentren. In diesem Fall soll jeweils möglichst viel Datenverkehr innerhalb des Rechenzentrums verlaufen, da Verbindungen zwischen Rechenzentren für gewöhnlich teuer und langsam sind. Sie können in jedem Rechenzentrum Connectors bereitstellen, die jeweils nur die lokalen Anwendungen bedienen. Mit diesem Vorgehen werden bei völliger Transparenz für den Benutzer die Verbindungen zwischen den Rechenzentren minimiert.
- Verwalten von Anwendungen, die in isolierten Netzwerken installiert, jedoch nicht Teil des Hauptnetzwerks des Unternehmens sind. Connectorgruppen können verwendet werden, um dedizierte Connectors für isolierte Netzwerke zu installieren und außerdem die Anwendungen vom Netzwerk zu isolieren.
- Für IaaS-Anwendungen mit Cloudzugriff bieten Connectorgruppen einen gemeinsamen Dienst zum Absichern des Zugriffs auf alle Apps. Connectorgruppen erzeugen keine zusätzliche Abhängigkeit vom Unternehmensnetzwerk und beeinträchtigen nicht das App-Erlebnis. Connectors können in allen Cloudrechenzentren installiert werden und bedienen nur Anwendungen, die sich in diesem Netzwerk befinden. Es können mehrere Connectors installiert werden, um eine hohe Verfügbarkeit zu gewährleisten.
- Unterstützung für Umgebungen mit mehreren Gesamtstrukturen, in denen spezifische Connectors in einzelnen Gesamtstrukturen bereitgestellt werden können, um dort bestimmte Anwendungen zu bedienen.
- Connectorgruppen können bei Websites als Teil von Notfallkonzepten eingesetzt werden, um ein Failover zu erkennen oder als Datensicherung für die Hauptwebsite zu dienen.
- Connectorgruppen können auch verwendet werden, um mehrere Unternehmen von einem einzigen Mandanten aus zu bedienen.

## Voraussetzung: Erstellen der Connectors
Um Connectors zu gruppieren, müssen Sie sicherstellen, dass [mehrere Connectors installiert sind](active-directory-application-proxy-enable.md). Wenn Sie einen neuen Connector installieren, wird diese automatisch der Connectorgruppe **Standard** hinzugefügt.

## Schritt 1: Erstellen von Connectorgruppen
Sie können beliebig viele Connectorgruppen erstellen. Das Erstellen von Connectorgruppen erfolgt im [Azure-Portal](https://portal.azure.com).

1. Wählen Sie **Azure Active Directory** aus, um zum Verwaltungsdashboard für Ihr Verzeichnis zu wechseln. Wählen Sie dort **Unternehmensanwendungen** > **Anwendungsproxy** aus.

2. Klicken Sie auf die Schaltfläche **Connectorgruppen**. Das Blatt „Neue Connectorgruppe“ wird geöffnet.

3. Bennen Sie Ihre neue Connectorgruppe, und verwenden Sie das Dropdownmenü, um auszuwählen, welche Connectoren dieser Gruppe angehören sollen.

4. Wählen Sie **Speichern**, wenn Sie die Konfiguration Ihrer Connectorgruppe abgeschlossen haben.

## Schritt 2: Zuweisen von Anwendungen zu Ihren Connectorgruppen
Der letzte Schritt besteht darin, jede Anwendung der Connectorgruppe zuzuweisen, von der sie bedient wird.

1. Klicken Sie im Verwaltungsdashboard für Ihr Verzeichnis auf **Unternehmensanwendungen** > **Alle Anwendungen** > Anwendung, die Sie einer Connectorgruppe zuweisen möchten > **Anwendungsproxy**.
2. Wählen Sie unter **Connectorgruppe** mithilfe des Dropdownmenüs die Gruppe aus, die die Anwendung verwenden soll.
3. Klicken Sie auf **Speichern**, um die Änderungen zu übernehmen.


## Siehe auch

- [Aktivieren des Anwendungsproxys](active-directory-application-proxy-enable.md)
- [Aktivieren der einmaligen Anmeldung](active-directory-application-proxy-sso-using-kcd.md)
- [Aktivieren des bedingten Zugriffs](active-directory-application-proxy-conditional-access.md)
- [Problembehandlung von Anwendungsproxys](active-directory-application-proxy-troubleshoot.md)

Aktuelle Neuigkeiten und Updates finden Sie im [Blog zum Anwendungsproxy](http://blogs.technet.com/b/applicationproxyblog/).

<!---HONumber=AcomDC_0914_2016-->
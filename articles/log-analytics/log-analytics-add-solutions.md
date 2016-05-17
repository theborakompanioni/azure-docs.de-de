<properties
	pageTitle="Hinzufügen von Log Analytics-Lösungen aus dem Lösungskatalog | Microsoft Azure"
	description="Log Analytics-Lösungen sind eine Sammlung von Logik-, Visualisierungs- und Datenerfassungsregeln, die Metriken rund um einen bestimmten Problembereich bereitstellen."
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
	ms.date="04/28/2016"
	ms.author="banders"/>

# Hinzufügen von Log Analytics-Lösungen aus dem Lösungskatalog

Log Analytics-Lösungen sind eine Sammlung von **Logik-**, **Visualisierungs-** und **Datenerfassungsregeln**, die Metriken rund um einen bestimmten Problembereich bereitstellen. Dieser Artikel beschreibt von Log Analytics unterstützte Lösungen und erläutert, wie Sie diese über den Lösungskatalog hinzufügen und entfernen.

Die detaillierten Einblicke der Lösungen ermöglichen Folgendes:
- Schnelleres Untersuchen und Lösen von Betriebsproblemen
- Sammeln und Korrelieren von verschiedenen Arten von Computerdaten
- Proaktive Umsetzung von Maßnahmen wie Kapazitätsplanung, Patchstatusberichte und Sicherheitsüberwachung.


>[AZURE.NOTE] OMS umfasst Funktionen für die Protokollsuche, Sie müssen also keine separate Lösung dafür installieren. Allerdings können Sie zusätzliche Funktionen nutzen, indem Sie Lösungen aus dem Lösungskatalog hinzufügen.

Nachdem Sie eine Lösung hinzugefügt haben, werden Daten von den Servern in Ihrer Infrastruktur erfasst und an den OMS-Dienst gesendet. Die Verarbeitung durch den OMS-Dienst kann wenige Minuten, jedoch auch mehrere Stunden dauern. Nach der Verarbeitung der Daten durch den Dienst können Sie diese in OMS anzeigen.

Wenn eine Lösung nicht mehr benötigt wird, können Sie sie problemlos entfernen. Wenn Sie eine Lösung entfernen, werden ihre Daten nicht mehr an OMS gesendet, und die Datenmenge, die Ihr Tageskontingent belastet (sofern vorhanden), verringert sich entsprechend.


## Von Microsoft Monitoring Agent unterstützte Lösungen

Zurzeit können Server, die über den Microsoft Monitoring Agent direkt mit OMS verbunden sind, die meisten verfügbaren Lösungen nutzen, einschließlich der folgenden:

- System Updates
- Antimalware
- Change Tracking
- SQL Assessment
- Active Directory Assessment
- Alert Management (ohne SCOM-Warnungen)

Die folgenden Lösungen werden hingegen *nicht* mit dem Microsoft Monitoring Agent unterstützt, sondern erfordern einen SCOM-Agent (System Center Operations Manager).

- Capacity Management
- Alert Management (mit SCOM-Warnungen)
- Konfigurationsbewertung

Informationen zum Verbinden des SCOM-Agents mit Log Analytics finden Sie unter [Connecting Operations Manager to Log Analytics](log-analytics-om-agents.md) (Verbinden von Operations Manager mit Log Analytics).

### So fügen Sie eine Lösung mit Solutions Gallery hinzu

1. Klicken Sie in OMS auf der Übersichtsseite auf die Kachel **Lösungskatalog**. ![Lösungskatalog](./media/log-analytics-add-solutions/sol-gallery.png)
2. Auf der Seite des OMS-Lösungskatalogs finden Sie Informationen zu jeder der verfügbaren Lösungen. Klicken Sie auf den Namen der Lösung, die Sie OMS hinzufügen möchten.
3. Auf der Seite für die ausgewählte Lösung werden ausführliche Informationen zur Lösung angezeigt. Klicken Sie auf **Hinzufügen**.
4. Auf der Übersichtsseite in OMS wird eine neue Kachel für die hinzugefügte Lösung angezeigt, und Sie können die Lösung verwenden, nachdem der OMS-Dienst Ihre Daten verarbeitet hat.

## Konfigurieren von Lösungen
1. Sie müssen einige Lösungen konfigurieren. Beispielsweise müssen Sie Automation, Azure Site Recovery und Backup konfigurieren, bevor Sie die Lösungen verwenden können.
2. Klicken Sie auf der Übersichtsseite auf die Kachel für eine dieser Lösungen. ![Lösung konfigurieren](./media/log-analytics-add-solutions/configure-additional.png)
3. Konfigurieren Sie die Lösung mit den notwendigen Informationen, und klicken Sie dann auf **Speichern**. ![Lösung konfigurieren](./media/log-analytics-add-solutions/configure.png)

### So entfernen Sie eine Lösung mit Solutions Gallery

1. Klicken Sie in OMS auf der Übersichtsseite auf die Kachel **Einstellungen**.
2. Klicken Sie auf der Einstellungsseite auf der Registerkarte mit den Lösungen bei der Lösung, die Sie entfernen möchten, auf **Entfernen**.
3. Klicken Sie im Bestätigungsdialogfeld auf **Ja**, um die Lösung zu entfernen.

## Details zur Datensammlung für OMS-Features und -Lösungen

Die folgende Tabelle zeigt die Datensammlungsmethoden und andere Details dazu, wie Daten für OMS-Features und -Lösungen gesammelt werden.

|Datentyp| Plattform | Direkt-Agent | SCOM-Agent | Azure Storage | SCOM erforderlich? | Daten von SCOM-Agent über Verwaltungsgruppe gesendet | Sammlungshäufigkeit |
|---|---|---|---|---|---|---|---|
|AD Assessment|Windows|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|	7 Tage|
|AD Replication Status|Windows|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|5 Tage|
|Warnungen (Nagios)|Linux|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|Bei Ankunft|
|Warnungen (Zabbix)|Linux|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|1 Minute|
|Warnungen (Operations Manager)|Windows|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|3 Minuten|
|Antimalware|Windows|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)| Stündlich|
|Capacity Management|Windows|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)| Stündlich|
|Change Tracking|Windows|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)| Stündlich|
|Change Tracking|Linux|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|Stündlich|
|Configuration Assessment (ehemals Advisor)|Windows|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)| Zweimal pro Tag|
|ETW|Windows|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|5 Minuten|
|IIS-Protokolle|Windows|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|5 Minuten|
|Netzwerksicherheitsgruppen|Windows|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|10 Minuten|
|Office 365|Windows|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|Bei Benachrichtigung|
|Leistungsindikatoren|Windows|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|Gemäß Zeitplan, mindestens 10 Sekunden|
|Leistungsindikatoren|Linux|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|Gemäß Zeitplan, mindestens 10 Sekunden|
|Service Fabric|Windows|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|5 Minuten|
|SQL Assessment|Windows|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|	7 Tage|
|SurfaceHub|Windows|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|Bei Ankunft|
|Syslog|Linux|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|Von Azure-Speicher: 10 Minuten; von Agent: bei Ankunft|
|System Updates|Windows|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)| Mindestens zweimal pro Tag und 15 Minuten nach Installation eines Updates|
|Windows-Sicherheitsereignisprotokolle|Windows|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)| Für Azure-Speicher: 10 Minuten; für Agent: bei Ankunft|
|Windows-Firewallprotokolle|Windows|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)| Bei Ankunft|
|Windows-Ereignisprotokolle|Windows|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)| Für Azure-Speicher: 1 Minute; für Agent: bei Ankunft|
|Wire Data|Windows (2012 R2 / 8.1 oder höher)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Ja](./media/log-analytics-add-solutions/oms-bullet-green.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)|![Nein](./media/log-analytics-add-solutions/oms-bullet-red.png)| Jede Minute|


## Nächste Schritte

- [Suchprotokolle](log-analytics-log-searches.md) zur Anzeige von detaillierten Informationen, die von Lösungen gesammelt wurden.

<!---HONumber=AcomDC_0504_2016-->
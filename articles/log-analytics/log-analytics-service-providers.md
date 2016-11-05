---
title: Log Analytics-Funktionen für Dienstanbieter | Microsoft Docs
description: Log Analytics unterstützt Managed Service Provider (MSPs), Großunternehmen, unabhängige Softwarehersteller (Independent Software Vendors, ISVs) und Hosting-Anbieter beim Verwalten und Überwachen von Servern in der lokalen oder Cloudinfrastruktur des Kunden.
services: log-analytics
documentationcenter: ''
author: richrundmsft
manager: jochan
editor: ''

ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/25/2016
ms.author: richrund

---
# <a name="log-analytics-features-for-service-providers"></a>Log Analytics-Funktionen für Dienstanbieter
Log Analytics unterstützt Managed Service Provider (MSPs), Großunternehmen, unabhängige Softwarehersteller (Independent Software Vendors, ISVs) und Hosting-Anbieter beim Verwalten und Überwachen von Servern in der lokalen oder Cloudinfrastruktur des Kunden. 

Zwischen Großunternehmen und Dienstanbietern bestehen viele Gemeinsamkeiten, insbesondere wenn ein zentrales IT-Team für die Verwaltung der IT vieler unterschiedlicher Geschäftseinheiten verantwortlich ist. Der Einfachheit halber wird in diesem Dokument der Begriff *Dienstanbieter* verwendet, jedoch ist die gleiche Funktionalität für Unternehmen und andere Kunden verfügbar.

## <a name="cloud-solution-provider"></a>Cloud-Lösungsanbieter
Für Partner und Dienstanbieter, die am Programm [Cloud-Lösungsanbieter (Cloud Solution Provider, CSP)](https://partner.microsoft.com/Solutions/cloud-reseller-overview) teilnehmen, ist Log Analytics einer der in einem CSP-Abonnement verfügbaren Azure-Dienste. 

Für Log Analytics sind in *Cloud-Lösungsanbieter*-Abonnements die folgenden Funktionen aktiviert.

Als *Cloud-Lösungsanbieter (Cloud Solution Provider, CSP)* haben Sie folgende Möglichkeiten:

* Erstellen von Log Analytics-Arbeitsbereichen im Abonnement eines Mandanten (Kunden).
* Zugreifen auf Arbeitsbereiche, die von Mandanten erstellt wurden. 
* Hinzufügen und Entfernen des Benutzerzugriffs auf den Arbeitsbereich mithilfe der Azure-Benutzerverwaltung, wenn im Arbeitsbereich eines Mandanten im OMS-Portal die Seite für die Benutzerverwaltung unter „Einstellungen“ nicht verfügbar ist.
  * Rollenbasierter Zugriff wird von Log Analytics noch nicht unterstützt. Wenn einem Benutzer im Azure-Portal die Berechtigung `reader` erteilt wird, kann er Konfigurationsänderungen im OMS-Portal vornehmen.

Zum Anmelden beim Abonnement eines Mandanten müssen Sie die Mandanten-ID angeben. Die Mandanten-ID ist häufig der letzte Teil der E-Mail-Adresse, mit der Sie sich anmelden.

* Fügen Sie im OMS-Portal in der URL für das Portal `?tenant=contoso.com` hinzu. Beispiel: `mms.microsoft.com/?tenant=contoso.com`
* Geben Sie in PowerShell den `-Tenant contoso.com`-Parameter an, wenn Sie das `Add-AzureRmAccount`-Cmdlet verwenden.
* Die Mandanten-ID wird automatisch hinzugefügt, wenn Sie zum Aufrufen des OMS-Portals für den ausgewählten Arbeitsbereich und Anmelden beim Portal den `OMS portal`-Link aus dem Azure-Portal verwenden.

Als *Kunde* eines Cloud-Lösungsanbieters haben Sie folgende Möglichkeiten:

* Erstellen von Log Analytics-Arbeitsbereichen in einem CSP-Abonnement
* Zugreifen auf Arbeitsbereiche, die vom CSP erstellt wurden
  * Verwenden Sie den `OMS portal`-Link aus dem Azure-Portal, um das OMS-Portal für den ausgewählten Arbeitsbereich aufzurufen und sich bei ihm anzumelden.
* Anzeigen und Verwenden der Seite für die Benutzerverwaltung unter „Einstellungen“ im OMS-Portal

> [!NOTE]
> Die Backup und Site Recovery-Lösungen für Log Analytics können keine Verbindung mit dem Recovery Services-Tresor herstellen und nicht in einem CSP-Abonnement konfiguriert werden.
> 
> 

## <a name="managing-multiple-customers-using-log-analytics"></a>Verwalten von mehreren Kunden mit Log Analytics
Es wird empfohlen, für jeden Kunden, den Sie verwalten, einen Log Analytics-Arbeitsbereich zu erstellen. Ein Log Analytics-Arbeitsbereich bietet Folgendes:

* Einen geografischen Standort für die Speicherung von Daten 
* Granularität für die Abrechnung 
* Datenisolation 
* Eindeutige Konfiguration

Durch das Erstellen eines Arbeitsbereichs pro Kunde können Sie die Daten der einzelnen Kunden getrennt halten und außerdem die Nutzung durch die einzelnen Kunden nachverfolgen.

Weitere Informationen darüber, wann und warum mehrere Arbeitsbereiche erstellt werden sollten, finden Sie unter [Verwalten des Zugriffs auf Log Analytics](log-analytics-manage-access.md#determine-the-number-of-workspaces-you-need).

Die Erstellung und Verwaltung von Kundenarbeitsbereichen kann mit [PowerShell](log-analytics-powershell-workspace-configuration.md), [Resource Manager-Vorlagen](log-analytics-template-workspace-configuration.md) oder der [REST-API](https://www.nuget.org/packages/Microsoft.Azure.Management.OperationalInsights/) automatisiert werden.

Die Verwendung von Resource Manager-Vorlagen für die Konfiguration von Arbeitsbereichen ermöglicht es Ihnen, Arbeitsbereiche mithilfe einer Hauptkonfiguration zu erstellen und zu konfigurieren. Sie können sich darauf verlassen, dass beim Erstellen von Arbeitsbereichen für Kunden diese automatisch entsprechend deren Bedürfnissen konfiguriert werden. Wenn Sie die Anforderungen aktualisieren, wird die Vorlage ebenfalls aktualisiert und dann erneut auf die vorhandenen Arbeitsbereiche angewendet. Dadurch wird sichergestellt, dass selbst bereits vorhandene Arbeitsbereiche die neuen Standards erfüllen.    

Beim Verwalten mehrerer Log Analytics-Arbeitsbereiche wird empfohlen, mithilfe der Funktion [Warnungen](log-analytics-alerts.md) jeden Arbeitsbereich in das vorhandene Ticketausstellungssystem/die vorhandene Betriebskonsole zu integrieren. Durch die Integration in die vorhandenen Systeme können die Supportmitarbeiter weiterhin die ihnen vertrauten Verfahren verwenden. Log Analytics überprüft regelmäßig jeden Arbeitsbereich anhand der von Ihnen angegebenen Kriterien und generiert eine Warnung, wenn eine Aktion erforderlich ist.

Für Berichte auf Leitungsebene, in denen Daten mehrerer Arbeitsbereiche zusammengefasst werden, können Sie die Integration zwischen Log Analytics und [Power BI](log-analytics-powerbi.md) nutzen. Wenn die Integration in ein anderes Berichterstellungssystem erforderlich ist, können Sie die Search-API verwenden (über PowerShell oder [REST](log-analytics-log-search-api.md)), um Abfragen auszuführen und Suchergebnisse zu exportieren.

## <a name="next-steps"></a>Nächste Schritte
* Automatisieren Sie mithilfe von [Resource Manager-Vorlagen](log-analytics-template-workspace-configuration.md) die Erstellung und Konfiguration von Arbeitsbereichen.
* Automatisieren Sie mit [PowerShell](log-analytics-powershell-workspace-configuration.md) die Erstellung von Arbeitsbereichen. 
* Verwenden Sie [Warnungen](log-analytics-alerts.md) für die Integration in vorhandene Systeme.
* Generieren Sie mit [Power BI](log-analytics-powerbi.md) Zusammenfassungsberichte.

<!--HONumber=Oct16_HO2-->



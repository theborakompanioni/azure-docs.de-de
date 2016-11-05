---
title: Dokumentation zu Azure Government | Microsoft Docs
description: Dies bietet einen Vergleich der Features und Richtlinien zum Entwickeln von Anwendungen für Azure Government
services: Azure-Government
cloud: gov
documentationcenter: ''
author: scooxl
manager: zakramer
editor: ''

ms.service: multiple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: azure-government
ms.date: 10/05/2016
ms.author: scooxl

---
# <a name="azure-government-management-and-security"></a>Azure Government-Verwaltung und -Sicherheit
## <a name="key-vault"></a>Schlüsseltresor
Einzelheiten zu diesem Dienst und seiner Verwendung finden Sie in der <a href="https://azure.microsoft.com/documentation/services/key-vault">öffentlichen Dokumentation zu Azure Key Vault. </a>

### <a name="data-considerations"></a>Überlegungen zu Daten
Die folgenden Informationen geben Aufschluss über die Abgrenzung von Azure Government für Azure Key Vault:

| Regulierte/kontrollierte Daten zulässig | Regulierte/kontrollierte Daten nicht zulässig |
| --- | --- |
| Alle mit einem Azure Key Vault-Schlüssel verschlüsselten Daten können regulierte/kontrollierte Daten enthalten. |Azure Key Vault-Metadaten dürfen keine den Exportbestimmungen unterliegenden Daten enthalten. Zu diesen Metadaten gehören alle Konfigurationsdaten, die beim Erstellen und Verwalten der Key Vault-Instanz eingegeben werden.  Geben Sie keine regulierten/kontrollierten Daten in die folgenden Felder ein: Resource group names (Ressourcengruppennamen), Key Vault names (Key Vault-Namen), Abonnementname. |

Key Vault ist allgemein in Azure Government verfügbar. Wie auch bei der öffentlichen Variante gibt es keine Erweiterung. Key Vault ist daher nur über PowerShell und die CLI verfügbar.

## <a name="log-analytics"></a>Log Analytics
Log Analytics ist allgemein in Azure Government verfügbar. 

### <a name="differences-from-public-azure"></a>Unterschiede zu öffentlichem Azure
Die folgenden Log Analytics-Features und -Lösungen sind aktuell nicht in Azure Government verfügbar. Diese Liste wird aktualisiert, sobald sich der Status von Features bzw. Lösungen ändert.

* Lösungen, die als Vorschau im öffentlichen Azure enthalten sind, einschließlich:
  * Netzwerküberwachungslösung
  * Azure Networking Analytics-Lösung
  * Office 365-Lösung
  * Windows 10 Upgrade Analytics-Lösung
  * Anwendungsabhängigkeitsüberwachung
  * Application Insights
  * Azure-Aktivitätsprotokolle
  * Azure Automation-Analysen
  * Key Vault-Analysen
* Lösungen und Funktionen, die Azure Automation erfordern, einschließlich:
  * Updateverwaltung
  * Change Management
  * Warnungen, die ein Azure Automation-Runbook auslösen
* Lösungen und Funktionen, die Updates von lokaler Software erfordern, einschließlich
  * Integration in System Center Operations Manager 2016
  * Computergruppen aus System Center Configuration Manager
  * Surface Hub-Lösung
* Funktionen, die als Vorschau im öffentlichen Azure enthalten sind, einschließlich
  * Exportieren von Daten in Power BI
* Azure-Portal-Integration
  * Das Auswählen zu überwachender Azure-Speicherkonten muss über PowerShell oder Resource Manager-Vorlagen erfolgen.
  * Das Auswählen von virtuellen Computern zum Aktivieren des Log Analytics-Agents muss über PowerShell oder Resource Manager-Vorlagen erfolgen.
  * Azure-Metriken und Azure-Diagnose
* OMS Mobile-Anwendungen
* OMS Linux Agent-VM-Erweiterung
* Nutzungsdaten

Die folgenden Log Analytics-Feature verhalten sich in Azure Government anders:

* Der Windows-Agent muss für Azure Government aus dem [Log Analytics-Portal](https://oms.microsoft.us) heruntergeladen werden.
* Das Hochladen von Daten mithilfe der Data Collector-API erfordert die Verwendung der Azure Government-URL, https://*workspaceId*.ods.opinsights.azure.us, wobei *workspaceId* die Arbeitsbereichs-ID aus dem OMS-Portal ist. 
* Zum Verbinden Ihres System Center Operations Manager-Verwaltungsservers mit Log Analytics müssen Sie aktualisierte Management Packs herunterladen und importieren.
  1. Laden Sie die [aktualisierten Management Packs](http://go.microsoft.com/fwlink/?LinkId=828749) herunter, und speichern Sie sie.
  2. Entzippen Sie die heruntergeladene Datei.
  3. Importieren Sie die Management Packs in Operations Manager. Informationen zum Importieren eines Management Packs von einem Datenträger finden Sie auf der TechNet-Website im Thema [Importieren eines Management Packs von Operations Manager](http://technet.microsoft.com/library/hh212691.aspx).
  4. Zum Verbinden von Operations Manager mit Log Analytics führen Sie die Schritte in [Herstellen einer Verbindung zwischen Operations Manager und Log Analytics](../log-analytics/log-analytics-om-agents.md) aus. 

### <a name="frequently-asked-questions"></a>Häufig gestellte Fragen
* Kann ich Daten aus Log Analytics im öffentlichen Azure in Azure Government migrieren?
  * Nein. Es ist nicht möglich, Daten oder Ihren Arbeitsbereich aus öffentlichem Azure in Azure Government zu verschieben.
* Kann ich im OMS Log Analytics-Portal zwischen öffentlichen Azure- und Azure Government-Arbeitsbereichen wechseln?
  * Nein. Die Portale für öffentliches Azure und Azure Government sind getrennt und nutzen Informationen nicht gemeinsam. 

Weitere Informationen finden Sie in der [öffentlichen Log Analytics-Dokumentation](../log-analytics/log-analytics-overview.md).

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen und Updates erhalten Sie, indem Sie den <a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure Government-Blog</a> abonnieren.

<!--HONumber=Oct16_HO2-->



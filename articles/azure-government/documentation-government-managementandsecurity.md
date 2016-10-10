<properties
	pageTitle="Dokumentation zu Azure Government | Microsoft Azure"
	description="Dies bietet einen Vergleich der Features und Richtlinien zum Entwickeln von Anwendungen für Azure Government"
	services="Azure-Government"
	cloud="gov" 
	documentationCenter=""
	authors="ryansoc"
	manager="zakramer"
	editor=""/>

<tags
	ms.service="multiple"
	ms.devlang="na"
	ms.topic="article"
	ms.tgt_pltfrm="na"
	ms.workload="azure-government"
	ms.date="09/23/2016"
	ms.author="ryansoc"/>


#  Azure Government-Verwaltung und -Sicherheit

##  Schlüsseltresor

Die folgenden Informationen geben Aufschluss über die Abgrenzung von Azure Government für Azure Key Vault:

| Regulierte/kontrollierte Daten zulässig | Regulierte/kontrollierte Daten nicht zulässig |
|--------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Alle mit einem Azure Key Vault-Schlüssel verschlüsselten Daten können regulierte/kontrollierte Daten enthalten. | Azure Key Vault-Metadaten dürfen keine den Exportbestimmungen unterliegenden Daten enthalten. Zu diesen Metadaten gehören alle Konfigurationsdaten, die beim Erstellen und Verwalten der Key Vault-Instanz eingegeben werden. Geben Sie keine regulierten/kontrollierten Daten in die folgenden Felder ein: Resource group names (Ressourcengruppennamen), Key Vault names (Key Vault-Namen), Abonnementname. |

Key Vault ist allgemein in Azure Government verfügbar. Wie auch bei der öffentlichen Variante gibt es keine Erweiterung. Key Vault ist daher nur über PowerShell und die CLI verfügbar.

Weitere Informationen finden Sie in der [öffentlichen Dokumentation zu Azure Key Vault](/key-vault-get-started).

## Log Analytics

Log Analytics befindet sich in Azure Government in der Vorschauphase.

### Unterschiede zu öffentlichem Azure

Die folgenden Log Analytics-Features und -Lösungen sind aktuell nicht in Azure Government verfügbar. Diese Liste wird aktualisiert, sobald sich der Status von Features bzw. Lösungen ändert.

+ Metriken nahezu in Echtzeit
  - Beim Anzeigen von Diagrammen von Metriken für einen Zeitraum unter sechs Stunden wird das Diagramm nicht automatisch mit neuen Metrikwerten aktualisiert.
+ Exportieren von Daten in Power BI
+ Netzwerküberwachungslösung
+ Office 365-Lösung
+ Windows 10 Upgrade Analytics-Lösung
+ Anwendungsabhängigkeitsüberwachung
+ Updatebewertung
+ Azure-Portal-Integration
  - Das Auswählen zu überwachender Azure-Speicherkonten muss über PowerShell oder Resource Manager-Vorlagen erfolgen.
  - Das Auswählen von virtuellen Computern zum Aktivieren des Log Analytics-Agents muss über PowerShell oder Resource Manager-Vorlagen erfolgen.
+ Linux-Überwachung
+ OMS Mobile-Anwendungen
+ Microsoft Monitoring Agent-VM-Erweiterung
+ OMS Linux Agent-VM-Erweiterung
+ Nutzungsdaten
+ Warn-E-Mails und Wiederherstellung mithilfe von Azure Automation

Die folgenden Log Analytics-Feature verhalten sich in Azure Government anders:

+ Der Windows-Agent muss für Azure Government aus dem Log Analytics-Portal heruntergeladen werden.
+ Die Sicherheits- und Überwachungslösung unterstützt nicht die Erkennung böswilliger IP-Adressen.
+ Das Hochladen von Daten mithilfe der Data Collector-API erfordert die Verwendung der Azure Government-URL.

### Häufig gestellte Fragen

+ Kann ich Daten aus Log Analytics im öffentlichen Azure in Azure Government migrieren?
  - Nein. Es ist nicht möglich, Daten oder Ihren Arbeitsbereich aus öffentlichem Azure in Azure Government zu verschieben.
+ Kann ich im OMS Log Analytics-Portal zwischen öffentlichen Azure- und Azure Government-Arbeitsbereichen wechseln?
  - Nein. Die Portale für öffentliches Azure und Azure Government sind getrennt und nutzen Informationen nicht gemeinsam.

Weitere Informationen finden Sie in der [öffentlichen Log Analytics-Dokumentation](../log-analytics/log-analytics-overview.md).

## Nächste Schritte

Weitere Informationen und Updates erhalten Sie, indem Sie den <a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure Government-Blog</a> abonnieren.

<!---HONumber=AcomDC_0928_2016-->
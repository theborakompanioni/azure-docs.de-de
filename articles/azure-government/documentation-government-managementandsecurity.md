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
	ms.date="08/25/2016"
	ms.author="ryansoc"/>


#  Azure Government-Verwaltung und -Sicherheit

##  Schlüsseltresor

Die folgenden Informationen geben Aufschluss über die Abgrenzung von Azure Government für Azure Key Vault:

| Regulierte/kontrollierte Daten zulässig | Regulierte/kontrollierte Daten nicht zulässig |
|--------------------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Alle mit einem Azure Key Vault-Schlüssel verschlüsselten Daten können regulierte/kontrollierte Daten enthalten. | Azure Key Vault-Metadaten dürfen keine den Exportbestimmungen unterliegenden Daten enthalten. Zu diesen Metadaten gehören alle Konfigurationsdaten, die beim Erstellen und Verwalten der Key Vault-Instanz eingegeben werden. Geben Sie keine regulierten/kontrollierten Daten in die folgenden Felder ein: Resource group names (Ressourcengruppennamen), Key Vault names (Key Vault-Namen), Abonnementname. |

Key Vault ist allgemein in Azure Government verfügbar. Wie auch bei der öffentlichen Variante gibt es keine Erweiterung. Key Vault ist daher nur über PowerShell und die CLI verfügbar.

Weitere Informationen finden Sie in der [öffentlichen Dokumentation zu Azure Key Vault](/key-vault-get-started).

Weitere Informationen und Updates erhalten Sie, indem Sie den <a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure Government-Blog</a> abonnieren.

<!---HONumber=AcomDC_0831_2016-->
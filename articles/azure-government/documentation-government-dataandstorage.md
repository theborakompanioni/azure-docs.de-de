---
title: Dokumentation zu Azure Government | Microsoft Docs
description: Dies bietet einen Vergleich der Features und Richtlinien zum Entwickeln von Anwendungen für Azure Government
services: Azure-Government
cloud: gov
documentationcenter: ''
author: ryansoc
manager: zakramer
editor: ''

ms.service: multiple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: azure-government
ms.date: 08/25/2016
ms.author: ryansoc

---
# Azure Government-Daten und -Speicher
## Speicher
Die folgenden Informationen geben Aufschluss über die Abgrenzung von Azure Government für Azure Storage:

| Regulierte/kontrollierte Daten zulässig | Regulierte/kontrollierte Daten nicht zulässig |
| --- | --- |
| Daten, die in einem Azure Storage-Produkt eingegeben, gespeichert und verarbeitet werden, können Exportbestimmungen unterliegende Daten enthalten. Statische Authentifikatoren wie Kennwörter und Smartcard-PINs für den Zugriff auf Komponenten der Azure-Plattform. Private Schlüssel von Zertifikaten, die für die Verwaltung von Komponenten der Azure-Plattform verwendet werden. Andere Sicherheitsinformationen/geheime Schlüssel, wie etwa Zertifikate, Verschlüsselungsschlüssel, Hauptschlüssel und Speicherschlüssel, die in Azure-Diensten gespeichert sind. |Azure Storage-Metadaten dürfen keine den Exportbestimmungen unterliegenden Daten enthalten. Zu diesen Metadaten gehören alle Konfigurationsdaten, die beim Erstellen und Verwalten des Speicherprodukts eingegeben werden. Geben Sie keine regulierten/kontrollierten Daten in die folgenden Felder ein: Ressourcengruppen, Deployment names (Bereitstellungsnamen), Ressourcennamen, Resourcentags. |

Weitere Informationen finden Sie in der <a href=https://azure.microsoft.com/documentation/services/storage/>öffentlichen Azure Storage-Dokumentation</a>.

Weitere Informationen und Updates erhalten Sie, indem Sie den <a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure Government-Blog</a> abonnieren.

## SQL-Datenbank
Die folgenden Informationen geben Aufschluss über die Abgrenzung von Azure Government für Azure Storage:

| Regulierte/kontrollierte Daten zulässig | Regulierte/kontrollierte Daten nicht zulässig |
| --- | --- |
| Alle in Microsoft Azure SQL gespeicherte und verarbeitete Daten können durch Azure Government regulierte Daten enthalten. Sie müssen Datenbanktools für die Übertragung von durch Azure Government regulierte Daten verwenden. |Azure SQL-Metadaten dürfen keine den Exportbestimmungen unterliegenden Daten enthalten. Zu diesen Metadaten gehören alle Konfigurationsdaten, die beim Erstellen und Verwalten des Speicherprodukts eingegeben werden. Geben Sie keine regulierten/kontrollierten Daten in die folgenden Felder ein: Datenbankname, Abonnementname, Ressourcengruppen, Servername, Serveradministratoranmeldung, Deployment names (Bereitstellungsnamen), Ressourcennamen, Ressourcentags. |

SQL-Datenbank V11 ist allgemein in Azure Government verfügbar.

Zusätzliche Anleitungen zur Konfiguration der Metadatensichtbarkeit und bewährte Methoden für den Schutz finden Sie unter <a href="https://msdn.microsoft.com/de-DE/library/bb510589.aspx">Sicherheitscenter für SQL Server-Datenbankmodul und Azure SQL-Datenbank</a> und in der <a href="https://azure.microsoft.com/documentation/services/sql-database/">öffentlichen Dokumentation zu Azure SQL-Datenbank</a>.

Weitere Informationen und Updates erhalten Sie, indem Sie den <a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure Government-Blog</a> abonnieren.

<!---HONumber=AcomDC_0831_2016-->
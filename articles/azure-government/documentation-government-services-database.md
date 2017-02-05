---
title: Azure Government-Datenbanken | Microsoft Docs
description: "Dies bietet einen Vergleich der Features und Richtlinien zum Entwickeln von Anwendungen für Azure Government"
services: azure-government
cloud: gov
documentationcenter: 
author: ryansoc
manager: zakramer
ms.assetid: a1e173a9-996a-4091-a2e3-6b1e36da9ae1
ms.service: azure-government
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: azure-government
ms.date: 11/14/2016
ms.author: ryansoc
translationtype: Human Translation
ms.sourcegitcommit: c94e643565374f852633254a94c2138713cc290e
ms.openlocfilehash: 58d6295057c71dad488e04be25cc9a85d73bcd20


---
# <a name="azure-government-databases"></a>Azure Government-Datenbanken
## <a name="sql-database"></a>SQL-Datenbank
Zusätzliche Anleitungen zur Konfiguration der Metadatensichtbarkeit und bewährte Methoden für den Schutz finden Sie unter <a href="https://msdn.microsoft.com/en-us/library/bb510589.aspx">Sicherheitscenter für SQL Server-Datenbankmodul und Azure SQL-Datenbank</a> und in der [öffentlichen Dokumentation zu Azure SQL-Datenbank](../sql-database/index.md).

### <a name="variations"></a>Variationen
SQL-Datenbank V12 ist allgemein in Azure Government verfügbar.

In Azure Government wird eine abweichende Adresse für SQL Azure-Server verwendet:

| Service Type | Azure – Öffentlich | Azure Government |
| --- | --- | --- |
| SQL-Datenbank |*.database.windows.net |*.database.usgovcloudapi.net |

### <a name="considerations"></a>Überlegungen
Die folgenden Informationen geben Aufschluss über die Abgrenzung von Azure Government für Azure SQL:

| Regulierte/kontrollierte Daten zulässig | Regulierte/kontrollierte Daten nicht zulässig |
| --- | --- |
| Alle in Microsoft Azure SQL gespeicherte und verarbeitete Daten können durch Azure Government regulierte Daten enthalten. Sie müssen Datenbanktools für die Übertragung von durch Azure Government regulierte Daten verwenden. |Azure SQL-Metadaten dürfen keine den Exportbestimmungen unterliegenden Daten enthalten. Zu diesen Metadaten gehören alle Konfigurationsdaten, die beim Erstellen und Verwalten des Speicherprodukts eingegeben werden.  Geben Sie keine regulierten/kontrollierten Daten in die folgenden Felder ein: Datenbankname, Abonnementname, Ressourcengruppen, Servername, Serveradministratoranmeldung, Deployment names (Bereitstellungsnamen), Ressourcennamen, Ressourcentags. |

## <a name="azure-redis-cache"></a>Azure Redis Cache
Einzelheiten zu diesem Dienst und seiner Verwendung finden Sie in der [öffentlichen Dokumentation zu Azure Redis Cache](../redis-cache/index.md).

### <a name="variations"></a>Variationen
In Azure Government werden abweichende URLs für den Zugriff auf und die Verwaltung von Azure Redis Cache verwendet:

| Service Type | Azure – Öffentlich | Azure Government |
| --- | --- | --- |
| Cache-Endpunkt |*.redis.cache.windows.net |*.redis.cache.usgovcloudapi.net |
| Azure-Portal |https://portal.azure.com |https://portal.azure.us |

> [!NOTE]
> Ihre gesamten Skripts und Ihr Code müssen die richtigen Endpunkte und Umgebungen berücksichtigen. Weitere Informationen finden Sie unter [Gewusst wie: Verbinden mit Azure Government Cloud](../redis-cache/cache-howto-manage-redis-cache-powershell.md#how-to-connect-to-azure-government-cloud-or-azure-china-cloud).
> 
> 

### <a name="considerations"></a>Überlegungen
Die folgenden Informationen geben Aufschluss über die Abgrenzung von Azure Government für Azure Redis Cache:

| Regulierte/kontrollierte Daten zulässig | Regulierte/kontrollierte Daten nicht zulässig |
| --- | --- |
| Alle in Azure Redis Cache gespeicherten und verarbeiteten Daten können durch Azure Government regulierte Daten enthalten. |Azure Redis Cache-Metadaten dürfen keine den Exportbestimmungen unterliegenden Daten enthalten. Geben Sie in die folgenden Felder keine regulierten/kontrollierten Daten ein: Cachename, VNET-Name, Abonnementname, Ressourcengruppen, Ressourcentags, Redis-Eigenschaften. |

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen und Updates erhalten Sie, indem Sie den <a href="https://blogs.msdn.microsoft.com/azuregov/">Microsoft Azure Government-Blog</a> abonnieren.




<!--HONumber=Nov16_HO3-->



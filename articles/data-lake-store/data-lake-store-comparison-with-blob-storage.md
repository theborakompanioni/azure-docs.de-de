---
title: Vergleich von Azure Data Lake Store mit Azure Storage Blob | Microsoft Docs
description: Vergleich von Azure Data Lake Store mit Azure Storage Blob
services: data-lake-store
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
ms.assetid: b199525b-84de-4f79-9eb6-69a613b8b217
ms.service: data-lake-store
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 01/03/2017
ms.author: nitinme
translationtype: Human Translation
ms.sourcegitcommit: ab9c4bc6e2e68a3522bbc3fe23ea33760f03b620
ms.openlocfilehash: 10926263ee2657dc96fb1873733d349cf0956e92
ms.lasthandoff: 01/05/2017


---
# <a name="comparing-azure-data-lake-store-and-azure-blob-storage"></a>Vergleich von Azure Data Lake Store und Azure Blob Storage
In der Tabelle in diesem Artikel werden die Unterschiede zwischen Azure Data Lake Store und Azure Blob Storage zusammengefasst und einige wichtige Aspekte der Big Data-Verarbeitung aufgezeigt. Azure Blob Storage ist ein universell einsetzbarer und skalierbarer Objektspeicher, der auf eine Vielzahl von Speicherszenarien ausgelegt ist. Azure Data Lake Store ist ein hochgradig skalierbares Repository, das für Big Data-Analyseworkloads optimiert ist.

|  | Azure Data Lake Store | Azure Blob Storage |
| --- | --- | --- |
| Zweck |Optimierter Speicher für Big Data-Analyseworkloads |Universell einsetzbarer Objektspeicher für eine Vielzahl von Speicherszenarien |
| Anwendungsfälle |Batch-, interaktive Streaming Analytics- und Machine Learning-Daten wie beispielsweise Protokolldateien, IoT-Daten, Clickstreams, große Datasets |Jede Art von Text oder Binärdaten, beispielsweise Daten vom Anwendungs-Back-End, Sicherungsdaten, Medienspeicher für Streaming- und allgemeine Daten |
| Wichtige Begriffe |Das Data Lake Store-Konto enthält Ordner, die wiederum als Dateien gespeicherte Daten enthalten. |Das Speicherkonto enthält Container, die wiederum Daten in Form von Blobs enthalten. |
| Strukturdefinition |Hierarchisches Dateisystem |Objektspeicher mit flachem Namespace |
| API |REST-API über HTTPS |REST-API über HTTP/HTTPS |
| Serverseitige API |[WebHDFS-kompatible REST-API](https://msdn.microsoft.com/library/azure/mt693424.aspx) |[Azure Blob Storage-REST-API](https://msdn.microsoft.com/library/azure/dd135733.aspx) |
| Hadoop-Dateisystemclient |Ja |Ja |
| Datenvorgänge – Authentifizierung |Basierend auf [Azure Active Directory-Identitäten](../active-directory/active-directory-authentication-scenarios.md) |Basierend auf gemeinsamen Geheimnissen – [Kontozugriffsschlüssel](../storage/storage-create-storage-account.md#manage-your-storage-account) und [Shared Access Signature-Schlüssel](../storage/storage-dotnet-shared-access-signature-part-1.md). |
| Datenvorgänge – Authentifizierungsprotokoll |OAuth 2.0. Aufrufe müssen ein gültiges, über Azure Active Directory ausgestelltes JWT (JSON Web Token) enthalten. |Hashbasierter Nachrichtenauthentifizierungscode (Hashed Message Authentication Code, HMAC). Aufrufe müssen einen Base64-codierten SHA-256-Hash über einen Teil der HTTP-Anforderung enthalten. |
| Datenvorgänge – Autorisierung |POSIX-Zugriffssteuerungslisten (ACLs).  Auf Azure Active Directory-Identitäten basierende ACLs können auf Datei- und Ordnerebene festgelegt werden. |Für die Autorisierung auf Kontoebene – Verwenden Sie [Zugriffsschlüssel](../storage/storage-create-storage-account.md#manage-your-storage-account)<br>Für Konto-, Container- oder Blobautorisierung – Verwenden Sie [Shared Access Signature-Schlüssel](../storage/storage-dotnet-shared-access-signature-part-1.md) |
| Datenvorgänge – Überwachung |Verfügbar. Weitere Informationen finden Sie [hier](data-lake-store-diagnostic-logs.md) . |Verfügbar |
| Verschlüsselung für ruhende Daten |Transparent, serverseitig <ul><li>Mit dienstverwalteten Schlüsseln</li><li>Mit kundenverwalteten Schlüsseln in Azure Key Vault</li></ul> |<ul><li>Transparent, serverseitig</li> <ul><li>Mit dienstverwalteten Schlüsseln</li><li>Mit kundenverwalteten Schlüsseln in Azure Key Vault (bald verfügbar)</li></ul><li>clientseitiger Verschlüsselung</li></ul> |
| Verwaltungsvorgänge (z.B. Kontoerstellung) |[Rollenbasierte Zugriffssteuerung](../active-directory/role-based-access-control-what-is.md) (Role-Based Access Control, RBAC) von Azure zur Kontoverwaltung |[Rollenbasierte Zugriffssteuerung](../active-directory/role-based-access-control-what-is.md) (Role-Based Access Control, RBAC) von Azure zur Kontoverwaltung |
| Entwickler-SDKs |.NET, Java, Python, Node.js |.Net, Java, Python, Node.js, C++, Ruby |
| Leistung von Analyseworkloads |Optimierte Leistung für parallele Analyseworkloads. Hoher Durchsatz, hohe IOPS. |Nicht für Analyseworkloads optimiert. |
| Größenbeschränkungen |Keine Beschränkungen für Kontogrößen, Dateigrößen oder die Anzahl von Dateien. |Die geltenden Einschränkungen sind [hier](../azure-subscription-service-limits.md#storage-limits) |
| Georedundanz |Lokal redundant (mehrere Kopien von Daten in einer Azure-Region) |Lokal redundant (LRS), global redundant (GRS), global redundant mit Lesezugriff (RA-GRS). Weitere Informationen finden Sie [hier](../storage/storage-redundancy.md) . |
| Dienstzustand |Allgemein verfügbar |Allgemein verfügbar |
| Regionale Verfügbarkeit |Siehe [hier](https://azure.microsoft.com/regions/#services) |Siehe [hier](https://azure.microsoft.com/regions/#services) |
| Preis |Siehe [Preise](https://azure.microsoft.com/pricing/details/data-lake-store/) |Siehe [Preise](https://azure.microsoft.com/pricing/details/storage/) |

### <a name="next-steps"></a>Nächste Schritte
* [Übersicht über Azure Data Lake-Speicher](data-lake-store-overview.md)
* [Erste Schritte mit dem Data Lake-Speicher](data-lake-store-get-started-portal.md)



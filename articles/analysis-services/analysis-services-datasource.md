---
title: "In Azure Analysis Services unterstützte Datenquellen | Microsoft-Dokumentation"
description: "Beschreibt Datenquellen, die für Datenmodelle in Azure Analysis Services unterstützt werden."
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 6ec63319-ff9b-4b01-a1cd-274481dc8995
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 06/01/2017
ms.author: owend
ms.translationtype: Human Translation
ms.sourcegitcommit: 43aab8d52e854636f7ea2ff3aae50d7827735cc7
ms.openlocfilehash: a0fe91568d747148b3940e9c90db15481c765a9c
ms.contentlocale: de-de
ms.lasthandoff: 06/03/2017


---
# <a name="data-sources-supported-in-azure-analysis-services"></a>In Azure Analysis Services unterstützte Datenquellen
Azure Analysis Services-Server unterstützen das Herstellen von Verbindungen mit lokalen Datenquellen in der Organisation oder mit Datenquellen in der Cloud. Weitere unterstützte Datenquellen werden ständig hinzugefügt. Das Zurückkehren zu dieser Seite lohnt sich. 

Die folgenden Datenquellen werden derzeit unterstützt:

| Cloud  |
|---|
| Azure Blob Storage*  |
| Azure SQL-Datenbank  |
| Azure Data Warehouse |


| Lokal  |   |   |   |
|---|---|---|---|
| Access-Datenbank  | Ordner* | Oracle-Datenbank  | Teradata-Datenbank |
| Active Directory*  | JSON-Dokument*  | Postgre SQL-Datenbank*  |XML-Tabelle* |
| Analysis Services  | Zeilen aus Binärdatei*  | SAP HANA*  |
| Analytics Platform System  | MySQL Database  | SAP Business Warehouse*  | |
| Dynamics CRM*  | OData-Feed*  | SharePoint*  |
| Excel-Arbeitsmappe  | ODBC-Abfrage  | SQL-Datenbank  |
| Exchange*  | OLE DB  | Sybase-Datenbank  |

\* nur tabellarische Modelle mit Kompatibilitätsgrad 1400. 

> [!IMPORTANT]
> Das Herstellen einer Verbindung mit lokalen Datenquellen erfordert ein auf einem Computer in Ihrer Umgebung installiertes [lokales Datengateway](analysis-services-gateway.md).

## <a name="data-providers"></a>Datenanbieter

Datenmodelle in Azure Analysis Services erfordern möglicherweise verschiedene Datenanbieter beim Verbinden mit bestimmten Datenquellen. Gelegentlich kann es vorkommen, dass tabellarische Modelle beim Herstellen einer Verbindung mit Datenquellen mithilfe von nativen Anbietern wie SQL Server Native Client (SQLNCLI11) einen Fehler zurückgeben.

Wenn Datenmodelle eine Verbindung mit einer Clouddatenquelle wie Azure SQL-Datenbank herstellen und dabei einen anderen nativen Anbieter als SQLOLEDB verwenden, wird möglicherweise folgende Fehlermeldung angezeigt: **„The provider 'SQLNCLI11.1' is not registered“** (Der Anbieter ‚SQLNCLI11.1‘ ist nicht registriert). Wenn ein Direktabfrage-Modell eine Verbindung mit lokalen Datenquellen herstellt und dabei native Anbieter verwendet, wird möglicherweise folgende Fehlermeldung angezeigt: **„Error creating OLE DB row set. Incorrect syntax near 'LIMIT'.“ (Fehler beim Erstellen eines OLE DB-Rowsets. Falsche Syntax bei 'LIMIT'.)**.

Die folgenden Datenquellenanbieter werden für In-Memory- oder Direktabfrage-Datenmodelle beim Herstellen einer Verbindung mit lokalen Datenquellen oder Clouddatenquellen unterstützt:

### <a name="cloud"></a>Cloud
| **Datenquelle** | **In-Memory** | **DirectQuery** |
|  --- | --- | --- |
| Azure SQL Data Warehouse |.NET Framework-Datenanbieter für SQL Server |.NET Framework-Datenanbieter für SQL Server |
| Azure SQL-Datenbank |.NET Framework-Datenanbieter für SQL Server |.NET Framework-Datenanbieter für SQL Server | |

### <a name="on-premises-via-gateway"></a>Lokal (über Gateway)
|**Datenquelle** | **In-Memory** | **DirectQuery** |
|  --- | --- | --- |
| SQL Server |SQL Server Native Client 11.0 |.NET Framework-Datenanbieter für SQL Server |
| SQL Server |Microsoft OLE DB-Anbieter für SQL Server |.NET Framework-Datenanbieter für SQL Server | |
| SQL Server |.NET Framework-Datenanbieter für SQL Server |.NET Framework-Datenanbieter für SQL Server | |
| Oracle |Microsoft OLE DB-Anbieter für Oracle |Oracle-Datenanbieter für .NET | |
| Oracle |Oracle-Datenanbieter für .NET |Oracle-Datenanbieter für .NET | |
| Teradata |OLE DB-Anbieter für Teradata |Teradata-Datenanbieter für .NET | |
| Teradata |Teradata-Datenanbieter für .NET |Teradata-Datenanbieter für .NET | |
| Analytics Platform System |.NET Framework-Datenanbieter für SQL Server |.NET Framework-Datenanbieter für SQL Server | |

> [!NOTE]
> Bei Verwendung von lokalen Gateways müssen 64-Bit-Anbieter installiert sein.
> 
> 

Bei der Migration eines lokalen SQL Server Analysis Services-Tabellenmodells zu Azure Analysis Services muss möglicherweise der Anbieter gewechselt werden.

**So geben Sie einen Datenquellenanbieter an**

1. Klicken Sie unter SSDT > **Tabular Model Explorer** (Explorer für tabellarisches Modell)  > **Datenquellen** mit der rechten Maustaste auf eine Datenquellenverbindung, und klicken Sie anschließend auf **Datenquelle bearbeiten**.
2. Klicken Sie unter **Verbindung bearbeiten** auf **Erweitert**, um das Fenster „Erweiterte Eigenschaften“ zu öffnen.
3. Wählen Sie anschließend unter **Erweiterte Eigenschaften festlegen** > **Anbieter** den gewünschten Anbieter aus.

## <a name="impersonation"></a>Identitätswechsel
In einigen Fällen muss möglicherweise ein anderes Identitätswechselkonto angegeben werden. Das Identitätswechselkonto kann in SSDT oder SSMS angegeben werden.

Für lokale Datenquellen:

* Wenn Sie die SQL-Authentifizierung verwenden, sollte das Identitätswechselkonto ein Dienstkonto sein.
* Wenn Sie die Windows-Authentifizierung verwenden, legen Sie Windows-Benutzername/-Kennwort fest. Bei SQL Server wird die Windows-Authentifizierung mit einem bestimmten Identitätswechselkonto nur für In-Memory-Datenmodelle unterstützt.

Für Clouddatenquellen:

* Wenn Sie die SQL-Authentifizierung verwenden, sollte das Identitätswechselkonto ein Dienstkonto sein.

## <a name="next-steps"></a>Nächste Schritte
Wenn Sie lokale Datenquellen verwenden, müssen Sie das [lokale Gateway](analysis-services-gateway.md) installieren.   
Weitere Informationen zum Verwalten des Servers in SSDT oder SSMS finden Sie unter [Verwalten des Servers](analysis-services-manage.md).



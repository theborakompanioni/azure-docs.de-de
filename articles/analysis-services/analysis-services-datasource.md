---
title: Datenquellenverbindungen | Microsoft Docs
description: "Beschreibt die Datenquellenverbindungen für Datenmodelle in Azure Analysis Services."
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
ms.date: 01/20/2017
ms.author: owend
translationtype: Human Translation
ms.sourcegitcommit: a287ebd634a9305229424d0efea266146f88a952
ms.openlocfilehash: 234032630cb3911deb7c7d32cfc4963ad6aee43f


---
# <a name="datasource-connections"></a>Datenquellenverbindungen
Datenmodelle in Azure Analysis Services erfordern möglicherweise verschiedene Datenanbieter beim Verbinden mit bestimmten Datenquellen. Gelegentlich kann es vorkommen, dass tabellarische Modelle beim Herstellen einer Verbindung mit Datenquellen mithilfe von nativen Anbietern wie SQL Server Native Client (SQLNCLI11) einen Fehler zurückgeben.

Beispiel: Wenn ein In-Memory- oder Direktabfrage-Datenmodell eine Verbindung mit einer Clouddatenquelle wie Azure SQL-Datenbank herstellt und dabei einen anderen nativen Anbieter als SQLOLEDB verwendet, wird möglicherweise folgende Fehlermeldung angezeigt: **„The provider 'SQLNCLI11.1' is not registered“** (Der Anbieter 'SQLNCLI11.1' ist nicht registriert).

Wenn ein Direktabfrage-Modell eine Verbindung mit lokalen Datenquellen herstellt und dabei native Anbieter verwendet, wird möglicherweise folgende Fehlermeldung angezeigt: **„Error creating OLE DB row set. Incorrect syntax near 'LIMIT'.“ (Fehler beim Erstellen eines OLE DB-Rowsets. Falsche Syntax bei 'LIMIT'.)**.

## <a name="data-source-providers"></a>Datenquellenanbieter
Die folgenden Datenquellenanbieter werden für In-Memory- oder Direktabfrage-Datenmodelle beim Herstellen einer Verbindung mit lokalen Datenquellen oder Clouddatenquellen unterstützt:

### <a name="cloud"></a>Cloud
| **Datenquelle** | **In-Memory** | **Direktabfrage** |
|  --- | --- | --- |
| Azure SQL Data Warehouse |.NET Framework-Datenanbieter für SQL Server |.NET Framework-Datenanbieter für SQL Server |
| Azure SQL-Datenbank |.NET Framework-Datenanbieter für SQL Server |.NET Framework-Datenanbieter für SQL Server | |

### <a name="on-premises-via-gateway"></a>Lokal (über Gateway)
|**Datenquelle** | **In-Memory** | **Direktabfrage** |
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
Wenn Sie lokale Datenquellen verwenden, müssen Sie das [lokale Gateway](analysis-services-gateway.md) installieren. Weitere Informationen zum Verwalten des Servers in SSDT oder SSMS finden Sie unter [Verwalten des Servers](analysis-services-manage.md).




<!--HONumber=Jan17_HO3-->



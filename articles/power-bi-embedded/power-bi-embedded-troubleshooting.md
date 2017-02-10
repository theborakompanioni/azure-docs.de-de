---
title: Problembehandlung bei der Vorschau von Microsoft Power BI Embedded
description: Problembehandlung bei der Vorschau von Microsoft Power BI Embedded
services: power-bi-embedded
documentationcenter: 
author: guyinacube
manager: erikre
editor: 
tags: 
ms.assetid: c8aee652-ed8b-4c66-9c63-f798b7a655b4
ms.service: power-bi-embedded
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: powerbi
ms.date: 01/06/2017
ms.author: asaxton
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 1f4cf4a04e7a921f8e11c94f74d53ac80c231933


---
# <a name="microsoft-power-bi-embedded-preview-troubleshooting"></a>Problembehandlung bei der Vorschau von Microsoft Power BI Embedded
Dieser Artikel enthält Möglichkeiten zur Problembehandlung bei **Power BI Embedded**.

<a name="connection-string"/>

## <a name="setting-sql-server-connection-strings"></a>Festlegen von SQL Server-Verbindungszeichenfolgen
Beim Festlegen einer SQL Server-Verbindungszeichenfolge müssen Sie ein bestimmtes Format befolgen. Unten sehen Sie ein Beispiel für eine Verbindungszeichenfolge für SQL Server.

```
"Persist Security Info=False;Integrated Security=true;Initial Catalog=Northwind;server=(local)"
```

Weitere Informationen zu SQL Server-Verbindungszeichenfolgen finden Sie in den folgenden Artikeln:

* [SQL Server-Verbindungszeichenfolgen](https://msdn.microsoft.com/library/jj653752.aspx)
* [SqlConnection.ConnectionString](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.connectionstring.aspx)

<a name="credentials"/>

## <a name="setting-credentials"></a>Festlegen von Anmeldeinformationen
Falls Sie Anmeldeinformationen für eine Entwicklungs- oder eine Stagingumgebung besitzen, d.h. einen Benutzernamen und ein Kennwort, müssen Sie möglicherweise Anmeldeinformationen aktualisieren, die einer Produktionslösung entsprechen.

## <a name="see-also"></a>Weitere Informationen
* [Erste Schritte mit dem Beispiel](power-bi-embedded-get-started-sample.md)
* [Was ist Power BI Embedded?](power-bi-embedded-what-is-power-bi-embedded.md)




<!--HONumber=Nov16_HO3-->



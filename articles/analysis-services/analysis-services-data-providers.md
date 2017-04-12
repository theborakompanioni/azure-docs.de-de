---
title: "Für Clientverbindungen mit Azure Analysis Services erforderliche Datenanbieter | Microsoft-Dokumentation"
description: "Beschreibt Datenanbieter, die für Clients zum Herstellen einer Verbindung mit Azure Analysis Services erforderlich sind"
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 02/27/2016
ms.author: owend
translationtype: Human Translation
ms.sourcegitcommit: 538f282b28e5f43f43bf6ef28af20a4d8daea369
ms.openlocfilehash: 197e32058908533bc46b2ea2e38ff521a4a1fa3f
ms.lasthandoff: 04/07/2017


---

# <a name="data-providers-for-connecting-to-azure-analysis-services"></a>Datenanbieter zum Herstellen einer Verbindung mit Azure Analysis Services

Datenanbieter, die auch als Clientbibliotheken bezeichnet werden, sind für Clientanwendungen erforderlich, um eine Verbindung mit Analysis Services-Servern herzustellen. 

Analysis Services verwenden drei Datenanbieter. ADOMD.NET und Analysis Services Management Objects (AMO) sind verwaltete Datenanbieter. Der Analysis Services OLE DB-Anbieter (MSOLAP DLL) ist ein nativer Datenanbieter. In der Regel werden alle drei Anbieter gleichzeitig installiert. Für Azure Analysis Services sind die neuesten Versionen der Datenanbieter erforderlich. 

Microsoft-Clientanwendungen wie Power BI Desktop und Excel installieren die drei Datenanbieter. Abhängig von der Version von Excel oder davon, ob neuere Versionen von Excel und Power BI Desktop monatlich aktualisiert werden, werden die installierten Datenanbietern jedoch möglicherweise nicht auf die neuesten Versionen aktualisiert, die für Azure Analysis Services erforderlich sind. Dies gilt auch für benutzerdefinierte Anwendungen oder andere Schnittstellen wie AsCmd, TOM, ADOMD.NET. Für diese Anwendungen müssen die Anbieter manuell installiert werden. Die Datenanbieter für die manuelle Installation sind in Feature Packs für SQL Server als verteilbare Pakete enthalten. Allerdings sind sie mit der SQL Server-Version verknüpft, und sie weisen möglicherweise nicht die neuesten Versionen auf.  

Datenanbieter für Clientverbindungen unterscheiden sich von Datenanbietern, die für die Verbindung eines Azure Analysis Services-Server mit einer Datenquelle erforderlich sind. Weitere Informationen zu den Datenquellenverbindungen finden Sie unter [Datenquellenverbindungen](analysis-services-datasource.md).

## <a name="download-the-latest-preview-data-providers"></a>Herunterladen der neuesten **Vorschau** Datenanbieter  
Verwenden Sie die folgenden Datenanbietern, um die neuesten Programmfehlerbehebungen und Updates zu erhalten. Dieser Datenanbieter sind geeignet, wenn eine Verbindung mit der Vorschau des Azure Analysis Services oder SQL Server vNext Analysis Services hergestellt wird.

[MSOLAP (amd64) Vorschau](http://download.microsoft.com/download/4/8/2/482E5799-9B8E-4724-8A4C-F301BAE788EE/14.0.500.170/amd64/SQL_AS_OLEDB.msi)</br>
[MSOLAP (x86) Vorschau](http://download.microsoft.com/download/4/8/2/482E5799-9B8E-4724-8A4C-F301BAE788EE/14.0.500.170/x86/SQL_AS_OLEDB.msi)</br>
[AMO Vorschau](http://download.microsoft.com/download/4/8/2/482E5799-9B8E-4724-8A4C-F301BAE788EE/14.0.500.170/SQL_AS_AMO.msi)</br>
[ADOMD Vorschau](http://download.microsoft.com/download/4/8/2/482E5799-9B8E-4724-8A4C-F301BAE788EE/14.0.500.170/SQL_AS_ADOMD.msi)</br>

## <a name="download-the-latest-rtm-data-providers"></a>Herunterladen der neuesten **RTM** Datenanbieter  
Verwenden Sie die folgenden Datenanbieter, wenn Sie sich in einer Produktionsumgebung befinden und vollständig freigegebene und unterstützte Versionen benötigen.

[MSOLAP (amd64)](https://go.microsoft.com/fwlink/?linkid=829576)</br>
[MSOLAP (x86)](https://go.microsoft.com/fwlink/?linkid=829575)</br>
[AMO](https://go.microsoft.com/fwlink/?linkid=829578)</br>
[ADOMD](https://go.microsoft.com/fwlink/?linkid=829577)</br>

## <a name="next-steps"></a>Nächste Schritte
Wenn die neuesten Datenanbieter installiert sind, kann die Clientanwendung eine Verbindung mit einem Server herstellen. Weitere Informationen zum Herstellen einer Verbindung von einem Client finden Sie unter [Abrufen von Daten aus Azure Analysis Services](analysis-services-connect.md).


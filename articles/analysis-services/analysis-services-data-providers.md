---
title: Zum Herstellen einer Verbindung mit Azure Analysis Services erforderliche Clientbibliotheken | Microsoft-Dokumentation
description: "Beschreibt für Clientanwendungen und Tools erforderliche Clientbibliotheken zum Herstellen einer Verbindung mit Azure Analysis Services"
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
ms.date: 04/14/2016
ms.author: owend
translationtype: Human Translation
ms.sourcegitcommit: 8c4e33a63f39d22c336efd9d77def098bd4fa0df
ms.openlocfilehash: c29a6627f712b9d89ac65e845f3ccb4fb87bf8fb
ms.lasthandoff: 04/20/2017


---

# <a name="client-libraries-for-connecting-to-azure-analysis-services"></a>Clientbibliotheken zum Herstellen einer Verbindung mit Azure Analysis Services

Damit Clientanwendungen und Tools eine Verbindung mit Analysis Services-Servern herstellen können, sind Clientbibliotheken erforderlich. 

Analysis Services verwenden drei Clientbibliotheken. ADOMD.NET und Analysis Services Management Objects (AMO) sind verwaltete Clientbibliotheken. Der Analysis Services OLE DB-Anbieter (MSOLAP DLL) ist eine native Clientbibliothek. In der Regel werden alle drei gleichzeitig installiert. Für Azure Analysis Services sind die neuesten Versionen erforderlich. 

Microsoft-Clientanwendungen wie Power BI Desktop und Excel installieren die drei Clientbibliotheken. Abhängig von der Version von Excel oder davon, ob neuere Versionen von Excel und Power BI Desktop monatlich aktualisiert werden, werden die installierten Clientbibliotheken jedoch möglicherweise nicht auf die neuesten Versionen aktualisiert, die für Azure Analysis Services erforderlich sind. Dies gilt auch für benutzerdefinierte Anwendungen oder andere Schnittstellen wie AsCmd, TOM, ADOMD.NET. Für diese Anwendungen müssen die Bibliotheken manuell installiert werden. Die Clientbibliotheken für die manuelle Installation sind in Feature Packs für SQL Server als verteilbare Pakete enthalten. Allerdings sind sie mit der SQL Server-Version verknüpft, und sie weisen möglicherweise nicht die neuesten Versionen auf.  

Clientbibliotheken für Clientverbindungen unterscheiden sich von Datenanbietern, die für die Verbindung eines Azure Analysis Services-Servers mit einer Datenquelle erforderlich sind. Weitere Informationen zu den Datenquellenverbindungen finden Sie unter [Datenquellenverbindungen](analysis-services-datasource.md).

## <a name="download-the-latest-preview-client-libraries"></a>Herunterladen der neuesten **Vorschau**-Clientbibliotheken  
Verwenden Sie die folgenden Clientbibliotheken, um die neuesten Programmfehlerbehebungen und Updates zu erhalten. Diese sind geeignet, wenn eine Verbindung mit Azure Analysis Services oder mit SQL Server vNext Analysis Services hergestellt wird.

[MSOLAP (amd64) Vorschau](http://download.microsoft.com/download/4/8/2/482E5799-9B8E-4724-8A4C-F301BAE788EE/14.0.500.170/amd64/SQL_AS_OLEDB.msi)</br>
[MSOLAP (x86) Vorschau](http://download.microsoft.com/download/4/8/2/482E5799-9B8E-4724-8A4C-F301BAE788EE/14.0.500.170/x86/SQL_AS_OLEDB.msi)</br>
[AMO Vorschau](http://download.microsoft.com/download/4/8/2/482E5799-9B8E-4724-8A4C-F301BAE788EE/14.0.500.170/SQL_AS_AMO.msi)</br>
[ADOMD Vorschau](http://download.microsoft.com/download/4/8/2/482E5799-9B8E-4724-8A4C-F301BAE788EE/14.0.500.170/SQL_AS_ADOMD.msi)</br>

## <a name="download-the-latest-rtm-client-libraries"></a>Herunterladen der neuesten **RTM**-Clientbibliotheken  
Verwenden Sie die folgenden Clientbibliotheken, wenn Sie sich in einer Produktionsumgebung befinden und vollständig freigegebene und unterstützte Versionen benötigen.

[MSOLAP (amd64)](https://go.microsoft.com/fwlink/?linkid=829576)</br>
[MSOLAP (x86)](https://go.microsoft.com/fwlink/?linkid=829575)</br>
[AMO](https://go.microsoft.com/fwlink/?linkid=829578)</br>
[ADOMD](https://go.microsoft.com/fwlink/?linkid=829577)</br>

## <a name="next-steps"></a>Nächste Schritte
[Herstellen einer Verbindung mit einem Azure Analysis Services-Server](analysis-services-connect.md)


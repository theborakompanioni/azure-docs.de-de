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
ms.date: 08/15/2017
ms.author: owend
ms.translationtype: HT
ms.sourcegitcommit: 19be73fd0aec3a8f03a7cd83c12cfcc060f6e5e7
ms.openlocfilehash: 949f34158f4fc8ee6d86cf3dd408c3c903234109
ms.contentlocale: de-de
ms.lasthandoff: 07/13/2017

---

# <a name="client-libraries-for-connecting-to-azure-analysis-services"></a>Clientbibliotheken zum Herstellen einer Verbindung mit Azure Analysis Services

Damit Clientanwendungen und Tools eine Verbindung mit Analysis Services-Servern herstellen können, sind Clientbibliotheken erforderlich. 

Analysis Services verwenden drei Clientbibliotheken. ADOMD.NET und Analysis Services Management Objects (AMO) sind verwaltete Clientbibliotheken. Der Analysis Services OLE DB-Anbieter (MSOLAP DLL) ist eine native Clientbibliothek. In der Regel werden alle drei gleichzeitig installiert. Für Azure Analysis Services sind die neuesten Versionen erforderlich. 

Microsoft-Clientanwendungen wie Power BI Desktop und Excel installieren die drei Clientbibliotheken. Abhängig von der Version oder der Updatehäufigkeit weisen die Clientbibliotheken dabei jedoch unter Umständen nicht die neuesten, von Azure Analysis Services benötigten Versionen auf. Dies gilt auch für benutzerdefinierte Anwendungen oder andere Schnittstellen wie AsCmd, TOM, ADOMD.NET. Für diese Anwendungen müssen die Bibliotheken manuell installiert werden. Die Clientbibliotheken für die manuelle Installation sind in SQL Server-Funktionspaketen wie verteilbaren Paketen enthalten. Allerdings sind diese Clientbibliotheken an die SQL Server-Version gebunden und möglicherweise nicht auf dem neuesten Stand.  

Clientbibliotheken für Clientverbindungen unterscheiden sich von Datenanbietern, die für die Verbindung eines Azure Analysis Services-Servers mit einer Datenquelle erforderlich sind. Weitere Informationen zu den Datenquellenverbindungen finden Sie unter [Datenquellenverbindungen](analysis-services-datasource.md).

## <a name="download-the-latest-client-libraries"></a>Herunterladen der neuesten Clientbibliotheken  
Verwenden Sie die folgenden Clientbibliotheken, wenn Sie sich in einer Produktionsumgebung befinden und vollständig freigegebene und unterstützte Versionen benötigen.

[MSOLAP (amd64)](https://go.microsoft.com/fwlink/?linkid=829576)</br>
[MSOLAP (x86)](https://go.microsoft.com/fwlink/?linkid=829575)</br>
[AMO](https://go.microsoft.com/fwlink/?linkid=829578)</br>
[ADOMD](https://go.microsoft.com/fwlink/?linkid=829577)</br>

## <a name="next-steps"></a>Nächste Schritte
[Herstellen einer Verbindung mit Excel](analysis-services-connect-excel.md)    
[Herstellen einer Verbindung mit Power BI](analysis-services-connect-pbi.md)


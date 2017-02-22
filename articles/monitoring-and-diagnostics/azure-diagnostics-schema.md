---
title: "Liste der Konfigurationsschemaversionen für Azure-Diagnose | Microsoft-Dokumentation"
description: Wird zum Konfigurieren der Erfassung von Leistungsindikatoren in Azure Virtual Machines, VM-Skalierungsgruppen, Service Fabric und Cloud Services verwendet.
services: multiple
documentationcenter: .net
author: rboucher
manager: carmonm
editor: 
ms.assetid: 
ms.service: multiple
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: article
ms.date: 01/23/2017
ms.author: robb
translationtype: Human Translation
ms.sourcegitcommit: 8c9c9dea1248205aa6303e11e1166d5d38786c1b
ms.openlocfilehash: 2e1bc45b55cd79af8579a5ddaf43cf0db019c92f


---
# <a name="list-of-azure-diagnostics-versions"></a>Liste der Azure-Diagnose-Versionen
Diese Seite enthält einen Index für die Schemaversionen der Azure-Diagnose, die zusammen mit dem Microsoft Azure SDK bereitgestellt werden.  

> [!NOTE]
> Azure-Diagnose ist die Komponente, die zum Erfassen von Leistungsindikatoren und anderen Statistiken aus Azure Virtual Machines, VM-Skalierungsgruppen, Service Fabric und Cloud Services verwendet wird.  Diese Seite ist nur relevant, wenn Sie einen dieser Dienste verwenden.
>

Azure-Diagnose wird zusammen mit anderen Microsoft-Diagnoseprodukten wie Azure Monitor, Application Insights und Log Analytics verwendet.

## <a name="azure-sdk-and-diagnostics-versions-shipping-chart"></a>Azure SDK- und Diagnoseversionen – Bereitstellungsdiagramm  

|Azure SDK-Version | Version von Azure-Diagnose | Modell|  
|------------------|---------------------------|------|  
|1.x               |1,0                         | Plug-In|  
|2.0–2.4         |1,0                         |"|  
|2.5               |1.2                         |Erweiterung|  
|2.6               |1.3                         |"|  
|2.7               |1.4                         |"|  
|2.8               |1.5                         |"|  

 Version 1.0 der Azure-Diagnose wurde zunächst über ein Plug-In-Modell bereitgestellt. Bei der Installation des Azure SDKs haben Sie also auch die Version der Azure-Diagnose erhalten.  

 Ab SDK 2.5 (Diagnose-Version 1.2) wurde für die Azure-Diagnose ein Erweiterungsmodell verwendet. Die Tools für die Nutzung neuer Features standen zwar erst in neueren Azure SDKs zur Verfügung, trotzdem erhielt jeder Clouddienst oder virtuelle Computer, der die Diagnose verwendet, die neueste Version direkt von Azure.  

 So wurde bei jedem Benutzer, der noch die SDK-Version 2.5 verwendet, unabhängig von der Nutzung der neueren Features die Diagnoseversion 1.5 geladen.  

## <a name="azure-diagnostics-schemas-index"></a>Schemaindex für die Azure-Diagnose  
[Konfigurationsschema für Diagnoseversion 1.0](azure-diagnostics-schema-1dot0.md)  

[Konfigurationsschema für Diagnoseversion 1.2](azure-diagnostics-schema-1dot2.md)  

[Konfigurationsschema für Diagnoseversionen 1.3 bis 1.5](azure-diagnostics-schema-1dot3-to-1dot5.md)  



<!--HONumber=Jan17_HO5-->



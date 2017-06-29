---
title: "Azure Analysis Services-Tutorial – Lektion 7: Erstellen von Key Performance Indicators | Microsoft-Dokumentation"
description: Dieser Artikel beschreibt, wie Key Performance Indicators im Azure Analysis Services-Tutorialprojekt erstellt werden.
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 05/26/2017
ms.author: owend
ms.translationtype: Human Translation
ms.sourcegitcommit: 43aab8d52e854636f7ea2ff3aae50d7827735cc7
ms.openlocfilehash: d78808421dd5acd907aa9e9000bb3b770a42c061
ms.contentlocale: de-de
ms.lasthandoff: 06/03/2017

---
# <a name="lesson-7-create-key-performance-indicators"></a>Lektion 7: Erstellen von Key Performance Indicators

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

In dieser Lektion erstellen Sie Key Performance Indicators (KPIs). KPIs werden verwendet, um die Leistung eines Werts zu messen, der von dem *Base*-Measure definiert wird. Dies geschieht anhand eines *Target*-Werts, der ebenfalls von einem Measure oder einem absoluten Wert definiert wird. Mit KPIs können Geschäftsleuten in Berichterstellungsclientanwendungen Zusammenfassungen von Geschäftserfolgen schnell und einfach verstehen bzw. Trends erkennen. Weitere Informationen finden Sie unter [KPIs](https://docs.microsoft.com/sql/analysis-services/tabular-models/kpis-ssas-tabular)
  
Geschätzte Zeit zum Bearbeiten dieser Lektion: **15 Minuten**  
  
## <a name="prerequisites"></a>Voraussetzungen  
Dieses Thema ist Teil eines Tutorials zur Tabellenmodellierung, das in der richtigen Reihenfolge absolviert werden sollte. Bevor Sie diese Lektion beginnen, sollten Sie die vorherige [Lektion 6: Erstellen von Measures](../tutorials/aas-lesson-6-create-measures.md) abgeschlossen haben.   
  
## <a name="create-key-performance-indicators"></a>Erstellen von Key Performance Indicators  
  
#### <a name="to-create-an-internetcurrentquartersalesperformance-kpi"></a>So erstellen Sie einen KPI „InternetCurrentQuarterSalesPerformance“  
  
1.  Klicken Sie im Modell-Designer auf die Tabelle **FactInternetSales**.  
  
2.  Klicken Sie im Measureraster auf eine leere Zelle.  
  
3.  Geben Sie in der Bearbeitungsleiste über der Tabelle folgende Formel ein: 
 
    ```  
    InternetCurrentQuarterSalesPerformance :=DIVIDE([InternetCurrentQuarterSales]/[InternetPreviousQuarterSalesProportionToQTD],BLANK())  
    ```

    Dieses Measure fungiert als Basiskennzahl für den KPI.  
  
4.  Klicken Sie mit der rechten Maustaste auf **InternetCurrentQuarterSalesPerformance** > **KPI erstellen**.   
  
5.  Wählen Sie im Dialogfeld „Key Performance Indicator (KPI)“ unter **Target** (Zielwert) **Absolute Value** (Absoluter Wert) aus, und geben Sie anschließend **1,1** ein.  
  
7.  Geben Sie im linken Schiebereglerfeld (niedrig) **1** ein und im rechten Schiebereglerfeld (hoch) **1,07**.  
  
8.  Wählen Sie unter **Symbolart auswählen** die Raute (rot), das Dreieck, (gelb) und den Kreis (grün) aus.
  
    ![aas-lektion7-kpi](../tutorials/media/aas-lesson7-kpi.png)
    
    > [!TIP]  
    > Beachten Sie die erweiterbare Beschriftung **Beschreibungen** unter den verfügbaren Symbolarten. Verwenden Sie Beschreibungen der verschiedenen KPI-Elemente, damit diese in Clientanwendungen besser voneinander unterschieden werden können.  
  
9. Klicken Sie auf **OK**, um den KPI fertigzustellen.  
  
    Beachten Sie im Measureraster das Symbol neben dem Measure **InternetCurrentQuarterSalesPerformance**. Dieses Symbol gibt an, dass dieses Measure der Basewert für einen KPI ist.  
  
#### <a name="to-create-an-internetcurrentquartermarginperformance-kpi"></a>So erstellen Sie einen KPI „InternetCurrentQuarterMarginPerformance“  
  
1.  Klicken Sie im Measureraster der Tabelle **FactInternetSales** auf eine leere Zelle.  
  
2.  Geben Sie in der Bearbeitungsleiste über der Tabelle folgende Formel ein:  

    ```
    InternetCurrentQuarterMarginPerformance :=IF([InternetPreviousQuarterMarginProportionToQTD]<>0,([InternetCurrentQuarterMargin]-[InternetPreviousQuarterMarginProportionToQTD])/[InternetPreviousQuarterMarginProportionToQTD],BLANK())  
    ```
 
3.  Klicken Sie mit der rechten Maustaste auf **InternetCurrentQuarterMarginPerformance** > **KPI erstellen**.  
  
4.  Wählen Sie im Dialogfeld „Key Performance Indicator (KPI)“ unter **Target** (Zielwert) **Absolute Value** (Absoluter Wert) aus, und geben Sie anschließend **1,25** ein.   
  
5.  Schieben Sie den Regler im linken Schiebereglerfeld (niedrig) bis zu **0,8**, und schieben Sie anschließend den Regler im rechten Schiebereglerfeld (hoch) bis zu **1,03**.  
  
6.  Wählen Sie unter **Symbolart auswählen** die Raute (rot), das Dreieck, (gelb) und den Kreis (grün) aus, und klicken Sie anschließend auf **OK**.  
  
## <a name="whats-next"></a>Wie geht es weiter?
[Lektion 8: Erstellen von Perspektiven](../tutorials/aas-lesson-8-create-perspectives.md)
  
  


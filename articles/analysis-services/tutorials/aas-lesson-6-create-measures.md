---
title: "Azure Analysis Services-Tutorial – Lektion 6: Erstellen von Measures | Microsoft-Dokumentation"
description: Dieser Artikel beschreibt, wie Measures im Azure Analysis Services-Tutorialprojekt erstellt werden.
services: analysis-services
documentationcenter: 
author: Minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 
ms.service: analysis-services
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 06/01/2017
ms.author: owend
ms.translationtype: Human Translation
ms.sourcegitcommit: 43aab8d52e854636f7ea2ff3aae50d7827735cc7
ms.openlocfilehash: 90833fa9744eac298b0da82cd3d12f27cc237510
ms.contentlocale: de-de
ms.lasthandoff: 06/03/2017

---
# <a name="lesson-6-create-measures"></a>Lektion 6: Erstellen von Measures

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

In dieser Lektion erstellen Sie Measures, die im Modell enthalten sein sollen. Ähnlich wie die berechneten Spalten, die Sie erstellt haben, ist ein Measure eine Berechnung, die mithilfe einer DAX-Formel erstellt wird. Im Gegensatz zu berechneten Spalten werden Measures anhand eines vom Benutzer ausgewählten *Filters* ausgewertet. Ein Beispiel ist eine bestimmte Spalte oder ein Slicer, die bzw. der im Feld für Zeilenbezeichnungen in einer PivotTable hinzugefügt wurde. Anschließend wird vom angewendeten Measure ein Wert für jede Zelle im Filter berechnet. Measures sind leistungsstarke, flexible Berechnungen, die Sie in fast allen Tabellenmodellen für dynamische Berechnungen von numerischen Daten einschließen können. Weitere Informationen finden Sie unter [Measures](https://docs.microsoft.com/sql/analysis-services/tabular-models/measures-ssas-tabular).
  
Verwenden Sie das *Measureraster*, um Measures zu erstellen. Standardmäßig verfügt jede Tabelle über ein leeres Measureraster; allerdings erstellen Sie normalerweise nicht für jede Tabelle Measures. Das Measureraster wird in der Datenansicht unter einer Tabelle im Modell-Designer in der Datensicht angezeigt. Klicken Sie auf das Menü **Tabelle**, und klicken Sie dann auf **Measureraster anzeigen**, um das Measureraster für eine Tabelle anzuzeigen oder auszublenden.  
  
Sie können ein Measure erstellen, indem Sie auf eine leere Zelle im Measureraster klicken und anschließend eine DAX-Formel in der Bearbeitungsleiste eingeben. Wenn Sie die EINGABETASTE drücken, um die Formel abzuschließen, wird das Measure in der Zelle angezeigt. Sie können auch Measures mithilfe einer Standardaggregationsfunktion erstellen, indem Sie auf eine Spalte und anschließend auf die Schaltfläche AutoSumme (**∑**) auf der Symbolleiste klicken. Measures, die mithilfe der Funktion AutoSumme erstellt wurden, werden im Measureraster direkt unterhalb der Spalte angezeigt, können aber verschoben werden.  
  
In dieser Lektion erstellen Sie Measures sowohl durch Eingabe einer DAX-Formel in der Bearbeitungsleiste als auch mithilfe der Funktion AutoSumme.  
  
Geschätzte Zeit zum Bearbeiten dieser Lektion: **30 Minuten**  
  
## <a name="prerequisites"></a>Voraussetzungen  
Dieses Thema ist Teil eines Tutorials zur Tabellenmodellierung, das in der vorgegebenen Reihenfolge durchgeführt werden sollte. Bevor Sie diese Lektion beginnen, sollten Sie die vorherige [Lektion 5: Erstellen berechneter Spalten](../tutorials/aas-lesson-5-create-calculated-columns.md) abgeschlossen haben.  
  
## <a name="create-measures"></a>Erstellen von Measures  
  
#### <a name="to-create-a-dayscurrentquartertodate-measure-in-the-dimdate-table"></a>So erstellen Sie ein DaysCurrentQuarterToDate-Measure in der DimDate-Tabelle  
  
1.  Klicken Sie im Modell-Designer auf die **DimDate**-Tabelle.  
  
2.  Klicken Sie im Measureraster auf die leere Zelle links oben.  
  
3.  Geben Sie in der Bearbeitungsleiste folgende Formel ein:  
  
    ```
    DaysCurrentQuarterToDate:=COUNTROWS( DATESQTD( 'DimDate'[Date])) 
    ```
  
    Beachten Sie, dass die linke obere Zelle jetzt einen Measurenamen, **DaysCurrentQuarterToDate**, gefolgt von dem Ergebnis **92** enthält.
    
      ![aas-lektion6-neuesmeasure](../tutorials/media/aas-lesson6-newmeasure.png) 
    
    Im Gegensatz zu berechneten Spalten können Sie mit Measureformeln den Measurenamen gefolgt von einem Doppelpunkt, gefolgt vom Formelausdruck eingeben.

  
#### <a name="to-create-a-daysincurrentquarter-measure-in-the-dimdate-table"></a>So erstellen Sie ein DaysInCurrentQuarter-Measure in der DimDate-Tabelle  
  
1.  Klicken Sie, solange die **DimDate**-Tabelle noch im Modell-Designer aktiv ist, im Measureraster auf die leere Zelle unter dem Measure, das Sie erstellt haben.  
  
2.  Geben Sie in der Bearbeitungsleiste folgende Formel ein:  
  
    ```
    DaysInCurrentQuarter:=COUNTROWS( DATESBETWEEN( 'DimDate'[Date], STARTOFQUARTER( LASTDATE('DimDate'[Date])), ENDOFQUARTER('DimDate'[Date])))
    ```
  
    Bei der Erstellung eines Vergleichsverhältnisses zwischen einem nicht abgeschlossenen Zeitraum und dem vorherigen Zeitraum muss die Formel den Anteil des verstrichenen Zeitraums berechnen und mit dem gleichen Anteil des vorherigen Zeitraums vergleichen. In diesem Fall gibt [DaysCurrentQuarterToDate]/[DaysInCurrentQuarter] den Anteil des verstrichenen Zeitraums im aktuellen Zeitraum an.  
  
#### <a name="to-create-an-internetdistinctcountsalesorder-measure-in-the-factinternetsales-table"></a>So erstellen Sie ein InternetDistinctCountSalesOrder-Measure in der FactInternetSales-Tabelle  
  
1.  Klicken Sie auf die **FactInternetSales**-Tabelle.   
  
2.  Klicken Sie auf die Spaltenüberschrift **SalesOrderNumber**.  
  
3.  Klicken Sie in der Symbolleiste auf den Dropdownpfeil neben der Schaltfläche AutoSumme (**∑**), und wählen Sie dann **DistinctCount** aus.  
  
    Die Funktion AutoSumme erstellt mithilfe der Standardaggregationsformel „DistinctCount“ automatisch ein Measure für die ausgewählte Spalte.  
    
       ![aas-lektion6-neuesmeasure2](../tutorials/media/aas-lesson6-newmeasure2.png)
  
4.  Klicken Sie im Measureraster auf das neue Measure, und benennen Sie dann im Fenster **Eigenschaften** unter **Measurename** das Measure in **InternetDistinctCountSalesOrder** um. 
 
  
#### <a name="to-create-additional-measures-in-the-factinternetsales-table"></a>So erstellen Sie zusätzliche Measures in der FactInternetSales-Tabelle  
  
1.  Erstellen Sie mithilfe der Funktion AutoSumme die folgenden Measures und benennen Sie sie:  

    |Column|Measurename|AutoSumme (∑)|Formel|  
    |----------------|----------|-----------------|-----------|  
    |SalesOrderLineNumber|InternetOrderLinesCount|Count|=COUNTA([SalesOrderLineNumber])|  
    |OrderQuantity|InternetTotalUnits|Summe|=SUM([OrderQuantity])|  
    |DiscountAmount|InternetTotalDiscountAmount|Summe|=SUM([DiscountAmount])|  
    |TotalProductCost|InternetTotalProductCost|Summe|=SUM([TotalProductCost])|  
    |SalesAmount|InternetTotalSales|Summe|=SUM([SalesAmount])|  
    |Margin (Spanne)|InternetTotalMargin|Summe|=SUM([Margin])|  
    |TaxAmt|InternetTotalTaxAmt|Summe|=SUM([TaxAmt])|  
    |Freight (Fracht)|InternetTotalFreight|Summe|=SUM([Freight])|  
  
2.  Erstellen Sie die folgenden Measures in dieser Reihenfolge durch Klicken auf eine leere Zelle im Measureraster mithilfe der Bearbeitungsleiste, und benennen Sie sie:  
  
      ```
      InternetPreviousQuarterMargin:=CALCULATE([InternetTotalMargin],PREVIOUSQUARTER('DimDate'[Date]))
      ```
      
      ```
      InternetCurrentQuarterMargin:=TOTALQTD([InternetTotalMargin],'DimDate'[Date])
      ```
  
      ```
      InternetPreviousQuarterMarginProportionToQTD:=[InternetPreviousQuarterMargin]*([DaysCurrentQuarterToDate]/[DaysInCurrentQuarter])
      ```
  
      ```
      InternetPreviousQuarterSales:=CALCULATE([InternetTotalSales],PREVIOUSQUARTER('DimDate'[Date]))
      ```
  
      ```
      InternetCurrentQuarterSales:=TOTALQTD([InternetTotalSales],'DimDate'[Date])
      ```
      
      ```
      InternetPreviousQuarterSalesProportionToQTD:=[InternetPreviousQuarterSales]*([DaysCurrentQuarterToDate]/[DaysInCurrentQuarter])
      ```
  
Measures, die für die FactInternetSales-Tabelle erstellt wurden, können verwendet werden, um kritische finanzielle Daten zu analysieren, z.B. den Absatz, die Kosten und die Gewinnspanne von Elementen, die durch die vom Benutzer ausgewählten Filter definiert wurden.  
  
## <a name="whats-next"></a>Wie geht es weiter?
[Lektion 7: Erstellen von Key Performance Indicators](../tutorials/aas-lesson-7-create-key-performance-indicators.md)  

  


---
title: "Azure Analysis Services-Tutorial – Lektion 5: Erstellen von berechneten Spalten | Microsoft-Dokumentation"
description: Dieser Artikel beschreibt, wie berechnete Spalten im Azure Analysis Services-Tutorialprojekt erstellt werden.
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
ms.date: 05/26/2017
ms.author: owend
ms.translationtype: Human Translation
ms.sourcegitcommit: e72275ffc91559a30720a2b125fbd3d7703484f0
ms.openlocfilehash: 0cce578185ba7811e4b13cc061a2adcb18452b13
ms.contentlocale: de-de
ms.lasthandoff: 05/05/2017

---
# <a name="lesson-5-create-calculated-columns"></a>Lektion 5: Erstellen berechneter Spalten

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

In dieser Lektion erstellen Sie neue Daten in Ihrem Modell, indem Sie berechnete Spalten hinzufügen. Sie können mithilfe des Abfrage-Editors berechnete Spalten (wie benutzerdefinierte Spalten) erstellen, wenn Sie „Daten abrufen“ verwenden, oder zu einem späteren Zeitpunkt im Modell-Designer, wie in diesem Tutorial. Weitere Informationen finden Sie unter [Berechnete Spalten](https://docs.microsoft.com/sql/analysis-services/tabular-models/ssas-calculated-columns).
  
Sie erstellen fünf neue berechnete Spalten in drei verschiedenen Tabellen. Die Schritte sind für jede Aufgabe etwas anders. Hierdurch werden Sie sehen, dass es verschiedene Möglichkeiten gibt, neue Spalten zu erstellen, sie umzubenennen und an verschiedenen Positionen in einer Tabelle zu platzieren.  

Hier benutzen Sie auch zum ersten Mal Data Analysis Expressions (DAX). DAX ist eine spezielle Sprache zum Erstellen von extrem anpassbaren Formelausdrücken für tabellarische Modelle. In diesem Tutorial verwenden Sie DAX zum Erstellen von berechneten Spalten, Measures und Rollenfiltern. Weitere Informationen finden Sie unter [DAX in tabular models (DAX in tabellarischen Modellen)](https://docs.microsoft.com/sql/analysis-services/tabular-models/understanding-dax-in-tabular-models-ssas-tabular). 
  
Geschätzte Zeit zum Bearbeiten dieser Lektion: **15 Minuten**  
  
## <a name="prerequisites"></a>Voraussetzungen  
Dieses Thema ist Teil eines Tutorials zur Tabellenmodellierung, das in der richtigen Reihenfolge absolviert werden sollte. Bevor Sie diese Lektion beginnen, sollten Sie die vorherige Lektion abgeschlossen haben: [Lektion 4: Erstellen von Beziehungen](../tutorials/aas-lesson-4-create-relationships.md). 
  
## <a name="create-calculated-columns"></a>Erstellen berechneter Spalten  
  
#### <a name="create-a-monthcalendar-calculated-column-in-the-dimdate-table"></a>Erstellen einer berechneten MonthCalendar-Spalte in der DimDate-Tabelle  
  
1.  Klicken Sie auf das Menü **Modell** und dann auf **Modellansicht** > **Datenansicht**.  
  
    Berechnete Spalten können nur mithilfe des Modell-Designers in der Datensicht erstellt werden.  
  
2.  Klicken Sie im Modell-Designer auf die **DimDate**-Tabelle (Registerkarte).  
  
3.  Klicken Sie mit der rechten Maustaste auf die Spaltenüberschrift **CalendarQuarter**, und klicken Sie dann auf **Spalte einfügen**.  
  
    Eine neue Spalte mit dem Namen **Calculated Column 1**  (Berechnete Spalte 1) wird auf der linken Seite der Spalte **Calendar Quarter** eingefügt.  
  
4.  Geben Sie in der Bearbeitungsleiste über der Tabelle die folgende DAX-Formel ein. Die Funktion AutoVervollständigen unterstützt Sie beim Eingeben des vollqualifizierten Namens von Spalten und Tabellen und listet die verfügbaren Funktionen auf.  
  
    ```  
    =RIGHT(" " & FORMAT([MonthNumberOfYear],"#0"), 2) & " - " & [EnglishMonthName]  
    ``` 
  
    Anschließend werden Werte für alle Zeilen in der berechneten Spalte aufgefüllt. Wenn Sie durch die Tabelle scrollen, werden Sie sehen, dass die Zeilen über verschiedene Werte für diese Spalte verfügen, basierend auf den Daten in jeder Zeile.    
  
5.  Benennen Sie diese Spalte in **MonthCalendar** um. 

    ![aas-lektion5-neuespalte](../tutorials/media/aas-lesson5-newcolumn.png) 
  
Die berechnete Spalte „MonthCalendar“ verfügt über einen sortierbaren Namen für den Monat.  
  
#### <a name="create-a-dayofweek-calculated-column-in-the-dimdate-table"></a>Erstellen Sie eine berechnete DayOfWeek-Spalte in der DimDate-Tabelle  
  
1.  Klicken Sie auf das Menü **Spalte**, während die **DimDate**-Tabelle aktiv ist, und klicken Sie auf **Spalten hinzufügen**.  
  
2.  Geben Sie in der Bearbeitungsleiste folgende Formel ein:  
    
    ```
    =RIGHT(" " & FORMAT([DayNumberOfWeek],"#0"), 2) & " - " & [EnglishDayNameOfWeek]  
    ```
    
    Wenn Sie das Erstellen der Formel abgeschlossen haben, drücken Sie die EINGABETASTE. Die neue Spalte wird ganz rechts zur Tabelle hinzugefügt.  
  
3.  Benennen Sie die Spalte in **DayOfWeek** um.  
  
4.  Klicken Sie auf die Spaltenüberschrift, und ziehen Sie die Spalte dann zwischen die Spalte **EnglishDayNameOfWeek** und die Spalte **DayNumberOfMonth**.  
  
    > [!TIP]  
    > Das Verschieben von Spalten in der Tabelle erleichtert die Navigation.  
  
Die berechnete Spalte „DayOfWeek“ verfügt über einen sortierbaren Namen für den Tag der Woche.  
  
#### <a name="create-a-productsubcategoryname-calculated-column-in-the-dimproduct-table"></a>Erstellen einer berechneten ProductSubcategoryName-Spalte in der DimProduct-Tabelle  
  
  
1.  Scrollen Sie in der **DimProduct**-Tabelle zur rechten Seite. Beachten Sie, dass die Spalte ganz rechts **Spalte hinzufügen** (in Kursivdruck) benannt ist, und klicken Sie auf die Spaltenüberschrift.  
  
2.  Geben Sie in der Bearbeitungsleiste folgende Formel ein.  
    
    ```
    =RELATED('DimProductSubcategory'[EnglishProductSubcategoryName])  
    ```
  
3.  Benennen Sie die Spalte in **ProductSubcategoryName** um.  
  
Die berechnete Spalte „ProductSubcategoryName“ wird zum Erstellen einer Hierarchie in der DimProduct-Tabelle verwendet, die Daten aus der Spalte „EnglishProductSubcategoryName“ in der Tabelle „DimProductSubcategory“ enthält. Hierarchien können nicht mehr als eine Tabelle umfassen. Sie erstellen Hierarchien später in Lektion 9.  
  
#### <a name="create-a-productcategoryname-calculated-column-in-the-dimproduct-table"></a>Erstellen einer berechneten ProductCategoryName-Spalte in der DimProduct-Tabelle  
  
1.  Klicken Sie auf das Menü **Spalte**, während die **DimProduct**-Tabelle aktiv ist, und klicken Sie auf **Spalten hinzufügen**.  
  
2.  Geben Sie in der Bearbeitungsleiste folgende Formel ein:  
  
    ```
    =RELATED('DimProductCategory'[EnglishProductCategoryName]) 
    ```
    
3.  Benennen Sie die Spalte in **ProductCategoryName** um.  
  
Die berechnete Spalte „ProductCategoryName“ wird zum Erstellen einer Hierarchie in der DimProduct-Tabelle verwendet, die Daten aus der Spalte „EnglishProductCategoryName“ in der Tabelle „DimProductCategory“ enthält. Hierarchien können nicht mehr als eine Tabelle umfassen.  
  
#### <a name="create-a-margin-calculated-column-in-the-factinternetsales-table"></a>Erstellen einer berechneten Margin-Spalte in der FactInternetSales-Tabelle  
  
1.  Wählen Sie im Modell-Designer die Tabelle **FactInternetSales** aus.  
  
2.  Erstellen Sie eine neue berechnete Spalte zwischen der Spalte **SalesAmount** und der Spalte **TaxAmt**.  
  
3.  Geben Sie in der Bearbeitungsleiste folgende Formel ein:  
  
    ```
    =[SalesAmount]-[TotalProductCost]
    ``` 

4.  Benennen Sie die Spalte in **Margin** um.  
 
      ![aas-lektion5-neuerrand](../tutorials/media/aas-lesson5-newmargin.png)
      
    Die berechnete Margin-Spalte wird für die Analyse von Gewinnspannen für jeden Verkauf verwendet.  
  
## <a name="whats-next"></a>Wie geht es weiter?
[Lektion 6: Erstellen von Measures](../tutorials/aas-lesson-6-create-measures.md)
  
  
  


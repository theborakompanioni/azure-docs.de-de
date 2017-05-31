---
title: "Azure Analysis Services-Tutorial – Lektion 9: Erstellen von Hierarchien | Microsoft-Dokumentation"
description: 
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
ms.openlocfilehash: abe9edb0cbcdae18964731b1ab90a27117fc0c15
ms.contentlocale: de-de
ms.lasthandoff: 05/05/2017

---
# <a name="lesson-9-create-hierarchies"></a>Lektion 9: Erstellen von Hierarchien

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

In dieser Lektion lernen Sie, wie Sie Hierarchien erstellen können. Hierarchien sind Gruppen von Spalten, die in Ebenen aufgeteilt sind. Eine Hierarchie „Geographie“ kann z.B. in die Ebenen „Land“, „Staat/Bundesland“, „Bezirk“ und „Stadt“ unterteilt sein. Hierarchien können in einer Feldliste einer Berichterstellungsclientanwendung getrennt von anderen Spalten auftreten. So können Clientbenutzer leichter durch diese navigieren und sie in Berichte einbeziehen. Weitere Informationen finden Sie unter [Hierarchien](https://docs.microsoft.com/sql/analysis-services/tabular-models/hierarchies-ssas-tabular).
  
Verwenden Sie den Modell-Designer unter *Diagrammansicht*, um Hierarchien zu erstellen. Das Erstellen und Verwalten von Hierarchien wird in Data View nicht unterstützt.  
  
Geschätzte Zeit zum Bearbeiten dieser Lektion: **20 Minuten**  
  
## <a name="prerequisites"></a>Voraussetzungen  
Dieses Thema ist Teil eines Tutorials zur Tabellenmodellierung, das in der richtigen Reihenfolge absolviert werden sollte. Bevor Sie diese Lektion beginnen, sollten Sie die vorherige [Lektion 8: Erstellen von Perspektiven](../tutorials/aas-lesson-8-create-perspectives.md) abgeschlossen haben.  
  
## <a name="create-hierarchies"></a>Erstellen von Hierarchien  
  
#### <a name="to-create-a-category-hierarchy-in-the-dimproduct-table"></a>So erstellen Sie eine Hierarchie „Category“ in der Tabelle „DimProduct“  
  
1.  Klicken Sie im Modell-Designer (Diagrammansicht) mit der rechten Maustaste auf die Tabelle **DimProduct** und dann auf **Hierarchie erstellen**. Am unteren Rand des Tabellenfensters wird eine neue Hierarchie angezeigt. Benennen Sie die Hierarchie in **Category** um.  
  
2.  Klicken Sie auf die Spalte **ProductCategoryName** und ziehen Sie diese in die neue Hierarchie **Category**.  
  
3.  Klicken Sie in der Hierarchie **Category** mit der rechten Maustaste auf **ProductCategoryName** > **Umbenennen**, und geben Sie anschließend **Category** ein.  
  
    > [!NOTE]  
    > Wenn Sie eine Spalte in der Hierarchie umbenennen, wird diese Namensänderung in der Tabelle nicht übernommen. Eine Spalte in der Hierarchie stellt lediglich eine Repräsentation der Spalte in der Tabelle dar.  
  
4.  Klicken Sie auf die Spalte **ProductSubcategoryName**, und ziehen Sie diese in die Hierarchie **Category**. Benennen Sie diese in **Subcategory** um. 
  
5.  Klicken Sie mit der rechten Maustaste auf die Spalte **ModelName** und dann auf **Zur Hierarchie hinzufügen**, und wählen Sie anschließend **Category** aus. Benennen Sie diese in **Model** um.

6.  Fügen Sie zuletzt **EnglishProductName** der Hierarchie „Category“ hinzu. Benennen Sie diese in **Product** um.  

    ![aas-lektion9-category](../tutorials/media/aas-lesson9-category.png)
  
#### <a name="to-create-hierarchies-in-the-dimdate-table"></a>So erstellen Sie Hierarchien in der Tabelle „DimDate“  
  
1.  Erstellen Sie in der Tabelle **DimDate** eine neue Hierarchie mit dem Namen **Calendar**.  
  
3.  Fügen Sie folgende Spalten in dieser Reihenfolge hinzu:

    *  CalendarYear
    *  CalendarSemester
    *  CalendarQuarter
    *  MonthCalendar
    *  DayNumberOfMonth
    
4.  Erstellen Sie in der Tabelle **DimDate** die Hierarchie **Fiscal**. Fügen Sie folgende Spalten in dieser Reihenfolge hinzu:  
  
    *  FiscalYear
    *  FiscalSemester
    *  FiscalQuarter
    *  MonthCalendar
    *  DayNumberOfMonth
  
5.  Erstellen Sie als Letztes in der Tabelle **DimDate** die Hierarchie **ProductionCalendar**. Fügen Sie folgende Spalten in dieser Reihenfolge hinzu:  
    *  CalendarYear
    *  WeekNumberOfYear
    *  DayNumberOfWeek
  
 ## <a name="whats-next"></a>Wie geht es weiter?
[Lektion 10: Erstellen von Partitionen](../tutorials/aas-lesson-10-create-partitions.md) 
  
  


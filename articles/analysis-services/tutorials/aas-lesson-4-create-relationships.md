---
title: "Azure Analysis Services-Tutorial – Lektion 4: Erstellen von Beziehungen | Microsoft-Dokumentation"
description: Dieser Artikel beschreibt, wie Beziehungen im Azure Analysis Services-Tutorialprojekt erstellt werden.
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
ms.date: 05/26/2017
ms.author: owend
ms.translationtype: Human Translation
ms.sourcegitcommit: 43aab8d52e854636f7ea2ff3aae50d7827735cc7
ms.openlocfilehash: d79af3915c718a79f60e5f589527eb4c2ae8b367
ms.contentlocale: de-de
ms.lasthandoff: 06/03/2017

---
# <a name="lesson-4-create-relationships"></a>Lektion 4: Erstellen von Beziehungen

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

In dieser Lektion erfahren Sie, wie Sie die Beziehungen, die beim Importieren von Daten automatisch erstellt wurden, überprüfen und neue Beziehungen zwischen verschiedenen Tabellen hinzufügen. Eine Beziehung ist eine Verbindung zwischen zwei Tabellen, die festlegt, wie die Daten in diesen Tabellen korreliert werden sollen. Beispielsweise haben die Tabellen „DimProduct“ und „DimProductSubcategory“ eine Beziehung, die auf der Tatsache beruht, dass beide Produkte zu einer Unterkategorie gehören. Weitere Informationen finden Sie unter [Beziehungen](https://docs.microsoft.com/sql/analysis-services/tabular-models/relationships-ssas-tabular).
  
Geschätzte Zeit zum Bearbeiten dieser Lektion: **10 Minuten**  
  
## <a name="prerequisites"></a>Voraussetzungen  
Dieses Thema ist Teil eines Tutorials zur Tabellenmodellierung, das in der vorgegebenen Reihenfolge durchgeführt werden sollte. Bevor Sie diese Lektion beginnen, sollten Sie die vorherige [Lektion 3: Markieren als Datumstabelle](../tutorials/aas-lesson-3-mark-as-date-table.md) abgeschlossen haben. 
  
## <a name="review-existing-relationships-and-add-new-relationships"></a>Überprüfen von vorhandenen Beziehungen und Hinzufügen von neuen Beziehungen  
Wenn Sie Daten mithilfe von Get Data importiert haben, haben Sie sieben Tabellen aus der Datenbank „AdventureWorksDW2014“ abgerufen. Beim Importieren von Daten aus einer relationalen Quelle werden im Allgemeinen vorhandene Beziehungen automatisch zusammen mit den Daten importiert. Bevor Sie jedoch mit der Erstellung Ihres Modells fortfahren, sollten Sie überprüfen, ob die Beziehungen zwischen den Tabellen korrekt sind. In diesem Tutorial fügen Sie außerdem drei neue Beziehungen hinzu.  
  
#### <a name="to-review-existing-relationships"></a>Zum Überprüfen vorhandener Beziehungen, gehen Sie folgendermaßen vor:  
  
1.  Klicken Sie auf das Menü **Modell** > **Modellansicht** > **Diagrammansicht**.  

    Der Modell-Designer wird jetzt in der Diagrammansicht angezeigt, in der alle importierten Tabellen mit dazwischen angeordneten Linien in einem Grafikformat dargestellt werden. Die Linien zwischen den Tabellen stellen die Beziehungen dar, die beim Importieren der Daten automatisch erstellt wurden.
    
    ![AAS-Lektion4-Diagramm](../tutorials/media/aas-lesson4-diagram.png)
  
    Mit dem Steuerelement der Minikarte in der unteren rechten Ecke des Modell-Designers können Sie so viele Tabellen wie möglich anzeigen. Sie können die Tabellen auch per Drag & Drop an andere Positionen verschieben, sie näher zusammenbringen oder in einer bestimmten Reihenfolge anordnen. Das Verschieben der Tabellen hat keinen Einfluss auf die bestehenden Beziehungen zwischen den Tabellen. Zum Anzeigen aller Spalten einer bestimmten Tabelle klicken Sie auf eine Ecke der Tabelle, und ziehen Sie sie bis zur gewünschten Größe.  
  
2.  Klicken Sie auf die durchgezogene Linie zwischen den Tabellen **DimCustomer** und **DimGeography**. Die durchgezogene Linie zwischen diesen beiden Tabellen zeigt an, dass die Beziehung aktiv ist, d.h., sie wird bei der Berechnung von DAX-Formeln standardmäßig verwendet.  
  
    Beachten Sie, dass die Spalte **GeographyKey** der Tabelle **DimCustomer** sowie die Spalte **GeographyKey** der Tabelle **DimGeography** jetzt jeweils in einem Feld angezeigt werden. Diese Spalten werden in der Beziehung verwendet. Die Eigenschaften der Beziehung werden jetzt auch im Fenster **Eigenschaften** angezeigt.  
  
    > [!TIP]  
    > Zusätzlich zum Modell-Designer in der Diagrammansicht können Sie auch das Dialogfeld „Beziehungen verwalten“ verwenden, um die Beziehungen zwischen allen Tabellen in einem Tabellenformat anzuzeigen. Klicken Sie im tabellarischen Modell-Explorer mit der rechten Maustaste auf **Beziehungen** > **Beziehungen verwalten**.
  
3.  Stellen Sie sicher, dass die folgenden Beziehungen beim Importieren der Tabellen aus der Datenbank „AdventureWorksDW“ erstellt wurden:  
  
    |Aktiv|Tabelle|Verknüpfte Nachschlagetabelle|  
    |----------|---------|------------------------|  
    |Ja|**DimCustomer [GeographyKey]**|**DimGeography [GeographyKey]**|  
    |Ja|**DimProduct [ProductSubcategoryKey]**|**DimProductSubcategory [ProductSubcategoryKey]**|  
    |Ja|**DimProductSubcategory [ProductCategoryKey]**|**DimProductCategory [ProductCategoryKey]**|  
    |Ja|**FactInternetSales [CustomerKey]**|**DimCustomer [CustomerKey]**|  
    |Ja|**FactInternetSales [ProductKey]**|**DimProduct [ProductKey]**|  
  
    Fehlen Beziehungen, so stellen Sie sicher, dass Ihr Modell die folgenden Tabellen enthält: DimCustomer, DimDate, DimGeography, DimProduct, DimProductCategory, DimProductSubcategory und FactInternetSales. Werden Tabellen aus derselben Datenquellenverbindung zu unterschiedlichen Zeitpunkten importiert, werden zwischen diesen Tabellen keine Beziehungen erstellt. Diese müssen anschließend manuell erstellt werden.  

### <a name="take-a-closer-look"></a>Ausführlichere Betrachtung
In der Diagrammansicht wird die Beziehung zwischen den Tabellen durch einen Pfeil, ein Sternchen und eine Zahl auf den Linien dargestellt.

![AAS-Lektion4-Linie](../tutorials/media/aas-lesson4-line.png)

Der Pfeil zeigt die Filterrichtung. Das Sternchen zeigt an, dass diese Tabelle die n-Seite in der Kardinalität der Beziehung darstellt. Und die 1 verdeutlicht, dass diese Tabelle die eine Seite der Beziehung darstellt. Wenn Sie eine Beziehung bearbeiten möchten, also beispielsweise die Filterrichtung oder Kardinalität der Beziehung ändern möchten, doppelklicken Sie auf die Beziehungslinie, um das Dialogfeld „Beziehung bearbeiten“ zu öffnen.

![AAS-Lektion4-bearbeiten](../tutorials/media/aas-lesson4-edit.png)

Diese Funktionen gehören zur erweiterten Datenmodellierung und sind nicht Teil dieses Tutorials. Weitere Informationen finden Sie unter [Bi-directional cross filters for tabular models in Analysis Services](https://docs.microsoft.com/sql/analysis-services/tabular-models/bi-directional-cross-filters-tabular-models-analysis-services) (Bidirektionale Kreuzfilter für tabellarische Modelle in Analysis Services).

In einigen Fällen möchten Sie möglicherweise zusätzliche Beziehungen zwischen den Tabellen im Modell erstellen, um eine bestimmte Geschäftslogik zu unterstützen. Für dieses Tutorial müssen Sie drei zusätzliche Beziehungen zwischen den Tabellen „FactInternetSales“ und „DimDate“ erstellen.  
  
#### <a name="to-add-new-relationships-between-tables"></a>Zum Hinzufügen neuer Beziehungen zwischen Tabellen, gehen Sie folgendermaßen vor:  
  
1.  Klicken Sie im Modell-Designer in der Tabelle **FactInternetSales** auf die Spalte **OrderDate**, und halten Sie die Maustaste gedrückt. Ziehen Sie den Cursor zu der Spalte **Date** der Tabelle **DimDate**, und lassen Sie die Maustaste los.  

    Die durchgezogene Linie zeigt an, dass Sie eine aktive Beziehung zwischen der Spalte **OrderDate** der Tabelle **Internet Sales** und der Spalte **Date** der Tabelle **Date** erstellt haben. 
  
      ![AAS-Lektion4-neu](../tutorials/media/aas-lesson4-new.png) 
  
    > [!NOTE]  
    > Beim Erstellen von Beziehungen werden die Kardinalität und die Filterrichtung zwischen der primären Tabelle und der verknüpften Nachschlagetabelle automatisch ausgewählt.  
  
2.  Klicken Sie in der Tabelle **FactInternetSales** auf die Spalte **DueDate**, und halten Sie die Maustaste gedrückt. Ziehen Sie den Cursor zur Spalte **Date** der Tabelle **DimDate**, und lassen Sie die Maustaste los.  
  
    Die gepunktete Linie zeigt an, dass Sie eine inaktive Beziehung zwischen der Spalte **DueDate** der Tabelle **FactInternetSales** und der Spalte **Date** der Tabelle **DimDate** erstellt haben. Zwischen Tabellen können mehrere Beziehungen bestehen, doch nur eine Beziehung kann jeweils aktiv sein. Inaktive Beziehungen können zu aktiven gemacht werden, um spezielle Aggregationen in benutzerdefinierten DAX-Ausdrücken durchzuführen.  
  
3.  Erstellen Sie abschließend eine weitere Beziehung. Klicken Sie in der Tabelle **FactInternetSales** auf die Spalte **ShipDate**, und halten Sie die Maustaste gedrückt. Ziehen Sie den Cursor zur Spalte **Date** der Tabelle **DimDate**, und lassen Sie die Maustaste los.  
    
     ![AAS-Lektion4-neuinaktiv](../tutorials/media/aas-lesson4-newinactive.png)
  
## <a name="whats-next"></a>Wie geht es weiter?
[Lektion 5: Erstellen berechneter Spalten](../tutorials/aas-lesson-5-create-calculated-columns.md)
  
  
  


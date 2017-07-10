---
title: "Ergänzende Lektion zum Azure Analysis Services-Tutorial – Unregelmäßige Hierarchien | Microsoft-Dokumentation"
description: "Dieser Artikel beschreibt, wie Sie unregelmäßige Hierarchien im Azure Analysis Services-Tutorial beseitigen können."
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
ms.sourcegitcommit: e72275ffc91559a30720a2b125fbd3d7703484f0
ms.openlocfilehash: 36acf2b20a4c3acab8050eb9c5489c8ee53e4d4e
ms.contentlocale: de-de
ms.lasthandoff: 05/05/2017

---
<a id="supplemental-lesson---ragged-hierarchies" class="xliff"></a>
# Ergänzende Lektion – Unregelmäßige Hierarchien

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

In dieser ergänzenden Lektion beheben Sie ein häufiges Problem beim Pivotieren von Hierarchien, die leere Werte (Member) auf verschiedenen Ebenen enthalten. Eine derartige Hierarchie ist z.B. eine Organisation, in der ein hochrangiger Manager sowohl Ressortleiter als auch Nicht-Manager als direkte Mitarbeiter hat , oder eine geographische Hierarchie mit den Ebenen Land-Region-Stadt, in der einige Städte keinen übergeordneten Staat oder keine übergeordnete Provinz haben, wie etwa Washington D.C. oder die Vatikanstadt. Wenn eine Hierarchie leere Member aufweist, steigt sie oft zu anderen oder unregelmäßigen Ebenen hinunter.

![aas-lektion-detail-unregelmäßige-hierarchien-tabelle](../tutorials/media/aas-lesson-detail-ragged-hierarchies-table.png)

Tabellarische Modelle mit dem Kompatibilitätsgrad 1400 verfügen über die zusätzliche Eigenschaft **Member ausblenden** für Hierarchien. Die **Standardeinstellung** geht davon aus, dass es auf keiner Ebene leere Member gibt. Die Einstellung **Leere Member ausblenden** schließt leere Member aus einer Hierarchie aus, wenn diese in ein PivotTable-Objekt oder einen Bericht eingefügt werden.  
  
Geschätzte Zeit zum Bearbeiten dieser Lektion: **20 Minuten**  
  
<a id="prerequisites" class="xliff"></a>
## Voraussetzungen  
Dieses ergänzende Thema ist Teil eines Tutorials zur Tabellenmodellierung. Vor dem Ausführen der Aufgaben in dieser ergänzenden Lektion sollten Sie alle vorherigen Lektionen oder das Beispielmodellprojekt „Adventure Works Internet Sales“ abgeschlossen haben. 

Wenn Sie das Projekt „AW Internet Sales“ im Rahmen des Tutorials erstellt haben, enthält Ihr Modell noch keine unregelmäßigen Daten oder Hierarchien. Sie müssen dieses Problem zunächst herbeiführen, um diese ergänzende Lektion durchführen zu können. Dazu fügen Sie einige weitere Tabellen hinzu, erstellen Beziehungen, berechnete Spalten, ein Measure und eine neue Hierarchie „Organization“. Dies nimmt lediglich 15 Minuten in Anspruch. Anschließend können Sie das Problem in wenigen Minuten wieder lösen.  

<a id="add-tables-and-objects" class="xliff"></a>
## Hinzufügen von Tabellen und Objekten
  
<a id="to-add-new-tables-to-your-model" class="xliff"></a>
### So fügen Sie neue Tabellen in Ihrem Modell hinzu
  
1.  Erweitern Sie im tabellarischen Modell-Explorer **Datenquellen**, und klicken Sie anschließend auf Ihre Verbindung und dann auf **Neue Tabelle importieren**.
  
2.  Wählen Sie im Navigator **DimEmployee** und **FactResellerSales** aus, und klicken Sie anschließend auf **OK**.

3.  Klicken Sie im Abfrage-Editor auf **Importieren**.

4.  Erstellen Sie folgende [Beziehungen](../tutorials/aas-lesson-4-create-relationships.md):

    | Tabelle 1           | Column       | Filterrichtung   | Tabelle 2     | Column      | Aktiv |
    |-------------------|--------------|--------------------|-------------|-------------|--------|
    | FactResellerSales | OrderDateKey | Standard            | DimDate     | Datum        | Ja    |
    | FactResellerSales | DueDate      | Standard            | DimDate     | Datum        | Nein     |
    | FactResellerSales | ShipDateKey  | Standard            | DimDate     | Datum        | Nein     |
    | FactResellerSales | ProductKey   | Standard            | DimProduct  | ProductKey  | Ja    |
    | FactResellerSales | EmployeeKey  | Beide Tabellen | DimEmployee | EmployeeKey | Ja    |

5. Erstellen Sie in der Tabelle **DimEmployee** die folgenden [berechneten Spalten](../tutorials/aas-lesson-5-create-calculated-columns.md): 

    **Path** 
    ```
    =PATH([EmployeeKey],[ParentEmployeeKey])
    ```

    **FullName** 
    ```
    =[FirstName] & " " & [MiddleName] & " " & [LastName]
    ```

    **Ebene1** 
    ```
    =LOOKUPVALUE(DimEmployee[FullName],DimEmployee[EmployeeKey],PATHITEM([Path],1,1)) 
    ```

    **Ebene2** 
    ```
    =LOOKUPVALUE(DimEmployee[FullName],DimEmployee[EmployeeKey],PATHITEM([Path],1,2)) 
    ```

    **Ebene3** 
    ```
    =LOOKUPVALUE(DimEmployee[FullName],DimEmployee[EmployeeKey],PATHITEM([Path],1,3)) 
    ```

    **Ebene4** 
    ```
    =LOOKUPVALUE(DimEmployee[FullName],DimEmployee[EmployeeKey],PATHITEM([Path],1,4)) 
    ```

    **Ebene5** 
    ```
    =LOOKUPVALUE(DimEmployee[FullName],DimEmployee[EmployeeKey],PATHITEM([Path],1,5)) 
    ```

6.  Erstellen Sie in der Tabelle **DimEmployee** eine [Hierarchie](../tutorials/aas-lesson-9-create-hierarchies.md) mit dem Namen **Organization**. Fügen Sie folgende Spalten in dieser Reihenfolge hinzu: **Ebene1**, **Ebene2**, **Ebene3**, **Ebene4** und **Ebene5**.

7.  Erstellen Sie in der Tabelle **FactResellerSales** das folgende [Measure](../tutorials/aas-lesson-6-create-measures.md):

    ```
    ResellerTotalSales:=SUM([SalesAmount])
    ```

8.  Verwenden Sie [Analysieren in Excel](../tutorials/aas-lesson-12-analyze-in-excel.md), um Excel zu öffnen und automatisch eine PivotTable zu erstellen.

9.  Fügen Sie unter **PivotTable-Felder** die Hierarchie **Organization** aus der Tabelle **DimEmployee** in **Zeilen** ein. Fügen Sie außerdem das Measure **ResellerTotalSales** aus der Tabelle **FactResellerSales** zu **Werte** hinzu.

    ![aas-lektion-detail-unregelmäßige-hierarchins-pivottable](../tutorials/media/aas-lesson-detail-ragged-hierarchies-pivottable.png)

    Wie Sie in der PivotTable sehen können, zeigt die Hierarchie unregelmäßige Zeilen an. Es gibt viele Zeilen, in denen leere Member angezeigt werden.

<a id="to-fix-the-ragged-hierarchy-by-setting-the-hide-members-property" class="xliff"></a>
## So beseitigen Sie unregelmäßige Hierarchien mit der Eigenschaft „Member ausblenden“

1.  Erweitern Sie unter **tabellarischer Modell-Explorer** **Tabellen** > **DimEmployee** > **Hierarchien** > **Organization**.

2.  Wählen Sie unter **Eigenschaften** > **Member ausblenden** **Leere Member ausblenden** aus. 

    ![aas-lektion-detail-unregelmäßige-hierarchien-memberausblenden](../tutorials/media/aas-lesson-detail-ragged-hierarchies-hidemembers.png)

3.  Aktualisieren Sie in Excel die PivotTable. 

    ![aas-lektion-detail-unregelmäßige-hierarchien-pivottable-aktualisieren](../tutorials/media/aas-lesson-detail-ragged-hierarchies-pivottable-refresh.png)

    Das sieht doch sehr viel besser aus.

<a id="see-also" class="xliff"></a>
## Weitere Informationen   
[Lektion 9: Erstellen von Hierarchien](../tutorials/aas-lesson-9-create-hierarchies.md)  
[Ergänzende Lektion – Dynamische Sicherheit](../tutorials/aas-supplemental-lesson-dynamic-security.md)  
[Ergänzende Lektion – Detailzeilen](../tutorials/aas-supplemental-lesson-detail-rows.md)  

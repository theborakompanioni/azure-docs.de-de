---
title: 'Azure Analysis Services-Tutorial, Lektion 2: Abrufen von Daten | Microsoft-Dokumentation'
description: In diesem Tutorial wird das Abrufen und Importieren von Daten in das Azure Analysis Services-Tutorialprojekt beschrieben.
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
ms.openlocfilehash: e77de4b9a74b528fa8a7ce86424fc14628b2cacc
ms.contentlocale: de-de
ms.lasthandoff: 06/03/2017

---

# <a name="lesson-2-get-data"></a>Lektion 2: Abrufen von Daten

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

In dieser Lektion verwenden Sie Get Data in SSDT zum Herstellen einer Verbindung mit der AdventureWorksDW2014-Beispieldatenbank. Weiterhin wählen Sie Daten aus, erstellen eine Vorschau und einen Filter und führen dann den Import in Ihr Arbeitsbereichsmodell aus.  
  
Mit Get Data können Sie Daten aus einer großen Anzahl von Quellen importieren: Azure SQL Database, Oracle, Sybase, OData Feed, Teradata, Dateien etc. Die Daten können auch mit einem Power Query-M-Formelausdruck abgefragt werden.
  
Geschätzte Zeit zum Bearbeiten dieser Lektion: **10 Minuten**  
  
## <a name="prerequisites"></a>Voraussetzungen  
Dieses Thema ist Teil eines Tutorials zur Tabellenmodellierung, das in der vorgegebenen Reihenfolge durchgeführt werden sollte. Bevor Sie mit den Aufgaben dieser Lektion beginnen, sollten Sie die vorherige Lektion abschließen: [Lektion 1: Erstellen eines neuen tabellarischen Modellprojekts](../tutorials/aas-lesson-1-create-a-new-tabular-model-project.md).  
  
## <a name="create-a-connection"></a>Verbindung herstellen  
  
#### <a name="to-create-a-connection-to-the-adventureworksdw2014-database"></a>Herstellen einer Verbindung mit der AdventureWorksDW2014-Datenbank  
  
1.  Klicken Sie mit der rechten Maustaste im tabellarischen Modell-Explorer auf **Datenquellen** > **Aus Datenquelle importieren**.  
  
    Dadurch wird Get Data aufgerufen, um Sie durch das Herstellen einer Verbindung mit einer Datenquelle zu führen. Wenn der tabellarische Modell-Explorer nicht im **Projektmappen-Explorer**, angezeigt wird, doppelklicken Sie auf **Model.bim**, um das Modell im Designer zu öffnen. 
    
    ![aas-lesson2-getdata](../tutorials/media/aas-lesson2-getdata.png)
  
2.  In Get Data, klicken Sie auf **Datenbank** > **, SQL Server-Datenbank** > **Verbinden**.  
  
3.  Geben Sie im Dialogfeld **SQL Server-Datenbank** in das Feld **Server**den Namen des Servers ein, auf dem Sie die AdventureWorksDW2014-Datenbank installiert haben, und klicken Sie dann auf **Verbinden**.  

4.  Wenn Sie zur Erfassung von Anmeldedaten aufgefordert werden, müssen Sie die Anmeldedaten angeben, die Analysis Services beim Importieren und beim Verarbeiten von Daten für die Verbindung an die Datenquelle verwendet. Wählen Sie im **Identitätswechselmodus** **Identität des Kontos annehmen**, geben Sie dann die Anmeldeinformationen ein, und klicken Sie auf **Verbinden**. Es wird empfohlen, dass Sie ein Konto verwenden, bei dem das Kennwort nicht abläuft.

    ![aas-lesson2-account](../tutorials/media/aas-lesson2-account.png)
  
    > [!NOTE]  
    > Ein Windows-Benutzerkonto mit Kennwort ist die sicherste Methode, um sich mit einer Datenquelle zu verbinden.
  
5.  Wählen Sie im Navigator die **AdventureWorksDW2014**-Datenbank, und klicken Sie dann auf **OK**. Dadurch wird die Verbindung zur Datenbank hergestellt. 
  
6.  Aktivieren Sie im Navigator das Kontrollkästchen für die folgenden Tabellen: **DimCustomer**, **DimDate**, **DimGeography**, **DimProduct**, **DimProductCategory**, **DimProductSubcategory**, und **FactInternetSales**.  

    ![aas-lesson2-select-tables](../tutorials/media/aas-lesson2-select-tables.png)
  
Wenn Sie auf „OK“ geklickt haben, wird der Abfrage-Editor geöffnet. Im nächsten Abschnitt wählen Sie nur die Daten aus, die Sie importieren möchten.

  
## <a name="filter-the-table-data"></a>Filtern der Tabellendaten  
Tabellen in der AdventureWorksDW2014-Beispieldatenbank enthalten Daten, die nicht in Ihr Modell übernommen werden müssen. Um den vom Modell verwendeten In-Memory-Speicher zu sparen, sollten Sie unnötige Daten wenn möglich herausfiltern. Dabei werden einige der Spalten aus den Tabellen herausgefiltert, damit sie nach der Bereitstellung nicht in die Arbeitsbereichsdatenbank oder die Modelldatenbank importiert werden. 
  
#### <a name="to-filter-the-table-data-before-importing"></a>So filtern Sie die Tabellendaten vor dem Importieren  
  
1.  Wählen Sie im Abfrage-Editor die Tabelle **DimCustomer**. Eine Ansicht der DimCustomer-Tabelle in der Datenquelle (der AdventureWorksDWQ2014-Beispieldatenbank) wird angezeigt. 
  
2.  Aktivieren Sie die Mehrfachauswahl (Strg + Klick), und wählen Sie **SpanishEducation**, **FrenchEducation**, **SpanishOccupation**, **FrenchOccupation**. Klicken Sie dann mit der rechten Maustaste, und klicken Sie auf **Spalten entfernen**. 

    ![aas-lesson2-remove-columns](../tutorials/media/aas-lesson2-remove-columns.png)
  
    Da diese Spaltenwerte für die Analyse der Internetverkäufe nicht relevant sind, müssen diese Spalten nicht importiert werden. Durch das Entfernen nicht erforderlicher Spalten wird das Modell kleiner und effizienter.  
  
4.  Filtern Sie die verbleibenden Tabellen, indem Sie die folgenden Spalten aus allen Tabellen entfernen:  
    
    **DimDate**
    
      |Column|  
      |--------|  
      |DateKey|  
      |**SpanishDayNameOfWeek**|  
      |**FrenchDayNameOfWeek**|  
      |**SpanishMonthName**|  
      |**FrenchMonthName**|  
  
    **DimGeography**
  
      |Column|  
      |-------------|  
      |**SpanishCountryRegionName**|  
      |**FrenchCountryRegionName**|  
      |**IpAddressLocator**|  
  
    **DimProduct**
  
      |Column|  
      |-----------|  
      |**SpanishProductName**|  
      |**FrenchProductName**|  
      |**FrenchDescription**|  
      |**ChineseDescription**|  
      |**ArabicDescription**|  
      |**HebrewDescription**|  
      |**ThaiDescription**|  
      |**GermanDescription**|  
      |**JapaneseDescription**|  
      |**TurkishDescription**|  
  
    **DimProductCategory**
  
      |Column|  
      |--------------------|  
      |**SpanishProductCategoryName**|  
      |**FrenchProductCategoryName**|  
  
    **DimProductSubcategory**
  
      |Column|  
      |-----------------------|  
      |**SpanishProductSubcategoryName**|  
      |**FrenchProductSubcategoryName**|  
  
    **FactInternetSales**
  
      |Column|  
      |------------------|  
      |**OrderDateKey**|  
      |**DueDateKey**|  
      |**ShipDateKey**|   
  
## <a name="Import"></a>Importieren Sie die ausgewählten Tabellen und Spaltendaten  
Nachdem Sie nun eine Vorschau der nicht benötigten Daten angezeigt und diese herausgefiltert haben, können Sie den Rest der Daten importieren, wenn Sie dies wünschen. Der Assistent importiert sowohl die Tabellendaten als auch sämtliche Beziehungen zwischen den Tabellen. Im Modell werden neue Tabellen und Spalten erstellt, und die von Ihnen herausgefilterten Daten werden nicht importiert.  
  
#### <a name="to-import-the-selected-tables-and-column-data"></a>So importieren Sie die ausgewählten Tabellen und Spaltendaten  
  
1.  Überprüfen Sie Ihre Auswahl. Wenn alles korrekt erscheint, klicken Sie auf **Importieren**. Im Dialogfeld „Datenverarbeitung“ wird der Status der aus Ihrer Datenquelle in Ihre Arbeitsbereichsdatenbank importierten Daten angezeigt.
  
    ![aas-lesson2-success](../tutorials/media/aas-lesson2-success.png) 
  
2.  Klicken Sie auf **Schließen**.  

  
## <a name="save-your-model-project"></a>Speichern Ihres Modellprojekts  
Es ist wichtig, das Modellprojekt häufig zu speichern.  
  
#### <a name="to-save-the-model-project"></a>So speichern Sie das Modellprojekt  
  
-   Klicken Sie auf **Datei** > **Alle speichern**.  
  
## <a name="whats-next"></a>Wie geht es weiter?
[Lektion 3: Markieren als Datumstabelle](../tutorials/aas-lesson-3-mark-as-date-table.md).

  
  


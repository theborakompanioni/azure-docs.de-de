---
title: "Azure Analysis Services-Tutorial – Lektion 12: Analysieren in Excel | Microsoft-Dokumentation"
description: In diesem Tutorial wird das Analysieren in Excel im Azure Analysis Services-Tutorialprojekt beschrieben.
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
ms.openlocfilehash: 6f47de43ff8d94de22f8b7c12fa0707a8d7ffbbc
ms.contentlocale: de-de
ms.lasthandoff: 06/03/2017

---
# <a name="lesson-12-analyze-in-excel"></a>Lektion 12: Analysieren in Excel

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

In dieser Lektion verwenden Sie die Funktion „In Excel analysieren“, um Microsoft Excel zu öffnen, automatisch eine Verbindung mit dem Arbeitsbereichsmodell herzustellen und automatisch eine PivotTable zum Arbeitsblatt hinzuzufügen. Die Funktion „In Excel analysieren“ bietet eine schnelle und einfache Möglichkeit, vor der Bereitstellung ihres Modells die Wirksamkeit Ihres Modelldesigns zu testen. In dieser Lektion führen Sie keine Datenanalyse durch. Der Zweck dieser Lektion ist, Sie als Urheber des Modells mit den Tools vertraut zu machen ,die Sie zum Testen Ihres Modelldesigns verwenden können.   
  
Excel muss auf dem gleichen Computer wie SSDT installiert sein, um diese Lektion abschließen zu können.
  
Geschätzte Zeit zum Bearbeiten dieser Lektion: **5 Minuten**  
  
## <a name="prerequisites"></a>Voraussetzungen  
Dieses Thema ist Teil eines Tutorials zur Tabellenmodellierung, das in der vorgegebenen Reihenfolge durchgeführt werden sollte. Bevor Sie diese Lektion beginnen, sollten Sie die vorherige [Lektion 11: Erstellen von Rollen ](../tutorials/aas-lesson-11-create-roles.md) abgeschlossen haben.  
  
## <a name="browse-using-the-default-and-internet-sales-perspectives"></a>Durchsuchen mithilfe der Standard- und Internet Sales-Perspektive  
In den ersten Aufgaben durchsuchen Sie Ihr Modell mithilfe der Standardperspektive, die alle Modellobjekte enthält, und der Internet Sales-Perspektive, die Sie vorher erstellt haben. Die Internet Sales-Perspektive schließt das Customer-Tabellenobjekt aus.  
  
#### <a name="to-browse-by-using-the-default-perspective"></a>So durchsuchen Sie mit der Standardperspektive  
  
1.  Klicken Sie auf das Menü **Modell** und dann auf **In Excel analysieren**.  
  
2.  Klicken Sie im Dialogfeld **In Excel analysieren** auf **OK**.  
  
    Excel wird mit einer neuen Arbeitsmappe geöffnet. Mithilfe des aktuellen Benutzerkontos wird eine Datenquellenverbindung hergestellt. Die Standardperspektive wird verwendet, um anzeigbare Felder zu definieren. Dem Arbeitsblatt wird automatisch eine PivotTable hinzugefügt.  
  
3.  In Excel werden unter **PivotTable-Feldliste** die Measuregruppen **DimDate** und **FactInternetSales** angezeigt. Außerdem werden die folgenden Tabellen mit den entsprechenden Spalten angezeigt: **DimCustomer**, **DimDate**, **DimGeography**, **DimProduct**, **DimProductCategory**, **DimProductSubcategory** und **FactInternetSales**.  
  
4.  Schließen Sie Excel, ohne die Arbeitsmappe zu speichern.  
  
#### <a name="to-browse-by-using-the-internet-sales-perspective"></a>So durchsuchen Sie mit der Internet Sales-Perspektive  
  
1.  Klicken Sie auf das Menü **Modell**, und klicken Sie dann auf **In Excel analysieren**.  
  
2.  Lassen Sie im Dialogfeld **In Excel Analysieren** **Aktueller Windows-Benutzer** ausgewählt, und wählen Sie anschließend im Dropdownlistenfeld **Perspektive** **Internet Sales** aus, und klicken Sie auf **OK**. 
    
    ![aas-lektion12-perspektive](../tutorials/media/aas-lesson12-perspective.png)
    
3.  Beachten Sie, dass die DimCustomer-Tabelle in **PivotTable-Feldern** aus der Feldliste ausgeschlossen ist.  
    
    ![aas-lektion12-felder](../tutorials/media/aas-lesson12-fields.png)
    
4.  Schließen Sie Excel, ohne die Arbeitsmappe zu speichern.  
  
## <a name="browse-by-using-roles"></a>Durchsuchen mithilfe von Rollen  
Rollen sind ein wesentlicher Bestandteil tabellarischer Modelle. Ohne mindestens eine Rolle, zu der Benutzer als Mitglieder hinzugefügt werden, können Benutzer nicht mit Ihrem Modell auf Daten zugreifen und diese analysieren. Die Funktion „In Excel analysieren“ bietet eine Möglichkeit, die Rollen zu testen, die Sie definiert haben.  
  
#### <a name="to-browse-by-using-the-sales-manager-user-role"></a>So durchsuchen Sie mithilfe der Sales Manager-Benutzerrolle  
  
1.  Klicken Sie in SSDT auf das Menü **Modell**, und klicken Sie dann auf **In Excel analysieren**.  
  
2.  Wählen Sie unter **Geben Sie den Benutzernamen oder die Rolle für die Verbindung mit dem Modell an** die Option **Rolle** aus, wählen Sie anschließend im Dropdownlistenfeld **Sales Manager** aus, und klicken Sie auf **OK**.  
  
    Excel wird mit einer neuen Arbeitsmappe geöffnet. Eine PivotTable wird automatisch erstellt. Die PivotTable-Feldliste enthält alle Datenfelder, die im neuen Modell verfügbar sind.  
      
3.  Schließen Sie Excel, ohne die Arbeitsmappe zu speichern.  
  
## <a name="whats-next"></a>Wie geht es weiter?
Wechseln Sie zur nächsten Lektion: [Lektion 13: Bereitstellen](../tutorials/aas-lesson-13-deploy.md).

  
  
  


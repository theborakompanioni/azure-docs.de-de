---
title: 'Azure Analysis Services-Tutorial, Lektion 3: Markieren als Datumstabelle | Microsoft-Dokumentation'
description: In diesem Tutorial wird beschrieben, wie eine Tabelle im Azure Analysis Services-Tutorialprojekt als Datumstabelle markiert werden kann.
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
ms.openlocfilehash: 80c15b2115afca7342771d3f53d1742fc347ae1c
ms.contentlocale: de-de
ms.lasthandoff: 05/05/2017

---
# <a name="lesson-3-mark-as-date-table"></a>Lektion 3: Markieren als Datumstabelle

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

In Lektion 2: Abrufen von Daten, Import einer Dimensionstabelle namens DimDate. Die in Ihrem Modell mit DimDate benannte Tabelle kann auch als eine *Datumstabelle* bekannt sein, da sie Tages- und Uhrzeitdaten enthält.  
  
Datumstabelleneigenschaften müssen Sie immer dann angeben, wenn Sie bei Berechnungen DAX-Zeitintelligenzfunktionen verwenden, wie Sie es etwas später beim Erstellen von Measures tun werden. Zu diesen Eigenschaften zählen eine *Datumstabelle* und ein eindeutiger *Datumsspaltenbezeichner* in dieser Tabelle.
  
In dieser Lektion markieren Sie die DimDate-Tabelle als *Datumstabelle* und die Datumsspalte (in der Datumstabelle) als die *Datumsspalte* (eindeutiger Bezeichner).  

Doch bevor Sie die Datumstabelle und die Datumsspalte markieren, sollten Sie etwas aufräumen, um Ihr Modell verständlicher zu machen. Beachten Sie die Spalte **FullDateAlternateKey** in der Tabelle DimDate; sie enthält pro Tag und in der Tabelle enthaltenen Kalenderjahr eine Zeile. Diese Spalte werden Sie bei Measureformeln und in Berichten häufig benötigen. Allerdings ist FullDateAlternateKey kein guter Bezeichner für diese Spalte. Um Sie in Formeln einfacher finden und verwenden zu können, benennen Sie die Spalte in **Datum** um. Immer, wenn dies möglich ist, sollten Tabellen und Spalten so umbenannt werden, dass sie in SSDT und Client-Berichtsanwendungen wie Power BI und Excel einfacher gefunden werden können. 
  
Geschätzte Zeit zum Bearbeiten dieser Lektion: **3 Minuten**  
  
## <a name="prerequisites"></a>Voraussetzungen  
Dieses Thema ist Teil eines Tutorials zur Tabellenmodellierung, das in der vorgegebenen Reihenfolge durchgeführt werden sollte. Bevor Sie diese Lektion beginnen, sollten Sie die vorherige [Lektion 2: Abrufen von Daten](../tutorials/aas-lesson-2-get-data.md) abgeschlossen haben. 

### <a name="to-rename-the-fulldatealternatekey-column"></a>Umbenennen der Spalte FullDateAlternateKey

1.  Klicken Sie im Modell-Designer auf die Tabelle **DimDate**.

2.  Doppelklicken Sie auf den Header der Spalte **FullDateAlternateKey**, und benennen Sie sie in **Datum** um.

  
### <a name="to-set-mark-as-date-table"></a>Markieren als Datumstabelle  
  
1.  Wählen Sie die Spalte **Datum**, und klicken Sie dann im Fenster **Eigenschaften** unter **Datentyp**. Überprüfen Sie, ob **Datum** ausgewählt ist.  
  
2.  Klicken Sie auf das Menü **Tabelle**, klicken Sie dann auf **Datum**, und klicken Sie schließlich auf **Als Datumstabelle markieren**.  
  
3.  Wählen Sie im Dialogfeld **Als Datumstabelle markieren** im Listenfeld **Datum** die **Datumsspalte** als eindeutigen Bezeichner. In der Regel wird diese Spalte standardmäßig ausgewählt. Klicken Sie auf **OK**. 

    ![aas-lesson3-date-table](../tutorials/media/aas-lesson3-date-table.png)
  

## <a name="whats-next"></a>Wie geht es weiter?
[Lektion 4: Erstellen von Beziehungen](../tutorials/aas-lesson-4-create-relationships.md).
  


---
title: 'Azure Analysis Services-Tutorial, Lektion 1: Erstellen eines neuen tabellarischen Modellprojekts | Microsoft-Dokumentation'
description: Dieser Artikel beschreibt, wie ein neues Azure Analysis Services-Tutorialprojekt erstellt werden kann.
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
ms.translationtype: HT
ms.sourcegitcommit: 83f19cfdff37ce4bb03eae4d8d69ba3cbcdc42f3
ms.openlocfilehash: ebd160372fc75c6d0fc323be9e948fa2475b71cf
ms.contentlocale: de-de
ms.lasthandoff: 08/21/2017

---
# <a name="lesson-1-create-a-tabular-model-project"></a>Lektion 1: Erstellen eines tabellarischen Modellprojekts

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

In dieser Lektion verwenden Sie SQL Server Data Tools (SSDT), um ein neues tabellarisches Modellprojekt mit dem Kompatibilitätsgrad 1400 zu erstellen. Sobald das neue Projekt erstellt wurde, können Sie damit beginnen, Ihr Modell zu erstellen und Daten hinzuzufügen. In dieser Lektion wird eine kurze Einführung in die tabellarische Modellerstellungsumgebung in SSDT gegeben.  
  
Geschätzte Zeit zum Bearbeiten dieser Lektion: **10 Minuten**  
  
## <a name="prerequisites"></a>Voraussetzungen  
Dieses Thema ist die erste Lektion in einem Tutorial über tabellarische Modellerstellung. Es müssen einige Voraussetzungen erfüllt sein, um diese Lektion abschließen zu können. Weitere Informationen erhalten Sie in [Azure Analysis Services – Tutorial von Adventure Works](../tutorials/aas-adventure-works-tutorial.md).  
  
## <a name="create-a-new-tabular-model-project"></a>Erstellen eines neuen tabellarischen Modellprojekts  
  
#### <a name="to-create-a-new-tabular-model-project"></a>Erstellen eines neuen tabellarischen Modellprojekts  
  
1.  Klicken Sie in SSDT im **Menü** Datei auf **Neues** > **Projekt**.  
  
2.  Erweitern Sie im Dialogfeld **Neues Projekt** Dialogfeld **installierte** > **Business- Intelligence-** > **Analysedienste**, und klicken Sie dann auf **tabellarisches Analysis Services-Projekts**.  
  
3.  Geben Sie als **Namen** **AW-Internetverkäufe** ein, und geben Sie dann einen Speicherort für die Projektdateien an.  
  
    Standardmäßig entspricht der **Projektmappenname** dem Projektnamen; Sie können aber einen anderen Projektmappennamen eingeben.  
  
4.  Klicken Sie auf **OK**.  
  
5.  Im **Designer für tabellarische Modelle** wählen Sie im Dialogfeld **Integrierter Arbeitsbereich** aus.  
  
    Der Arbeitsbereich umfasst eine tabellarische Modelldatenbank, die denselben Namen trägt wie das Projekt während der Modellerstellung. Integrierter Arbeitsbereich bedeutet, dass SSDT eine integrierte Instanz verwendet und deswegen nur für die Modellerstellung kein separater Analysis Services-Server installiert werden muss.
      
6.  Wählen Sie in der Option **Kompatibilitätsgrad** **SQL Server 2017 / Azure Analysis Services (1400)**.   
 
    ![aas-lesson1-tmd](../tutorials/media/aas-lesson1-tmd.png)
      
    Wenn SQL Server 2017 / Azure Analysis Services (1400) nicht im Kompatibilitätsgrad-Listenfeld angezeigt wird, verwenden Sie nicht die neueste Version der SQL Server-Datentools. Um die neueste Version zu erhalten, siehe [Installieren der SQL Server-Datentools](https://docs.microsoft.com/sql/ssdt/download-sql-server-data-tools-ssdt).  
      
  
## <a name="understanding-the-ssdt-tabular-model-authoring-environment"></a>Grundlagen der tabellarischen Modellerstellungsumgebung von SSDT  
Nachdem Sie nun ein neues tabellarisches Modellprojekt erstellt haben, möchten wir uns kurz die tabellarische Modellerstellungsumgebung in SSDT ansehen.  
  
Nachdem das Projekt erstellt wurde, wird es in SSDT geöffnet. Auf der rechten Seite unter **Tabellarischer Modell-Explorer** sehen Sie eine Strukturansicht der Objekte in Ihrem Modell. Die Ordner sind leer, da Sie noch keine Daten importiert haben. Ähnlich wie bei der Menüleiste können Sie mit der rechten Maustaste auf einen Objektordner klicken. In den verschiedenen Schritten in diesem Tutorial verwenden Sie den tabellarischen Modell-Explorer, um durch die verschiedenen Objekte in Ihrem Modellprojekt zu navigieren.

![aas-lesson1-tme](../tutorials/media/aas-lesson1-tme.png)

Klicken Sie auf die Registerkarte **Projektmappen-Explorer**. Hier wird Ihre Datei **Model.bim** angezeigt. Wenn das Designer-Fenster nicht auf der linken Seite angezeigt wird (das leere Fenster mit der Registerkarte Model.bim), doppelklicken Sie im **Projektmappen-Explorer** unter **AW Internet-Verkaufsprojekt** auf die Datei **Model.bim**. Die Datei „Model.bim“ enthält alle Metadaten für Ihr Modellprojekt. 

![aas-lesson1-se](../tutorials/media/aas-lesson1-se.png)
  
Klicken Sie auf **Model.bim**. Im **Eigenschaftenfenster** werden die Modelleigenschaften angezeigt. Die wichtigste davon ist die Eigenschaft **DirectQuery-Modus**. Diese Eigenschaft gibt an, ob das Modell im In-Memory-Modus (Aus) oder im DirectQuery-Modus (Ein) bereitgestellt wird. Für dieses Tutorial werden Sie Ihr Modell im In-Memory-Modus erstellen und bereitstellen.

![aas-lesson1-properties](../tutorials/media/aas-lesson1-properties.png)
  
Wenn Sie ein neues Modell erstellen, werden bestimmte Modelleigenschaften gemäß den Datenmodellierungseinstellungen automatisch festgelegt. Diese Einstellungen können im Menü **Tools** im Dialogfeld **Optionen** festgelegt werden. Durch die Eigenschaften „Datensicherung“, „Arbeitsbereich beibehalten“ und „Arbeitsbereichsserver“ ist festgelegt, wie und wo die Datenbank des Arbeitsbereichs (Ihre Modellerstellungsdatenbank) gesichert, im Speicher gehalten und erstellt werden soll. Sie können diese Einstellungen später ändern, falls erforderlich, aber vorläufig lassen Sie diese Eigenschaften unverändert.  

Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf **AW-Internetverkäufe Sales** (Projekt), und klicken Sie dann auf **Eigenschaften**. Das Dialogfeld **Eigenschaftenseiten der AW-Internetverkäufe** wird angezeigt. Sie legen einige dieser Eigenschaften später fest, wenn Sie das Modell bereitstellen.  
  
Wenn Sie SSDT installiert haben, wurden der Visual Studio-Umgebung mehrere neue Menüelemente hinzugefügt. Klicken Sie auf das Menü **Modell**. Hier können Sie Daten importieren, die Daten des Arbeitsbereichs aktualisieren, das Modell in Excel analysieren, Perspektiven und Rollen erstellen, die Modellansicht auswählen und Berechnungsoptionen festlegen. Klicken Sie auf das Menü **Tabelle**. Hier können Sie Beziehungen erstellen und verwalten, Einstellungen für Datentabellen festlegen, Partitionen erstellen und die Tabelleneigenschaften bearbeiten. Wenn Sie auf das Menü **Spalte** klicken, können Sie Spalten in einer Tabelle hinzufügen oder diese löschen oder fixieren und die Sortierreihenfolge festlegen. SSDT fügt der Leiste auch einige Schaltflächen hinzu. Besonders nützlich ist die Funktion „AutoSumme“, mit der Sie für eine ausgewählte Spalte ein Standardaggregationsmaß erstellen können. Einige andere Schaltflächen der Symbolleiste bieten schnellen Zugriff auf häufig verwendete Funktionen und Befehle.  
  
Erkunden Sie einige der Dialoge und Speicherorte für verschiedene Funktionalitäten zur Erstellung von tabellarischen Modellen. Einige Elemente sind noch nicht aktiv, aber Sie können sich bereits eine gute Vorstellung von einer Erstellungsumgebung für tabellarische Modelle verschaffen.  
  

## <a name="whats-next"></a>Wie geht es weiter?
[Lektion 2: Abrufen von Daten](../tutorials/aas-lesson-2-get-data.md).

  
  
  


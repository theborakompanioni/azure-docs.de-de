---
title: "Cortana Intelligence-Katalog – benutzerdefinierte Module | Microsoft Docs"
description: Lernen Sie benutzerdefinierte Machine Learning-Module im Cortana Intelligence-Katalog kennen.
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 16037a84-dad0-4a8c-9874-a1d3bd551cf0
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/14/2017
ms.author: roopalik;garye
translationtype: Human Translation
ms.sourcegitcommit: e851a3e1b0598345dc8bfdd4341eb1dfb9f6fb5d
ms.openlocfilehash: 56c308643ad6d20170174725f76f6ddbc76b3e22
ms.lasthandoff: 04/15/2017


---
# <a name="discover-custom-machine-learning-modules-in-cortana-intelligence-gallery"></a>Lernen Sie benutzerdefinierte Machine Learning-Module im Cortana Intelligence-Katalog kennen
[!INCLUDE [machine-learning-gallery-item-selector](../../includes/machine-learning-gallery-item-selector.md)]

## <a name="custom-modules-for-machine-learning-studio"></a>Benutzerdefinierte Module für Machine Learning Studio
Im Cortana Intelligence-Katalog sind einige [benutzerdefinierte Module](https://gallery.cortanaintelligence.com/customModules) verfügbar, die die Funktionen von Azure Machine Learning Studio erweitern. Sie können diese Module zur Verwendung in Ihren Experimenten importieren, um noch fortschrittlichere Predictive Analytics-Lösungen entwickeln zu können.

Zurzeit bietet der Katalog Module zu *Zeitserienanalysen*, *Zuordnungsregeln*, *Clusteringalgorithmen* (über k-Means hinaus), *Visualisierungen* sowie andere leistungsstarke Hilfsprogramme.


## <a name="discover"></a>Entdecken
Wählen Sie zum Durchsuchen der benutzerdefinierten Module im [Katalog](http://gallery.cortanaintelligence.com) unter **Mehr** die Option **Benutzerdefinierte Module** aus.

![Auswählen von „Benutzerdefinierte Module“ auf der Startseite des Katalogs](media/machine-learning-gallery-custom-modules/select-custom-modules-in-gallery.png)

Auf der Seite **[Benutzerdefinierte Module](https://gallery.cortanaintelligence.com/customModules)** wird eine Liste der am häufigsten verwendeten benutzerdefinierten Module angezeigt. Um alle benutzerdefinierten Modulen anzuzeigen, wählen Sie die Schaltfläche **Alle anzeigen** aus. Wählen Sie zur Suche nach einem bestimmten Modul **Alle anzeigen** und dann die Filterkriterien aus. Sie können auch Suchbegriffe im Feld **Suchen** im oberen Bereich der Seite „Katalog“ eingeben.

![Klicken Sie auf „Alle anzeigen“, um alle benutzerdefinierten Module anzuzeigen](media/machine-learning-gallery-custom-modules/click-see-all-for-all-custom-modules.png)

### <a name="understand"></a>Informationen

Wählen Sie für Informationen über die Funktionsweise eines veröffentlichten benutzerdefinierten Moduls das benutzerdefinierte Modul aus, um die Seite „Moduldetails“ zu öffnen. Die Detailseite bietet eine konsistente und informative Lernerfahrung. Auf der Detailseite werden der Zweck des Moduls hervorgehoben und erwartete Eingaben, Ausgaben und Parameter aufgeführt. Die Detailseite verfügt außerdem über einen Link zum zugrunde liegenden Quellcode, den Sie untersuchen und anpassen können.

### <a name="comment-and-share"></a>Kommentieren und teilen
Auf der Detailseite eines benutzerdefinierten Moduls können Sie im Abschnitt **Kommentare** kommentieren, Feedback abgeben oder Fragen zum benutzerdefinierten Modul stellen. Sie können das Modul sogar für Ihre Kontakte oder Kollegen auf Twitter oder LinkedIn freigeben. Sie können auch einen Link zur Seite „Moduldetails“ per E-Mail senden, um andere Benutzer zum Besuch der Seite einzuladen.

![Teilen Sie dieses Element mit Freunden](media/machine-learning-gallery-how-to-use-contribute-publish/share-links.png)

![Fügen Sie Ihre eigenen Kommentare hinzu](media/machine-learning-gallery-how-to-use-contribute-publish/comments.png)

## <a name="import"></a>Import
Sie können alle benutzerdefinierten Module aus dem Katalog in Ihre eigenen Experimente importieren.

Der Cortana Intelligence-Katalog bietet die folgenden zwei Möglichkeiten, eine Kopie des Moduls zu importieren:

* **Aus dem Katalog**. Wenn Sie ein benutzerdefiniertes Modul aus dem Katalog importieren, erhalten Sie auch ein Beispielexperiment für die Verwendung des Moduls.
* **Aus Machine Learning Studio**. Sie können jedes benutzerdefinierte Modul importieren, während Sie Machine Learning Studio verwenden (in diesem Fall erhalten Sie das Beispielexperiment nicht).

### <a name="from-the-gallery"></a>Aus dem Katalog

1. Öffnen Sie die Moduldetailseite im Katalog. 
2. Wählen Sie **In Studio öffnen** aus.
   
    ![Öffnen eines benutzerdefinierten Moduls aus dem Katalog](media/machine-learning-gallery-custom-modules/open-custom-module-from-gallery.png)
   
Jedes benutzerdefinierte Modul enthält ein Beispielexperiment, das die Verwendung des Moduls veranschaulicht. Durch die Auswahl von **In Studio öffnen** wird das Beispielexperiment in Ihrem Machine Learning Studio-Arbeitsbereich geöffnet. (Wenn Sie nicht bereits bei Studio angemeldet sind, werden Sie zur erstmaligen Anmeldung mit Ihrem Microsoft-Konto aufgefordert.)

Zusätzlich zum Beispielexperiment wird das benutzerdefinierte Modul in Ihren Arbeitsbereich kopiert. Es wird außerdem mit allen integrierten oder benutzerdefinierten Machine Learning Studio-Modulen in Ihre Modulpalette eingefügt. Sie können es jetzt wie andere Module in Ihrem Arbeitsbereich in Ihren eigenen Experimenten verwenden.

### <a name="from-within-machine-learning-studio"></a>Aus Machine Learning Studio

1. Wählen Sie in Machine Learning Studio **NEU** aus.
2. Wählen Sie **Modul** aus. Sie können in einer Liste von Katalogmodulen auswählen oder mithilfe des Felds **Suchen** nach einem bestimmten Modul suchen.
3. Zeigen Sie mit dem Mauszeiger auf ein Modul, und wählen Sie dann **Modul importieren** aus. (Wählen Sie zum Anzeigen von Informationen über das Modul **Im Katalog anzeigen** aus. Dadurch wird die Moduldetailseite im Katalog geöffnet.)
   
    ![Import von benutzerdefinierten Modulen in Machine Learning Studio](media/machine-learning-gallery-custom-modules/add-custom-module-in-studio.png)

Das benutzerdefinierte Modul wird in Ihren Arbeitsbereich kopiert und zusammen mit den anderen integrierten oder benutzerdefinierten Machine Learning Studio-Modulen in der Modulpalette platziert. Sie können es jetzt wie andere Module in Ihrem Arbeitsbereich in Ihren eigenen Experimenten verwenden.

## <a name="use"></a>Verwenden Sie

Unabhängig davon, welche Methode Sie zum Importieren eines benutzerdefinierten Moduls auswählen, wird das Modul beim Importieren in der Modulpalette in Machine Learning Studio platziert. Sie können das benutzerdefinierte Modul aus der Modulpalette in allen Experimenten im Arbeitsbereich genau wie alle anderen Module verwenden.

So verwenden Sie ein importiertes Modul

1. Erstellen Sie ein neues Experiment, oder öffnen Sie ein vorhandenes.
2. Wählen Sie zum Erweitern der Liste der benutzerdefinierten Modulen in Ihrem Arbeitsbereich in der Modulpalette **Benutzerdefiniert** aus. Die Modulpalette befindet sich links neben der Experimentcanvas.
   
    ![Benutzerdefinierte Modulliste in Studio Palette](media/machine-learning-gallery-custom-modules/custom-module-in-studio-palette.png)
3. Wählen Sie das Modul aus, das Sie importiert haben, und ziehen Sie es zum Experiment.


**[Wechseln zum Katalog](http://gallery.cortanaintelligence.com)**

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]



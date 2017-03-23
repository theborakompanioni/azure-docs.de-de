---
title: Erweitern Ihres Experiments mit R | Microsoft Docs
description: "In diesem Artikel erfahren Sie, wie Sie die Funktionalität von Azure Machine Learning Studio über die R-Sprache mithilfe des Moduls &quot;Execute R Script&quot; erweitern."
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: 2c038a45-ba4d-42ea-9a88-e67391ef8c0a
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/20/2017
ms.author: garye
translationtype: Human Translation
ms.sourcegitcommit: 9e738c4e5f43ae6c939f7c6da90c258498943e73
ms.openlocfilehash: e9ff351232f68c81122efb74275a1f255b51b72f
ms.lasthandoff: 12/14/2016


---
# <a name="extend-your-experiment-with-r"></a>Erweitern Ihres Experiments mit R
Sie können die Funktionalität von Azure Machine Learning Studio über die R-Sprache mithilfe des Moduls [Execute R Script][execute-r-script] erweitern.

Dieses Modul akzeptiert mehrere Eingabedatasets und gibt als Ergebnis ein einzelnes Dataset aus. Sie können für den Parameter **R Script** des Moduls [Execute R Script][execute-r-script] ein R-Skript eingeben.

Sie können auf jeden Eingangsport des Moduls mithilfe von Code wie folgt zugreifen:

    dataset1 <- maml.mapInputPort(1)

## <a name="listing-all-currently-installed-packages"></a>Auflisten aller derzeit installierten Pakete
Die Liste der installierten Pakete kann sich ändern. Eine Liste der derzeit installierten Pakete finden Sie unter [R Packages Supported by Azure Machine Learning](https://msdn.microsoft.com/library/azure/mt741980.aspx) (Von Azure Machine Learning unterstützte R-Pakete).

Sie können auch die vollständige und aktuelle Liste der installierten Pakete abrufen, indem Sie folgenden Code in das Modul [Execute R Script][execute-r-script] eingeben:

    out <- data.frame(installed.packages(,,,fields="Description"))
    maml.mapOutputPort("out")

Damit wird die Liste der Pakete an den Ausgabeport des Moduls [Execute R Script][execute-r-script] gesendet.
Um die Liste der Pakete anzuzeigen, verbinden Sie ein Modul für die Konvertierung wie [Convert to CSV][convert-to-csv] mit der linken Ausgabe des Moduls [Execute R Script][execute-r-script]. Führen Sie das Experiment aus, klicken Sie dann auf die Ausgabe des Konvertierungsmoduls, und wählen Sie **Download** aus. 

![Herunterladen der Ausgabe des Moduls „Convert to CSV“](./media/machine-learning-extend-your-experiment-with-r/download-package-list.png)


<!--
For convenience, here is the [current full list with version numbers in Excel format](http://az754797.vo.msecnd.net/docs/RPackages.xlsx).
-->

## <a name="importing-packages"></a>Importieren von Paketen
Sie können noch nicht installierte Pakete mithilfe der folgenden Befehle im Modul [Execute R Script][execute-r-script] importieren:

    install.packages("src/my_favorite_package.zip", lib = ".", repos = NULL, verbose = TRUE)
    success <- library("my_favorite_package", lib.loc = ".", logical.return = TRUE, verbose = TRUE)

Dabei enthält die Datei `my_favorite_package.zip` Ihr Paket.

[!INCLUDE [machine-learning-free-trial](../../includes/machine-learning-free-trial.md)]


<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[convert-to-csv]: https://msdn.microsoft.com/library/azure/faa6ba63-383c-4086-ba58-7abf26b85814/


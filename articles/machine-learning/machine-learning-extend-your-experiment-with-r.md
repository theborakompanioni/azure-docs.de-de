<properties
	pageTitle="Erweitern Ihres Experiments mit R | Microsoft Azure"
	description="In diesem Artikel erfahren Sie, wie Sie die Funktionalität von Azure Machine Learning Studio über die R-Sprache mithilfe des Moduls ";Execute R Script"; erweitern."
	services="machine-learning"
	documentationCenter=""
	authors="garyericson"
	manager="jhubbard"
	editor="cgronlun"/>

<tags
	ms.service="machine-learning"
	ms.workload="data-services"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="08/19/2016"
	ms.author="garye" />


# Erweitern Ihres Experiments mit R

Sie können die Funktionalität von ML Studio mithilfe der Programmiersprache R hinaus erweitern, indem Sie das [Execute R Script][execute-r-script] verwenden.

Dieses Modul akzeptiert mehrere Eingabedatasets, als Ergebnis wird ein einzelnes Dataset ausgegeben. Sie können ein R-Skript für den Parameter **R Script** des Moduls [Execute R Script][execute-r-script] eingeben.

Sie können auf jeden Eingangsport des Moduls mithilfe von Code wie folgt zugreifen:

    dataset1 <- maml.mapInputPort(1)

[AZURE.INCLUDE [machine-learning-kostenlose-Testversion](../../includes/machine-learning-free-trial.md)]

## Auflisten aller derzeit installierten Pakete

Die Liste der installierten Pakete kann sich ändern. Um eine vollständige und aktuelle Liste der installierten Pakete abzurufen, einschließlich der Beschreibung jedes Pakets, geben Sie folgenden Code in das Modul [Execute R Script][execute-r-script] ein:

    out <- data.frame(installed.packages(,,,fields="Description"))
	maml.mapOutputPort("out")

Damit wird die Liste der Pakete an den Ausgabeport des Moduls [Execute R Script][execute-r-script] gesendet. Um die Liste der Pakete anzuzeigen, verbinden Sie ein Modul für die Konvertierung wie [Convert to CSV][convert-to-csv] mit der linken Ausgabe des [Moduls Execute R][execute-r-script] Script. Führen Sie das Experiment aus, klicken Sie dann auf die Ausgabe des Konvertierungsmoduls, und wählen Sie **Download** aus.

![](./media/machine-learning-extend-your-experiment-with-r/download-package-list.png)

<!--
For convenience, here is the [current full list with version numbers in Excel format](http://az754797.vo.msecnd.net/docs/RPackages.xlsx).
-->

## Importieren von Paketen

Sie können auch Pakete importieren, die noch nicht aus einem bereitgestellten ML Studio Repository installiert wurden. Hierzu verwenden Sie die folgenden Befehle im Modul [Execute R Script][execute-r-script] und einem gezippten Paketarchiv:

    install.packages("src/my_favorite_package.zip", lib = ".", repos = NULL, verbose = TRUE)
    success <- library("my_favorite_package", lib.loc = ".", logical.return = TRUE, verbose = TRUE)

Hierbei enthält `my_favorite_package.zip` die ZIP-Datei des Pakets.

<!-- Module References -->
[execute-r-script]: https://msdn.microsoft.com/library/azure/30806023-392b-42e0-94d6-6b775a6e0fd5/
[convert-to-csv]: https://msdn.microsoft.com/library/azure/faa6ba63-383c-4086-ba58-7abf26b85814/

<!---HONumber=AcomDC_0914_2016-->
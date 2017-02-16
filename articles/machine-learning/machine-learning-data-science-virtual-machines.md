---
title: Virtuelle Data Science-Computer in Azure | Microsoft Docs
description: "Einrichten eines virtuellen Computers für Data Science"
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 95e1fa87-794a-4d03-80a4-af4f3f3ac31e
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/19/2016
ms.author: xibingao;bradsev
translationtype: Human Translation
ms.sourcegitcommit: a6bc79b2cb5b73109cddd6cf57caeba754b52e2e
ms.openlocfilehash: 4de0683f2f58cc598a5c37e74a30b7e17aa7fd0d


---
# <a name="data-science-virtual-machines-in-azure"></a>Virtuelle Data Science-Computer in Azure
Sie erhalten hier Anweisungen zum Einrichten einer Azure-VM und einer Azure-VM mit SQL Server als IPython Notebook-Server. Der virtuelle Windows-Computer wird mit Unterstützung von Tools wie IPython Notebook, Azure-Speicher-Explorer und AzCopy sowie anderer Hilfsprogramme, die für Data Science-Projekte hilfreich sind, konfiguriert. Azure-Speicher-Explorer und AzCopy stellen z. B. hilfreiche Möglichkeiten zum Hochladen von Daten von Ihrem lokalen Computer in den Azure-Speicher oder das Herunterladen aus dem Speicher auf Ihren lokalen Computer bereit. 

Dieses Menü enthält Links zu Themen, in denen das Einrichten der verschiedenen Data Science-Umgebungen, die vom [Team Data Science-Prozess (TDSP)](data-science-process-overview.md)verwendet werden, beschrieben wird.

[!INCLUDE [data-science-environment-setup](../../includes/cap-setup-environments.md)]

Es können mehrere Arten von virtuellen Azure-Computern bereitgestellt und als Teil einer cloudbasierten Data Science-Umgebung konfiguriert werden. Die Entscheidung, welche Art von virtuellem Computer Sie verwenden, hängt von der Art und der Menge der Daten ab, die mit Machine Learning modelliert werden sollen, sowie vom Ziel für die Daten in der Cloud. 

* Hilfe bei dieser Entscheidung finden Sie unter [Planen der Azure Machine Learning Data Science-Umgebung](machine-learning-data-science-plan-your-environment.md). 
* Einen Katalog mit einigen Szenarien für die erweiterte Analyse finden Sie unter [Szenarien für Advanced Analytics Process and Technology (ADAPT) in Azure Machine Learning](machine-learning-data-science-plan-sample-scenarios.md)

Es werden zwei Verfahren beschrieben:

* [Einrichten eines virtuellen Azure-Computers als IPython Notebook-Server für die erweiterte Analyse](machine-learning-data-science-setup-virtual-machine.md) wird beschrieben, wie Sie mit IPython Notebook und anderen Tools einen virtuellen Azure-Computer bereitstellen, der für Data Science-Fälle eingesetzt wird, bei denen die Daten in einem anderen Azure-Speicher als SQL gespeichert werden.
* In [Einrichten einer Azure SQL Server-VM als IPython Notebook-Server für die erweiterte Analyse](machine-learning-data-science-setup-sql-server-virtual-machine.md) wird beschrieben, wie Sie mit IPython Notebook und anderen Tools eine Azure SQL Server-VM bereitstellen, die für Data Science-Fälle eingesetzt wird, bei denen die Daten in einer SQL-Datenbank gespeichert werden.

Nach der Bereitstellung und Konfiguration können diese virtuellen Computer als IPython Notebook-Server für das Untersuchen und Verarbeiten von Daten sowie für andere Aufgaben in Verbindung mit Azure Machine Learning und dem Team Data Science-Prozess (TDSP) verwendet werden. Die nächsten Schritte im Data Science-Prozess sind im [TDSP-Lernpfad](https://azure.microsoft.com/documentation/learning-paths/cortana-analytics-process/) aufgeführt. Dazu gehören auch das Verschieben von Daten nach SQL Server oder HDInsight sowie das Verarbeiten und Extrahieren von Stichproben für die Gewinnung von Informationen aus den Daten mithilfe von Azure Machine Learning.

> [!NOTE]
> Virtuelle Azure-Computer werden **nach Nutzung abgerechnet**. Damit Sie nicht für ungenutzte virtuelle Computer bezahlen müssen, müssen Sie diese im **klassischen Azure-Portal** in den Status [Angehalten (Zuordnung aufgehoben)](http://manage.windowsazure.com/)versetzen. Eine Schrittanleitung dazu, wie Sie die Zuordnung Ihrer virtuellen Computer aufheben, finden Sie unter [Herunterfahren und Freigeben von nicht genutzten virtuellen Computern](machine-learning-data-science-setup-virtual-machine.md#shutdown)
> 
> 




<!--HONumber=Dec16_HO3-->



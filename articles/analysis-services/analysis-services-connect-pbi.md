---
title: Verbinden von Azure Analysis Services mit Power BI | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie mithilfe von Power BI eine Verbindung mit einem Azure Analysis Services-Server herstellen.
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
ms.date: 04/12/2017
ms.author: owend
translationtype: Human Translation
ms.sourcegitcommit: 0d6f6fb24f1f01d703104f925dcd03ee1ff46062
ms.openlocfilehash: b17251f8e88dc02ddf792da41121fe2730bc50e8
ms.lasthandoff: 04/17/2017


---
# <a name="connect-with-power-bi"></a>Herstellen einer Verbindung mit Power BI

Sobald Sie in Azure einen Server erstellt, und ein tabellarisches Modell bereitgestellt haben, können Benutzer in Ihrer Organisation beginnen, eine Verbindung herzustellen und Daten zu durchsuchen. 

> [!TIP]
> Achten Sie darauf, dass Sie die neueste Version von [Power BI Desktop](https://powerbi.microsoft.com/desktop/) verwenden.
> 
> 
  
## <a name="connect-in-power-bi-desktop"></a>Herstellen einer Verbindung in Power BI Desktop

1. Klicken Sie in Power BI Desktop auf **Daten abrufen** > **Azure** > **Azure Analysis Services-Datenbank**.

2. Geben Sie unter **Server** den Namen des Servers ein. 
    
    Achten Sie darauf, die vollständige URL anzugeben. Beispiel: asazure://westcentralus.asazure.windows.net/advworks.

3. Wenn Sie den Namen der Datenbank für tabellarische Modelle oder die Perspektive kennen, mit der Sie sich verbinden wollen, fügen Sie ihn unter **Datenbanken** ein. Andernfalls können Sie dieses Feld leer lassen und später eine Datenbank oder Perspektive auswählen.

4. Lassen Sie den Standardwert **Live verbinden** ausgewählt, und klicken Sie dann auf **Verbinden**. 

5. Geben Sie bei der entsprechenden Aufforderung Ihre Anmeldeinformationen ein. 

6. Erweitern Sie unter **Navigator** den Server, wählen Sie das Modell oder die Perspektive aus, mit denen Sie eine Verbindung herstellen möchten, und klicken Sie dann auf **Verbinden**. Klicken Sie auf ein Modell oder eine Perspektive, um alle Objekte für diese Ansicht anzuzeigen.

    Das Modell wird in Power BI Desktop mit einem leeren Bericht in der Berichtsansicht geöffnet. In der Felderliste werden alle nicht ausgeblendeten Modellobjekte angezeigt. Der Verbindungsstatus wird in der unteren rechten Ecke angezeigt.

## <a name="connect-in-power-bi-service"></a>Herstellen einer Verbindung in Power BI (Dienst)

1. Erstellen Sie eine Power BI Desktop-Datei, die über eine Liveverbindung zu Ihrem Modell auf dem Server verfügt.
2. Klicken Sie unter [Power BI](https://powerbi.microsoft.com) auf **Daten abrufen** > **Dateien**. Suchen und markieren Sie Ihre Datei.



## <a name="see-also"></a>Weitere Informationen
[Herstellen einer Verbindung mit Azure Analysis Services](analysis-services-connect.md)   
[Clientbibliotheken](analysis-services-data-providers.md)



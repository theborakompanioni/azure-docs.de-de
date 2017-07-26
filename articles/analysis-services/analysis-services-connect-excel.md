---
title: Verbinden von Azure Analysis Services mit Excel | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie mithilfe von Excel eine Verbindung mit einem Azure Analysis Services-Server herstellen.
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
ms.date: 06/01/2017
ms.author: owend
ms.translationtype: Human Translation
ms.sourcegitcommit: 43aab8d52e854636f7ea2ff3aae50d7827735cc7
ms.openlocfilehash: ff96602642c56a3cd02aeada056c059573707731
ms.contentlocale: de-de
ms.lasthandoff: 06/03/2017


---
# <a name="connect-with-excel"></a>Herstellen einer Verbindung mit Excel

Sobald Sie in Azure einen Server erstellt und ein tabellarisches Modell bereitgestellt haben, können Sie damit beginnen, eine Verbindung herzustellen und Daten zu durchsuchen.


## <a name="connect-in-excel"></a>Eine Verbindung in Excel herstellen

Das Herstellen einer Verbindung mit einem Server in Excel wird mithilfe der Funktion „Daten abrufen“ in Excel 2016 unterstützt. Das Herstellen einer Verbindung mithilfe des Import Table Wizard (Assistent „Tabelle importieren“) in Power Pivot wird nicht unterstützt. 

**So stellen Sie eine Verbindung in Excel 2016 her**

1. Klicken Sie in Excel 2016 im Menüband **Daten** auf **Get External Data (Externe Daten abrufen)** > **Aus anderen Quellen** > **Aus Analysis Services**.

2. Geben Sie im Datenverbindungs-Assistenten in **Servername** den Servernamen samt Protokoll und URI ein. Wählen Sie dann unter **Anmeldeinformationen** die Option **Benutzername und Kennwort verwenden** aus, und geben Sie den Benutzernamen der Organisation, z.B. nancy@adventureworks.com, und das Kennwort ein.

    ![Herstellen einer Verbindung über die Excel-Anmeldung](./media/analysis-services-connect-excel/aas-connect-excel-logon.png)

3. Wählen Sie unter **Datenbank und Tabelle wählen** die Datenbank und das Modell oder die Perspektive aus, und klicken Sie dann auf **Fertig stellen**.
   
    ![Herstellen einer Verbindung über die Excel-Modellauswahl](./media/analysis-services-connect-excel/aas-connect-excel-select.png)


## <a name="see-also"></a>Weitere Informationen
[Clientbibliotheken](analysis-services-data-providers.md)   
[Manage your server (Verwalten des Servers)](analysis-services-manage.md)     




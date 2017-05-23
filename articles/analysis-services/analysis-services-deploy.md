---
title: Bereitstellen in Azure Analysis Services mithilfe von SSDT | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie ein tabellarisches Modell mithilfe von SSDT auf einem Azure Analysis Services-Server bereitstellen.
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 5f1f0ae7-11de-4923-a3da-888b13a3638c
ms.service: analysis-services
ms.devlang: NA
ms.topic: get-started-article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 04/17/2017
ms.author: owend
ms.translationtype: Human Translation
ms.sourcegitcommit: 95b8c100246815f72570d898b4a5555e6196a1a0
ms.openlocfilehash: 04b809a6fdd7b993a048a67f66a9851c2a7bf3d2
ms.contentlocale: de-de
ms.lasthandoff: 05/18/2017


---
# <a name="deploy-a-model-from-ssdt"></a>Bereitstellen eines Modells mithilfe von SSDT
Nachdem Sie im Azure-Abonnement einen Server erstellt haben, können Sie eine tabellarische Modelldatenbank auf ihm bereitstellen. Sie können SQL Server Data Tools (SSDT) zum Erstellen und Bereitstellen eines tabellarischen Modellprojekts verwenden. 

## <a name="before-you-begin"></a>Voraussetzungen
Zunächst benötigen Sie Folgendes:

* **Analysis-Services-Server** in Azure. Weitere Informationen finden Sie unter [Create an Analysis Services Server in Azure](analysis-services-create-server.md) (Erstellen eines Analysis-Services-Servers in Azure, in englischer Sprache).
* **Tabellarisches Modellprojekt** in SSDT oder ein vorhandenes tabellarisches Modell mit dem Kompatibilitätsgrad 1200 oder höhere in einer Analysis Services-Instanz. Haben Sie noch nie eines erstellt? Absolvieren Sie das [Adventure Works-Tutorial](https://msdn.microsoft.com/library/hh231691.aspx).
* **Lokales Gateway** – Wenn eine oder mehrere Datenquellen lokal im Netzwerk der Organisation vorhanden sind, müssen Sie ein [lokales Datengateway](analysis-services-gateway.md) installieren. Das Gateway ist erforderlich, damit der Server in der Cloud eine Verbindung mit den lokalen Datenquellen herstellen kann, um Daten im Modell zu verarbeiten und zu aktualisieren.

> [!TIP]
> Stellen Sie vor dem Bereitstellen sicher, dass Sie die Daten in den Tabellen verarbeiten können. Klicken Sie in SSDT auf **Modell** > **Verarbeiten** > **Alle verarbeiten**. Wenn die Verarbeitung fehlschlägt, schlägt auch die Bereitstellung fehl.
> 
> 

## <a name="to-deploy-a-tabular-model-from-ssdt"></a>So stellen Sie ein tabellarisches Modell aus SSDT bereit

1. Vor dem Bereitstellen müssen Sie den Servernamen abrufen. Kopieren Sie den Servernamen über **Azure-Portal** > Server > **Übersicht** > **Servername**.
   
    ![Abrufen von Servernamen in Azure](./media/analysis-services-deploy/aas-deploy-get-server-name.png)
2. Klicken Sie in SSDT im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt, und wählen Sie **Eigenschaften** aus. Fügen Sie dann in **Bereitstellung** > **Server** den Servernamen ein.   
   
    ![Einfügen des Servernamens in die Bereitstellungsserver-Eigenschaft](./media/analysis-services-deploy/aas-deploy-deployment-server-property.png)
3. Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf **Eigenschaften**, und klicken Sie dann auf **Bereitstellen**. Sie werden möglicherweise aufgefordert, sich bei Azure anzumelden.
   
    ![Auf Server bereitstellen](./media/analysis-services-deploy/aas-deploy-deploy.png)
   
    Der Bereitstellungsstatus wird im Ausgabefenster und in „Bereitstellen“ angezeigt.
   
    ![Bereitstellungsstatus](./media/analysis-services-deploy/aas-deploy-status.png)

Das war es schon!


## <a name="but-something-went-wrong"></a>Etwas hat jedoch nicht funktioniert
Wenn das Bereitstellen von Metadaten fehlschlägt, konnte wahrscheinlich keine Verbindung von SSDT mit dem Server hergestellt werden. Stellen Sie sicher, dass mithilfe von SSMS eine Verbindung mit dem Server hergestellt werden kann. Stellen Sie dann sicher, dass die Bereitstellungsserver-Eigenschaft für das Projekt korrekt ist.

Wenn die Bereitstellung für eine Tabelle fehlschlägt, konnte wahrscheinlich der Server keine Verbindung mit einer Datenquelle herstellen. Wenn die Datenquelle lokal im Netzwerk der Organisation vorhanden ist, müssen Sie ein [lokales Datengateway](analysis-services-gateway.md) installieren.

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie jetzt das tabellarische Modell auf dem Server bereitgestellt haben, können Sie eine Verbindung mit diesem herstellen. Sie können [per SSMS eine Verbindung mit diesem herstellen](analysis-services-manage.md), um es zu verwalten. Und Sie können [mithilfe eines Clienttools eine Verbindung mit diesem herstellen](analysis-services-connect.md) (z. B. mit Power BI, Power BI Desktop oder Excel) und mit dem Erstellen von Berichten beginnen.



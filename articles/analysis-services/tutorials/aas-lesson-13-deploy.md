---
title: "Azure Analysis Services-Tutorial – Lektion 13: Bereitstellen | Microsoft-Dokumentation"
description: Dieser Artikel beschreibt, wie das Tutorialprojekt an Azure Analysis Services bereitgestellt werden kann.
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
ms.date: 07/17/2017
ms.author: owend
ms.translationtype: HT
ms.sourcegitcommit: 83f19cfdff37ce4bb03eae4d8d69ba3cbcdc42f3
ms.openlocfilehash: 70dbf5786262f75199270aa8009e03b9b48b8559
ms.contentlocale: de-de
ms.lasthandoff: 08/21/2017

---
# <a name="lesson-13-deploy"></a>Lektion 13: Bereitstellen

[!INCLUDE[analysis-services-appliesto-aas-sql2017-later](../../../includes/analysis-services-appliesto-aas-sql2017-later.md)]

In dieser Lektion konfigurieren Sie Bereitstellungseigenschaften, indem Sie einen Azure Analysis Services-Server für die Bereitstellung und einen Namen für das Modell angeben. Dann stellen Sie das Modell für diese Instanz bereit. Nachdem Sie Ihr Modell bereitgestellt haben, können Benutzer mithilfe einer Berichterstellungsclientanwendung eine Verbindung herstellen. Weitere Informationen finden Sie unter [Deploy to Azure Analysis Services](https://docs.microsoft.com/azure/analysis-services/analysis-services-deploy) (Bereitstellen in Azure Analysis Services, in englischer Sprache).  
  
Geschätzte Zeit zum Bearbeiten dieser Lektion: **5 Minuten**  
  
## <a name="prerequisites"></a>Voraussetzungen  
Dieses Thema ist Teil eines Tutorials zur Tabellenmodellierung, das in der richtigen Reihenfolge absolviert werden sollte. Bevor Sie diese Lektion beginnen, sollten Sie die vorherige [Lektion 12: Analysieren in Excel](../tutorials/aas-lesson-12-analyze-in-excel.md) abgeschlossen haben.  

> [!IMPORTANT]  
> Sie müssen über [Administratorberechtigungen](../analysis-services-server-admins.md) auf dem Analysis Services-Remoteserver verfügen, um dort Modelle bereitstellen zu können.  

> [!IMPORTANT]  
> Wenn Sie die Beispieldatenbank „AdventureWorksDW2014“ auf einer lokalen SQL Server-Instanz installieren und Sie Ihr Modell auf einem Server mit Azure Analysis Services bereitstellen, benötigen Sie ein [lokales Datengateway](../analysis-services-gateway.md).
  
## <a name="deploy-the-model"></a>Bereitstellen des Modells  
  
#### <a name="to-configure-deployment-properties"></a>So Konfigurieren Sie die Bereitstellungseigenschaften  

  
1.  Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt **AW Internet Sales**, und klicken Sie anschließend auf **Eigenschaften**.  
  
2.  Geben Sie im Dialogfeld **Eigenschaftenseiten der AW-Internetverkäufe** unter **Bereitstellungsserver** in der **Server**-Eigenschaft den vollständigen Server ein.  

    ![aas-lektion13-bereitstellen-eigenschaft](../tutorials/media/aas-lesson13-deploy-property.png)
  
3.  Geben Sie in der Eigenschaft **Database** **Adventure Works Internet Sales** ein.  
  
4.  Geben Sie in der Eigenschaft **Model Name** **Adventure Works Internet Sales Model** ein.  
  
5.  Überprüfen Sie die Auswahl, und klicken Sie dann auf **OK**.  
  
#### <a name="to-deploy-the-adventure-works-internet-sales"></a>So stellen Sie „Adventure Works Internet Sales“ bereit
  
1.  Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt **AW Internet Sales** und dann auf **Erstellen**.  

2.  Klicken Sie mit der rechten Maustaste auf der Projekt **AW Internet Sales** und dann auf **Bereitstellen**.

    Wenn Sie eine Bereitstellung an Azure Analysis Services durchführen, werden Sie möglicherweise aufgefordert, Ihre Kontodaten einzugeben. Geben Sie Ihr Organisationskonto und das Kennwort ein, z.B. nancy@adventureworks.com. Dieses Konto muss ein Administratorkonto des Servers sein.
  
    Das Dialogfeld „Bereitstellen“ erscheint und zeigt den Bereitstellungsstatus der Metadaten und jeder Tabelle an, die sich im Modell befinden.  
    
    ![aas-lektion13-bereitstellen-status](../tutorials/media/aas-lesson13-deploy-status.png)
  
3. Wenn die Bereitstellung erfolgreich abgeschlossen ist, können Sie auf **Schließen** klicken.  
  
## <a name="conclusion"></a>Zusammenfassung  
Glückwunsch! Nun ist das Schreiben und Bereitstellen Ihres ersten tabellarischen Modells mit Analysis Services abgeschlossen. Dieses Tutorial hat Sie beim Durchführen der am häufigsten vorkommenden Aufgaben beim Erstellen eines tabellarischen Modells begleitet. Nachdem Ihr Modell „Adventure Works Internet Sales“ nun bereitgestellt wurde, können Sie SSMS zur Verwaltung des Modells verwenden. Außerdem können Sie Nachverarbeitungsskripts und einen Sicherungsplan erstellen. Außerdem können Benutzer jetzt eine Verbindung mit dem Modell mithilfe einer Berichterstellungsclientanwendung, wie z.B. Microsoft Excel oder Power BI, herstellen.  

![aas-lektion13-ssms](../tutorials/media/aas-lesson13-ssms.png)
  
  
  
## <a name="whats-next"></a>Wie geht es weiter?
[Herstellen einer Verbindung mit Power BI](../analysis-services-connect-pbi.md)   
[Ergänzende Lektion – Dynamische Sicherheit](../tutorials/aas-supplemental-lesson-dynamic-security.md)   
[Ergänzende Lektion – Detailzeilen](../tutorials/aas-supplemental-lesson-detail-rows.md)   
[Ergänzende Lektion – Unregelmäßige Hierarchien](../tutorials/aas-supplemental-lesson-ragged-hierarchies.md)   


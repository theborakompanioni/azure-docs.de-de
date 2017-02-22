---
title: Erstellen eines Analysis Services-Servers in Azure | Microsoft Docs
description: Informationen zum Erstellen einer Analysis Services-Serverinstanz in Azure.
services: analysis-services
documentationcenter: 
author: minewiskan
manager: erikre
editor: 
tags: 
ms.assetid: 7f560216-8a9a-4d06-852e-48cf24deab19
ms.service: analysis-services
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 01/20/2017
ms.author: owend
translationtype: Human Translation
ms.sourcegitcommit: 147c27d12b87379ff02b871ceabd8154e53b6c1a
ms.openlocfilehash: f189e19b5b843d6a7d056c8230bc06d85e568202


---
# <a name="create-an-analysis-services-server"></a>Erstellen eines Analysis Services-Servers
In diesem Artikel werden die Schritte zum Erstellen einer neuen Analysis Services-Serverressource in Ihrem Azure-Abonnement beschrieben.

## <a name="before-you-begin"></a>Voraussetzungen
Zunächst benötigen Sie Folgendes:

* **Azure-Abonnement:** Besuchen Sie die Webseite [Kostenlose Azure-Testversion](https://azure.microsoft.com/offers/ms-azr-0044p/), und erstellen Sie ein Konto.
* **Azure Active Directory**: Ihr Abonnement muss einem Azure Active Directory-Mandanten zugeordnet sein. Und Sie müssen bei Azure mit einem Konto in diesem Azure Active Directory angemeldet sein. Microsoft-Konten werden nicht unterstützt. Weitere Informationen finden Sie unter [Benutzerauthentifizierung](analysis-services-overview.md#secure).
* **Ressourcengruppe:** Verwenden Sie eine bereits vorhandene Ressourcengruppe, oder [erstellen Sie eine neue Ressourcengruppe](../azure-resource-manager/resource-group-overview.md).

> [!NOTE]
> Bei der Erstellung eines Analysis Services-Servers wird unter Umständen auch ein neuer abrechenbarer Dienst erstellt. Weitere Informationen finden Sie unter [Analysis Services – Preise](https://azure.microsoft.com/pricing/details/analysis-services/).
> 
> 

## <a name="create-an-analysis-services-server"></a>Erstellen eines Analysis Services-Servers
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Klicken Sie auf **+ Neu** > **Intelligence + Analyse** > **Analysis Services**.
3. Füllen Sie auf dem Blatt **Analysis Services** die erforderlichen Felder aus, und klicken Sie dann auf **Erstellen**.
   
    ![Server erstellen](./media/analysis-services-create-server/aas-create-server-blade.png)
   
   * **Servername:** Geben Sie einen eindeutigen Namen ein, mit dem auf den Server verwiesen wird.
   * **Abonnement:** Wählen Sie das Abonnement aus, für das dieser Server in Rechnung gestellt wird.
   * **Ressourcengruppe:** Dies sind Container, die zum Verwalten einer Azure-Ressourcensammlung entwickelt wurden. Weitere Informationen finden Sie unter [Ressourcengruppen](../azure-resource-manager/resource-group-overview.md).
   * **Standort:** An diesem Standort des Azure-Rechenzentrums wird der Server gehostet. Wählen Sie einen Standort in der Nähe Ihrer größten Benutzerbasis aus.
   * **Tarif:** Wählen Sie einen Tarif aus. Tabellenmodelle bis zu 100 GB werden unterstützt. Sie können den Tarif später jederzeit ändern.
4. Klicken Sie auf **Erstellen**.

Die Erstellung dauert in der Regel weniger als eine Minute, meist nur wenige Sekunden. Wenn Sie **Add to Portal** (Zu Portal hinzufügen) ausgewählt haben, navigieren Sie zu Ihrem Portal, um den neuen Server anzuzeigen. Oder navigieren Sie zu **Weitere Dienste** > **Analysis Services**, um zu überprüfen, ob der Server bereit ist. Wenn er nicht angezeigt wird, aktualisieren Sie die Liste.

 ![Dashboard](./media/analysis-services-create-server/aas-create-server-dashboard.png)

## <a name="automate-server-creation"></a>Automatisieren der Serverbereitstellung
Sie können die Serverbereitstellung mithilfe von Azure Resource Manager-Vorlagendateien dynamisch automatisieren. Sehen Sie sich dieses hilfreiche Video an, um mehr zu erfahren.

>[!VIDEO https://channel9.msdn.com/series/Azure-Analysis-Services/AzureAnalysisServicesAutomation/player]
>
>


## <a name="next-steps"></a>Nächste Schritte
Nach dem Erstellen des Servers können Sie mithilfe von SSDT oder SSMS [ein Modell für den Server bereitstellen](analysis-services-deploy.md).

Wenn ein Modell, das Sie für den Server bereitstellen, mit lokalen Datenquellen verbunden wird, müssen Sie ein [lokales Datengateway](analysis-services-gateway.md) auf einem Computer in Ihrem Netzwerk installieren.




<!--HONumber=Nov16_HO4-->



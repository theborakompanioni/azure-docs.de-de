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
ms.date: 06/01/2017
ms.author: owend
ms.translationtype: Human Translation
ms.sourcegitcommit: 95b8c100246815f72570d898b4a5555e6196a1a0
ms.openlocfilehash: 1782c7e5f7cd710d6e17a26197f9fcc84c967902
ms.contentlocale: de-de
ms.lasthandoff: 05/18/2017


---
# <a name="create-an-azure-analysis-services-server-in-azure-portal"></a>Erstellen eines Azure Analysis Services-Servers im Azure-Portal
In diesem Artikel werden die Schritte zum Erstellen einer Analysis Services-Serverressource in Ihrem Azure-Abonnement beschrieben.

## <a name="before-you-begin"></a>Voraussetzungen
Zunächst benötigen Sie Folgendes:

* **Azure-Abonnement:** Besuchen Sie die Webseite [Kostenlose Azure-Testversion](https://azure.microsoft.com/offers/ms-azr-0044p/), und erstellen Sie ein Konto.
* **Azure Active Directory**: Ihr Abonnement muss einem Azure Active Directory-Mandanten zugeordnet sein. Und Sie müssen bei Azure mit einem Konto in diesem Azure Active Directory angemeldet sein. Microsoft-Konten werden nicht unterstützt. Weitere Informationen finden Sie unter [Benutzerauthentifizierung](analysis-services-overview.md#secure).
* **Ressourcengruppe:** Verwenden Sie eine bereits vorhandene Ressourcengruppe, oder [erstellen Sie eine neue Ressourcengruppe](../azure-resource-manager/resource-group-overview.md).

> [!NOTE]
> Wenn Sie einen Server erstellen, wird dadurch unter Umständen auch ein neuer abrechenbarer Dienst erstellt. Weitere Informationen finden Sie unter [Analysis Services – Preise](https://azure.microsoft.com/pricing/details/analysis-services/).
> 
> 

## <a name="to-create-a-server-in-azure-portal"></a>Erstellen eines Servers im Azure-Portal
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an.
2. Klicken Sie auf **+ Neu** > **Intelligence + Analyse** > **Analysis Services**.
3. Füllen Sie auf dem Blatt **Analysis Services** die erforderlichen Felder aus, und klicken Sie dann auf **Erstellen**.
   
    ![Server erstellen](./media/analysis-services-create-server/aas-create-server-blade.png)
   
   * **Servername:** Geben Sie einen eindeutigen Namen ein, mit dem auf den Server verwiesen wird.
   * **Abonnement:** Wählen Sie das Abonnement aus, für das dieser Server in Rechnung gestellt wird.
   * **Ressourcengruppe:** Diese Container sind auf das Verwalten einer Azure-Ressourcensammlung ausgelegt. Weitere Informationen finden Sie unter [Ressourcengruppen](../azure-resource-manager/resource-group-overview.md).
   * **Standort:** An diesem Standort des Azure-Rechenzentrums wird der Server gehostet. Wählen Sie einen Standort in der Nähe Ihrer größten Benutzerbasis aus.
   * **Tarif:** Wählen Sie einen Tarif aus. Tabellenmodelle von bis zu 400 GB werden unterstützt. Weitere Informationen finden Sie unter [Azure Analysis Services – Preise](https://azure.microsoft.com/pricing/details/analysis-services/).
4. Klicken Sie auf **Erstellen**.

Die Erstellung dauert in der Regel weniger als eine Minute, meist nur wenige Sekunden. Wenn Sie **Add to Portal** (Zu Portal hinzufügen) ausgewählt haben, navigieren Sie zu Ihrem Portal, um den neuen Server anzuzeigen. Oder navigieren Sie zu **Weitere Dienste** > **Analysis Services**, um zu überprüfen, ob der Server bereit ist.

 ![Dashboard](./media/analysis-services-create-server/aas-create-server-dashboard.png)


## <a name="next-steps"></a>Nächste Schritte
Nach dem Erstellen des Servers können Sie mithilfe von SSDT oder SSMS [ein Modell für den Server bereitstellen](analysis-services-deploy.md).

Wenn ein Modell, das Sie für den Server bereitstellen, mit lokalen Datenquellen verbunden wird, müssen Sie ein [lokales Datengateway](analysis-services-gateway.md) auf einem Computer in Ihrem Netzwerk installieren.



---
title: Erstellen eines DocumentDB-Kontos zur Verwendung mit MongoDB-Apps | Microsoft Docs
description: "In diesem Tutorial erfahren Sie, wie Sie ein DocumentDB-Konto mit Protokollunterstützung für MongoDB erstellen (jetzt als Vorschau verfügbar)."
keywords: mongodb apps, mongodb app
services: documentdb
author: AndrewHoh
manager: jhubbard
editor: 
documentationcenter: 
ms.assetid: 413f3ae1-a68e-43be-b0d7-fa2987644f3e
ms.service: documentdb
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/24/2016
ms.author: anhoh
translationtype: Human Translation
ms.sourcegitcommit: 74f80f14a66fa61c19cb5d5fd758cea0ccdd43c5
ms.openlocfilehash: bf72fa88f9444578dd0192e94c89c862273886b2


---

# <a name="create-a-documentdb-account-for-use-with-mongodb-apps"></a>Erstellen eines DocumentDB-Kontos zur Verwendung mit MongoDB-Apps
DocumentDB-Datenbanken können jetzt als Datenspeicher für Apps verwendet werden, die für MongoDB geschrieben wurden. Um diese Funktion verwenden zu können, benötigen Sie ein Azure-Konto und ein DocumentDB-Konto. Dieses Tutorial führt Sie durch die Erstellung eines DocumentDB-Kontos zur Verwendung mit MongoDB-Apps. 

## <a name="prerequisite"></a>Voraussetzung
Ein Azure-Konto. Wenn Sie noch kein Azure-Konto besitzen, erstellen Sie jetzt ein [kostenloses Azure-Konto](https://azure.microsoft.com/free/).
## <a name="create-a-documentdb-account"></a>Erstellen eines DocumentDB-Kontos

1. Melden Sie sich in einem Internetbrowser beim [Azure-Portal](https://portal.azure.com) an.
2. Klicken Sie im linken Navigationsbereich auf **NoSQL (DocumentDB)**.

    ![Screenshot des linken Navigationsbereichs im Portal mit hervorgehobenem Eintrag „NoSQL (DocumentDB)“](./media/documentdb-create-mongodb-account/portalleftnav.png)

3. Alternativ dazu klicken Sie auf **Weitere Dienste >**, geben in der Suchleiste oben den Begriff **DocumentDB** ein und klicken auf **NoSQL (DocumentDB)**.

    ![Screenshot des Blatts „Weitere Dienste“ zum Suchen nach dem Eintrag für NoSQL (DocumentDB)](./media/documentdb-create-mongodb-account/more-services-search.PNG)

4. Klicken Sie auf dem Blatt **NoSQL (DocumentDB)** in der Aktionsleiste oben auf **+ Hinzufügen**.

    ![Screenshot der Schaltfläche „Hinzufügen“ auf dem Ressourcenblatt „NoSQL (DocumentDB)“](./media/documentdb-create-mongodb-account/add-documentdb-account.png)

5. Geben Sie auf dem Blatt **DocumentDB-Konto** die gewünschte Konfiguration für das Konto an.

   ![Screenshot des Blatts für ein neues DocumentDB-Konto mit Protokollunterstützung für MongoDB](./media/documentdb-create-mongodb-account/create-documentdb-mongodb-account.PNG)

    - Geben Sie im Feld **ID** einen Namen zur Identifizierung des Kontos ein.  Nachdem die **ID** überprüft wurde, wird im Feld **ID** ein grünes Häkchen angezeigt. Der Wert von **ID** wird zum Hostnamen im URI. Die **ID** darf nur Kleinbuchstaben, Ziffern und den Bindestrich "-" enthalten und muss zwischen 3 und 50 Zeichen lang sein. Beachten Sie, dass *documents.azure.com* an den gewählten Namen des Endpunkts angehängt wird, um den Endpunkt des Kontos zu erstellen.

    - Als **NoSQL-API** wählen Sie **MongoDB** aus. Damit wird die Kommunikations-API angegeben, die Sie zur Interaktion mit Ihrer DocumentDB-Datenbank verwenden möchten.

    - Wählen Sie unter **Abonnement**das Azure-Abonnement aus, das Sie für das Konto verwenden möchten. Wenn Ihr Konto nur über ein Abonnement verfügt, wird dieses Konto standardmäßig ausgewählt.

    - Unter **Ressourcengruppe**können Sie eine Ressourcengruppe für das Konto auswählen oder erstellen.  Standardmäßig wird eine vorhandene Ressourcengruppe im Azure-Abonnement ausgewählt.  Sie können jedoch eine neue Ressourcengruppe erstellen, zu der Sie Ihr Konto hinzufügen möchten. Weitere Informationen finden Sie unter [Verwenden des Azure-Portals zum Verwalten Ihrer Azure-Ressourcen](../azure-portal/resource-group-portal.md).

    - Unter **Standort** können Sie einen geografischen Standort angeben, an dem das Konto gehostet wird.

6. Klicken Sie nach der Konfiguration der Optionen für das neue Konto auf **Erstellen**.  Die Erstellung des Kontos kann einige Minuten dauern.

   Sie können den Status auch über den Notification Hub überwachen.  

   ![Screenshot des Notification Hubs, in dem gerade das DocumentDB-Konto erstellt wird](./media/documentdb-create-mongodb-account/create-documentdb-mongodb-deployment-status.png)  

7. Klicken Sie zum Zugreifen auf Ihr neues Konto links im Menü auf **DocumentDB (NoSQL)**. Klicken Sie in der Liste der regulären DocumentDB-Konten und DocumentDB-Konten mit Mongo-Protokollunterstützung auf den Namen Ihres neuen Kontos.
8. Ihr DocumentDB-Konto ist jetzt für den Einsatz mit Ihrer MongoDB-App bereit.

   ![Screenshot des Blatts für Standardkonten](./media/documentdb-create-mongodb-account/defaultaccountblade.png)

## <a name="next-steps"></a>Nächste Schritte
* Erfahren Sie, wie Sie eine [Verbindung](documentdb-connect-mongodb-account.md) mit einem DocumentDB-Konto mit Protokollunterstützung für MongoDB herstellen.



<!--HONumber=Nov16_HO4-->



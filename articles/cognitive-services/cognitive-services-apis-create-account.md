---
title: Erstellen eines Cognitive Services-APIs-Kontos im Azure-Portal | Microsoft-Dokumentation
description: Anleitung zum Erstellen eines Microsoft Cognitive Services-APIs-Kontos im Azure-Portal.
services: machine-learning
documentationcenter: 
author: garyericson
manager: jhubbard
editor: cgronlun
ms.assetid: b6176bb2-3bb6-4ebf-84d1-3598ee6e01c6
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 02/28/2017
ms.author: garye;gibattag
translationtype: Human Translation
ms.sourcegitcommit: 1e6ae31b3ef2d9baf578b199233e61936aa3528e
ms.openlocfilehash: a19f56f0b87d992e445034989bd35a827a72f367
ms.lasthandoff: 03/03/2017


---

# <a name="create-a-cognitive-services-apis-account-in-the-azure-portal"></a>Erstellen eines Cognitive Services-APIs-Kontos im Azure-Portal

Sie müssen zuerst ein Konto im Azure-Portal erstellen, um Microsoft Cognitive Services-APIs verwenden zu können.

1. Melden Sie sich beim [Azure-Portal](http://portal.azure.com)an.

2. Klicken Sie auf **+ NEU**.

3. Wählen Sie **Intelligence + Analyse** und anschließend **Cognitive Services-APIs (Vorschau)** aus.

    ![Auswahl Cognitive Services-APIs](media/cognitive-services-apis-create-account/select-cognitive-services-apis.png)

4. Geben Sie auf der Seite **Erstellen** die folgenden Informationen ein:

    -   **Kontoname:** Der Name des Kontos. Es wird empfohlen einen aussagekräftigen Namen zu wählen, z.B. *AFaceAPIAccount*.

    -   **Abonnement:** Wählen Sie eines der verfügbaren Azure-Abonnements aus, für das Sie mindestens Berechtigungen als Teilnehmer besitzen.

    -   **API-Typ:** Wählen Sie die Cognitive Services-API aus, die Sie benutzen möchten. Weitere Informationen zu den verschiedenen verfügbaren Cognitive Services-APIs finden Sie auf der Website von [Cognitive Services](https://azure.microsoft.com/services/cognitive-services/).

    ![Auswählen eines API-Typs](media/cognitive-services-apis-create-account/list-of-apis.png)

    -   **Tarif:** Die Kosten für Ihr Cognitive Services-Konto hängen von der tatsächlichen Nutzung und den ausgewählten Optionen ab. Weitere Informationen zu den Preisen für einzelne APIs finden Sie unter [Preise](https://azure.microsoft.com/pricing/details/cognitive-services/).

    -   **Ressourcengruppe:**Eine Ressourcengruppe ist eine Sammlung von Ressourcen, die in Bezug auf Lebenszyklus, Berechtigungen und Richtlinien gleich sind. Weitere Informationen zu Ressourcengruppen finden Sie unter [Verwalten von Azure-Ressourcen über das Portal](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal).

    -   **Ressourcengruppenstandort:** Ist nur erforderlich, wenn die ausgewählte API global (nicht an einen Standort gebunden) ist. Wenn die API global und nicht an einen Standort gebunden ist, müssen Sie allerdings einen Standort für die Ressourcengruppe angeben, an dem die Metadaten, die dem Cognitive Services-API-Konto zugeordnet sind, gespeichert werden. Dieser Standort hat keine Auswirkung auf die Laufzeitverfügbarkeit Ihres Kontos. Weitere Informationen zu Ressourcengruppen finden Sie unter [Verwalten von Azure-Ressourcen über das Portal](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-portal).

    -   **API-Einstellung:** Standardmäßig ist das Erstellen von Kontos deaktiviert bis Ihr [Azure-Kontoadministrator](https://docs.microsoft.com/azure/active-directory/active-directory-assign-admin-roles) es eindeutig aktiviert.

        Die Änderung dieser Einstellung betrifft nur den aktuell ausgewählten Typ und Standort der API oder den Standort der Ressourcengruppe im linken Bereich.

        ![Erstellen des Cognitive Services-APIs-Konto](media/cognitive-services-apis-create-account/create-account.png)

        > [!NOTE]
        > Wenn Sie eine Benachrichtigung erhalten, dass das Aktualisieren der Einstellung fehlgeschlagen ist, sind Sie nicht als [Kontoadministrator](https://docs.microsoft.com/azure/billing/billing-add-change-azure-subscription-administrator#types-of-azure-admin-accounts) eingeloggt. Der Kontoadministrator muss die vorherigen Schritte befolgen, um das Erstellen zu aktivieren.
        >
        > ![Fehlermeldung Aktualisierung der Einstellung fehlgeschlagen](./media/cognitive-services-apis-create-account/updatefailed.png)
        
        In einigen Fällen hat der Kontoadministrator möglicherweise keinen Zugriff auf das Abonnement. Wenn dies der Fall ist, muss der Dienstadministrator die Schritte im Dokument [Hinzufügen oder Ändern von Azure-Administratorrollen, die das Abonnement oder die Dienste verwalten](https://docs.microsoft.com/azure/billing/billing-add-change-azure-subscription-administrator) ausführen.
        
        Wählen Sie Ihr Abonnement im [Azure-Portal](https://portal.azure.com) und anschließend __Eigenschaften__ aus, um den Konto- oder Dienstadministrator für Ihr Abonnement zu suchen. Informationen zum __Kontoadministrator__ und __Dienstadministrator__ werden am unteren Ende des Blatts „Eigenschaften“ angezeigt.
        
        ![Eigenschaften Abonnement](./media/cognitive-services-apis-create-account/subscription-properties.png)
    
    Microsoft kann Daten, die Sie an Cognitive Services senden, für die Verbesserung von Microsoft-Produkten und -Diensten verwenden. Weitere Informationen finden Sie im [Abschnitt Microsoft Cognitive Services](http://www.microsoft.com/Licensing/product-licensing/products.aspx)
    in den Online Services-Nutzungsbedingungen.

5. Klicken Sie auf **An Dashboard anheften**, um das Konto an das Dashboard des Azure-Portals anzuheften.

6. Klicken Sie auf **Erstellen** , um das Konto zu erstellen.

Klicken Sie auf die Kachel auf dem Dashboard, nachdem das Cognitive Services-Konto erfolgreich bereitgestellt wurde, um die Kontoinformationen anzuzeigen.

Sie können die **Endpunkt-URL** im Bereich **Übersicht** und die Schlüssel im Bereich **Schlüssel** verwenden, um damit zu beginnen API-Aufrufe in Ihrer Anwendung durchzuführen.

![Anzeigen der Kontoinformationen](media/cognitive-services-apis-create-account/display-account.png)

![Anzeigen der Kontoschlüssel](media/cognitive-services-apis-create-account/account-keys.png)

### <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu allen verfügbaren Diensten von Microsoft Cognitive Services finden Sie unter [Cognitive Services](https://azure.microsoft.com/services/cognitive-services/).

- Schnellstartanleitungen zur Verwendung einiger beispielhafter Cognitive Services-APIs finden Sie unter:
    - [Erste Schritte mit den Textanalyse-APIs zur Erkennung von Stimmung, Schlüsselwörtern, Themen und Sprache](cognitive-services-text-analytics-quick-start.md)
    - [Kurzanleitung für die Empfehlungs-API der kognitiven Dienste](cognitive-services-recommendations-quick-start.md)

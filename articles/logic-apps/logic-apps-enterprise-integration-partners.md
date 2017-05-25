---
title: "Erstellen von Partnern für B2B-Nachrichten (Business-to-Business) – Azure Logic Apps | Microsoft Docs"
description: "Erfahren Sie, wie Sie Ihrem Integrationskonto mit dem Enterprise Integration Pack und Logic Apps Partner hinzufügen."
services: logic-apps
documentationcenter: .net,nodejs,java
author: MandiOhlinger
manager: anneta
editor: 
ms.assetid: b179325c-a511-4c1b-9796-f7484b4f6873
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2016
ms.author: LADocs; padmavc
ms.custom: H1Hack27Feb2017
ms.translationtype: Human Translation
ms.sourcegitcommit: 5913c81088724ef946ae147f4f3154fa6aefd22e
ms.openlocfilehash: 2a886d1eaf1253b55d8c95786139a2bbdc6f145e
ms.contentlocale: de-de
ms.lasthandoff: 03/01/2017


---
# <a name="add-or-update-partners-in-business-to-business-agreements-in-your-workflow"></a>Hinzufügen oder Aktualisieren von Partnern in B2B-Vereinbarungen in Ihrem Workflow

Partner sind Entitäten, die an B2B-Transaktionen (Business-To-Business) teilnehmen und gegenseitig Nachrichten austauschen. Bevor Sie Partner erstellen können, die Sie und eine andere Organisation in diesen Transaktionen darstellen, müssen Sie Informationen abstimmen, die zum Identifizieren und Überprüfen der gegenseitig gesendeten Nachrichten dienen. Sobald Sie diese Details abgestimmt haben und bereit sind, Ihre Geschäftsbeziehung zu starten, können Sie in Ihrem Integrationskonto Partner erstellen, die beiden Parteien darstellen.

## <a name="what-roles-do-partners-have-in-your-integration-account"></a>Welche Rollen haben Partner in Ihrem Integrationskonto?

Um Details zu den zwischen Partnern ausgetauschten Nachrichten zu definieren, erstellen Sie Vereinbarungen zwischen diesen Partnern. Bevor Sie jedoch eine Vereinbarung erstellen können, müssen Sie Ihrem Integrationskonto mindestens zwei Partner hinzugefügt haben. Ihre Organisation muss als der **Hostpartner** bei der Vereinbarung sein. Der andere Partner bzw. **Gastpartner** stellt die Organisation dar, die Nachrichten mit Ihrer Organisation austauscht. Der Gastpartner kann ein anderes Unternehmen oder sogar eine Abteilung innerhalb Ihrer eigenen Organisation sein.

Nachdem Sie diese Partner hinzugefügt haben, können Sie eine Vereinbarung erstellen.

Die Empfangs- und Sendeeinstellungen orientieren sich am Hostpartner. Die Empfangseinstellungen in einer Vereinbarung bestimmen beispielsweise, wie der Hostpartner Nachrichten von einem Gastpartner empfängt. Die Sendeeinstellungen in der Vereinbarung geben an, wie der Hostpartner Nachrichten an den Gastpartner sendet.

## <a name="how-to-create-a-partner"></a>Erstellen eines Partners

1. Wählen Sie im Microsoft Azure-Portal **Durchsuchen** aus.

    ![](./media/logic-apps-enterprise-integration-overview/overview-1.png)

2. Geben Sie im Filtersuchfeld das Wort **Integration** ein, und wählen Sie in der Liste mit den Ergebnissen den Eintrag **Integrationskonten** aus.

    ![](./media/logic-apps-enterprise-integration-overview/overview-2.png)

3. Wählen Sie das Integrationskonto aus, dem Sie Ihre Partner hinzufügen möchten.

    ![](./media/logic-apps-enterprise-integration-overview/overview-3.png)

4. Wählen Sie die Kachel **Partner**.

    ![](./media/logic-apps-enterprise-integration-partners/partner-1.png)

5. Wählen Sie auf dem Blatt „Partner“ **Hinzufügen** aus.

    ![](./media/logic-apps-enterprise-integration-partners/partner-2.png)

6. Geben Sie einen Namen für den Partner ein, und wählen Sie dann einen **Qualifizierer** aus. Geben Sie schließlich einen **Wert** ein, mit dessen Hilfe Dokumente identifiziert werden, die in Ihre Apps gelangen.

    ![](./media/logic-apps-enterprise-integration-partners/partner-3.png)

7. Wählen Sie das Benachrichtigungssymbol *Glocke* aus, um den Status des Partnererstellungsprozesses anzuzeigen.

    ![](./media/logic-apps-enterprise-integration-partners/partner-4.png)

8. Um zu bestätigen, dass Ihre neuen Partner erfolgreich hinzugefügt wurden, wählen Sie die Kachel **Partner** aus.

    ![](./media/logic-apps-enterprise-integration-partners/partner-5.png)

    Nachdem Sie die Kachel „Partner“ ausgewählt haben, werden auf dem Blatt „Partner“ auch neu hinzugefügte Partner angezeigt.

    ![](./media/logic-apps-enterprise-integration-partners/partner-6.png)

## <a name="how-to-edit-existing-partners-in-your-integration-account"></a>Bearbeiten der in Ihrem Integrationskonto vorhandenen Partner

1. Wählen Sie die Kachel **Partner**.
2. Wählen Sie auf dem geöffneten Blatt „Partner“ den Partner aus, den Sie bearbeiten möchten.
3. Nehmen Sie auf der Kachel **Partner aktualisieren** die gewünschten Änderungen vor.
4. Klicken Sie anschließend auf **Speichern**. Um Ihre Änderungen zu verwerfen, klicken Sie auf **Verwerfen**.

    ![](./media/logic-apps-enterprise-integration-partners/edit-1.png)

## <a name="how-to-delete-a-partner"></a>Löschen eines Partners

1. Wählen Sie die Kachel **Partner**.
2. Wählen Sie auf dem geöffneten Blatt „Partner“ den Partner aus, den Sie löschen möchten.
3. Wählen Sie **Löschen** aus.

    ![](./media/logic-apps-enterprise-integration-partners/delete-1.png)

## <a name="next-steps"></a>Nächste Schritte
* [Weitere Informationen zu Vereinbarungen](../logic-apps/logic-apps-enterprise-integration-agreements.md "Informationen zu Vereinbarungen zur Unternehmensintegration")  



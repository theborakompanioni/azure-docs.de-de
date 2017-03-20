---
title: "Verwalten von Artefaktmetadaten in Integrationskonten – Azure Logic Apps| Microsoft-Dokumentation"
description: "Hinzufügen oder Abrufen von Artefaktmetadaten aus Integrationskonten für Azure Logic Apps"
author: padmavc
manager: anneta
editor: 
services: logic-apps
documentationcenter: 
ms.assetid: bb7d9432-b697-44db-aa88-bd16ddfad23f
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.custom: H1Hack27Feb2017
ms.date: 11/21/2016
ms.author: padmavc
translationtype: Human Translation
ms.sourcegitcommit: ca8537a7e35e44efafb9b0a9d6ddefe069067475
ms.openlocfilehash: de55bf7d9e68146745c90b96f280c8a71e27b2f9
ms.lasthandoff: 03/02/2017

---

# <a name="manage-artifact-metadata-in-integration-accounts-for-logic-apps"></a>Verwalten von Artefaktmetadaten in Integrationskonten für Logik-Apps

Sie können benutzerdefinierte Metadaten für Artefakte in Integrationskonten definieren und diese Metadaten während der Laufzeit der Logik-App abrufen. Beispielsweise können Sie Metadaten für Artefakte wie Partner, Vereinbarungen, Schemas und Zuordnungen angeben – alle speichern Metadaten mit Schlüssel-Wert-Paaren. Derzeit können Artefakte Metadaten nicht über die Benutzeroberfläche erstellen, aber Sie können mit den REST-APIs Metadaten erstellen. Wählen Sie zum Hinzufügen von Metadaten beim Erstellen oder Wählen eines einer Partner, Vereinbarung oder eines Schemas im Azure-Portal **Als JSON bearbeiten**. Um Artefaktmetadaten in Logik-Apps abzurufen, können Sie die Funktion „Artefaktsuche für Integrationskonto“ verwenden.

## <a name="add-metadata-to-artifacts-in-integration-accounts"></a>Hinzufügen von Metadaten zu Artefakten in Integrationskonten

1. Erstellen Sie ein [Integrationskonto](logic-apps-enterprise-integration-create-integration-account.md).

2. Fügen Sie Ihrem Integrationskonto ein Artefakt hinzu, z. B. [Partner](logic-apps-enterprise-integration-partners.md#how-to-create-a-partner), [Vereinbarung](logic-apps-enterprise-integration-agreements.md#how-to-create-agreements) oder [Schema](logic-apps-enterprise-integration-schemas.md).

3.    Wählen Sie ein Artefakt und dann **Als JSON bearbeiten** aus, und geben Sie Metadatendetails ein.

    ![Eingeben von Metadaten](media/logic-apps-enterprise-integration-metadata/image1.png)

## <a name="retrieve-metadata-from-artifacts-for-logic-apps"></a>Abrufen von Metadaten aus Artefakten für Logik-Apps

1. Erstellen Sie einer [Logik-App](logic-apps-create-a-logic-app.md).

2. Erstellen Sie eine [Verknüpfung Ihrer Logik-App mit Ihrem Integrationskonto](logic-apps-enterprise-integration-create-integration-account.md#link-an-integration-account-to-a-logic-app). 

3. Fügen Sie Ihrer Logik-App im Logik-App-Designer einen Trigger wie *Anforderung* oder *HTTP* hinzu.

4.    Wählen Sie **Nächster Schritt** > **Aktion hinzufügen**. Suchen Sie nach *Integration*, und wählen Sie dann **Integrationskonto – Artefaktsuche für Integrationskonto** aus.

    ![Auswählen der Artefaktsuche für Integrationskonto](media/logic-apps-enterprise-integration-metadata/image2.png)

5. Wählen Sie den **Artefakttyp** aus, und geben Sie den **Namen des Artefakts** ein.

    ![Auswählen des Artefakttyps und Eingeben des Namens des Artefakts](media/logic-apps-enterprise-integration-metadata/image3.png)

## <a name="example-retrieve-partner-metadata"></a>Beispiel: Abrufen von Metadaten für Partner

Metadaten für Partner enthalten diese `routingUrl`-Details:

![Suchen von „RoutingURL“-Metadaten für Partner](media/logic-apps-enterprise-integration-metadata/image6.png)

1. Fügen Sie Ihrer Logik-App Ihren Trigger, eine **Integrationskonto – Artefaktsuche für Integrationskonto**-Aktion für Ihren Partner, und **HTTP** hinzu.

    ![Hinzufügen von Trigger, Artefaktsuche und „HTTP“ zur Logik-App](media/logic-apps-enterprise-integration-metadata/image4.png)

2. Wechseln Sie zur Codeansicht für Ihre Logik-App, um den URI abzurufen. Die Definition der Logik-App sollte wie im folgenden Beispiel aussehen:

    ![Suchen Sie „lookup“ (Suchen).](media/logic-apps-enterprise-integration-metadata/image5.png)


## <a name="next-steps"></a>Nächste Schritte
* [Weitere Informationen zu Vereinbarungen](logic-apps-enterprise-integration-agreements.md "Informationen zu Vereinbarungen zur Unternehmensintegration")  


---
title: "Schemas für XML-Überprüfung: Azure Logic Apps | Microsoft-Dokumentation"
description: "Überprüfen von XML-Dokumenten mit Schemas für Azure Logic Apps und Enterprise Integration Pack"
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: anneta
editor: cgronlun
ms.assetid: 56c5846c-5d8c-4ad4-9652-60b07aa8fc3b
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/29/2016
ms.author: LADocs; padmavc
ms.translationtype: Human Translation
ms.sourcegitcommit: 7884032fd4b5a72ca7e2f2dc2830b0a7c2220e94
ms.openlocfilehash: 5d8ddd4449d3b160dbca7496c801e75dcf4a44e5
ms.contentlocale: de-de
ms.lasthandoff: 02/07/2017


---
# <a name="validate-xml-with-schemas-for-azure-logic-apps-and-the-enterprise-integration-pack"></a>Überprüfen von XML mit Schemas für Azure Logic Apps und Enterprise Integration Pack

Schemas bestätigen, dass empfangene XML-Dokumente gültig sind und die erwarteten Daten in einem vordefinierten Format enthalten. Schemas dienen zum Überprüfen von Nachrichten, die in einem B2B-Szenario ausgetauscht werden.

## <a name="add-a-schema"></a>Hinzufügen eines Schemas

1. Klicken Sie im Azure-Portal auf **Weitere Dienste**.

    ![Azure-Portal, „Weitere Dienste“](media/logic-apps-enterprise-integration-schemas/overview-11.png)

2. Geben Sie im Filtersuchfeld das Wort **Integration** ein, und wählen Sie in der Liste mit den Ergebnissen den Eintrag **Integrationskonten** aus.

    ![Filtersuchfeld](media/logic-apps-enterprise-integration-schemas/overview-21.png)

3. Wählen Sie das **Integrationskonto** aus, dem Sie das Schema hinzufügen möchten.

    ![Liste der Integrationskonten](media/logic-apps-enterprise-integration-schemas/overview-31.png)

4. Wählen Sie die Kachel **Schemas** aus.

    ![Beispiel eines Integrationskontos, „Schemas“](media/logic-apps-enterprise-integration-schemas/schema-11.png)

### <a name="add-a-schema-file-smaller-than-2-mb"></a>Hinzufügen einer Schemadatei, die kleiner als 2 MB ist

1. Wählen Sie auf dem Blatt **Schemas**, das geöffnet wird (vorherige Schritte), die Option **Hinzufügen**.

    ![Blatt „Schemas“, „Hinzufügen“](media/logic-apps-enterprise-integration-schemas/schema-21.png)

2. Geben Sie einen Namen für das Schema ein. Wählen Sie zum Hochladen der Schemadatei das Ordnersymbol neben dem Textfeld **Schema** aus. Klicken Sie nach dem Hochladevorgang auf **OK**.

    ![Screenshot von „Schema hinzufügen“ mit Hervorhebung von „Kleine Datei“](media/logic-apps-enterprise-integration-schemas/schema-31.png)

### <a name="add-a-schema-file-larger-than-2-mb-up-to-8-mb-maximum"></a>Hinzufügen einer Schemadatei, die größer als 2 MB ist (bis maximal 8 MB)

Die Schritte hierfür richten sich nach der Zugriffsebene für den Blobcontainer: **Öffentlich** oder **Kein anonymer Zugriff**.

**So bestimmen Sie diese Zugriffsebene**

1.    Öffnen Sie den **Azure-Speicher-Explorer**. 

2.    Wählen Sie unter **Blobcontainer** den gewünschten Blobcontainer aus. 

3.    Klicken Sie auf **Sicherheit** > **Zugriffsebene**.

Führen Sie die folgenden Schritte aus, wenn die Blob-Sicherheitszugriffsebene **Öffentlich** lautet.

![Azure-Speicher-Explorer mit Hervorhebung von „Blobcontainer“, „Sicherheit“ und „Öffentlich“](media/logic-apps-enterprise-integration-schemas/blob-public.png)

1. Laden Sie das Schema in Ihr Speicherkonto hoch, und kopieren Sie den URI.

    ![Speicherkonto mit hervorgehobenem URI](media/logic-apps-enterprise-integration-schemas/schema-blob.png)

2. Wählen Sie unter **Schema hinzufügen** die Option **Große Datei**, und geben Sie den URI im Textfeld **Inhalts-URI** an.

    ![Schemas mit Hervorhebung der Schaltfläche „Hinzufügen“ und der Option „Große Datei“](media/logic-apps-enterprise-integration-schemas/schema-largefile.png)

Führen Sie die folgenden Schritte aus, wenn die Blob-Sicherheitszugriffsebene **Kein anonymer Zugriff** lautet.

![Azure-Speicher-Explorer mit Hervorhebung von „Blobcontainer“, „Sicherheit“ und „Kein anonymer Zugriff“](media/logic-apps-enterprise-integration-schemas/blob-1.png)

1. Laden Sie das Schema in Ihr Speicherkonto hoch.

    ![Speicherkonto](media/logic-apps-enterprise-integration-schemas/blob-3.png)

2. Generieren Sie eine Shared Access Signature für das Schema.

    ![Speicherkonto mit Hervorhebung der Registerkarte „Shared Access Signatures“](media/logic-apps-enterprise-integration-schemas/blob-2.png)

3. Wählen Sie unter **Schema hinzufügen** die Option **Große Datei**, und geben Sie den Shared Access Signature-URI im Textfeld**Inhalts-URI** an.

    ![Schemas mit Hervorhebung der Schaltfläche „Hinzufügen“ und der Option „Große Datei“](media/logic-apps-enterprise-integration-schemas/schema-largefile.png)

4. Auf dem Blatt **Schemas** Ihres Integrationskontos sollte jetzt das neu hinzugefügte Schema angezeigt werden.

    ![Ihr Integrationskonto mit Hervorhebung von „Schemas“ und des neuen Schemas](media/logic-apps-enterprise-integration-schemas/schema-41.png)

## <a name="edit-schemas"></a>Bearbeiten von Schemas

1. Wählen Sie die Kachel **Schemas** aus.

2. Wählen Sie auf dem geöffneten Blatt **Schemas** das Schema aus, das Sie bearbeiten möchten.

3. Wählen Sie auf dem Blatt **Schemas** die Option **Bearbeiten**.

    ![Blatt „Schemas“](media/logic-apps-enterprise-integration-schemas/edit-12.png)

4. Wählen Sie die Schemadatei, die Sie bearbeiten möchten, und dann **Öffnen** aus.

    ![Öffnen der zu bearbeitenden Schemadatei](media/logic-apps-enterprise-integration-schemas/edit-31.png)

Azure zeigt die Meldung, dass das Schema erfolgreich hochgeladen wurde.

## <a name="delete-schemas"></a>Löschen von Schemas

1. Wählen Sie die Kachel **Schemas** aus.

2. Wählen Sie auf dem geöffneten Blatt **Schemas** das Schema aus, das Sie löschen möchten.

3. Wählen Sie auf dem Blatt **Schemas** die Option **Löschen**.

    ![Blatt „Schemas“](media/logic-apps-enterprise-integration-schemas/delete-12.png)

4. Um zu bestätigen, dass Sie das ausgewählte Schema löschen möchten, klicken Sie auf **Ja**.

    ![Bestätigungsmeldung „Schema löschen“](media/logic-apps-enterprise-integration-schemas/delete-21.png)

    Auf dem Blatt **Schemas** wird die aktualisierte Schemaliste ohne das von Ihnen gelöschte Schema angezeigt.

    ![Ihr Integrationskonto mit Hervorhebung von „Schemas“](media/logic-apps-enterprise-integration-schemas/delete-31.png)

## <a name="next-steps"></a>Nächste Schritte
* [Übersicht über das Enterprise Integration Pack](logic-apps-enterprise-integration-overview.md "Informationen zum Enterprise Integration Pack")  



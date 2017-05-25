---
title: 'Validieren von XML: Azure Logic Apps | Microsoft-Dokumentation'
description: "Validieren von XML mit Schemas für Azure Logic Apps und B2B-Szenarien mit dem Enterprise Integration Pack"
services: logic-apps
documentationcenter: .net,nodejs,java
author: msftman
manager: anneta
editor: cgronlun
ms.assetid: d700588f-2d8a-4c92-93eb-e1e6e250e760
ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/08/2016
ms.author: LADocs; padmavc
ms.translationtype: Human Translation
ms.sourcegitcommit: 03cd3f4edd7bb7895efa02475411d813ef44b8b3
ms.openlocfilehash: ae0bb2cb090402f61bb10299e50024f1f2938489
ms.contentlocale: de-de
ms.lasthandoff: 02/07/2017


---
# <a name="validate-xml-for-enterprise-integration"></a>Validieren von XML für die Unternehmensintegration

In B2B-Szenarien müssen die Partner in einer Vereinbarung sicherstellen, dass die Nachrichten, die sie austauschen, gültig sind, ehe die Datenverarbeitung gestartet werden kann. Im Enterprise Integration Pack können Sie den XML-Connector „Überprüfung“ verwenden, um Dokumente im Abgleich mit einem vordefinierten Schema auf Gültigkeit zu überprüfen.

## <a name="validate-a-document-with-the-xml-validation-connector"></a>Validieren eines Dokuments mit dem XML-Connector „Überprüfung“

1. Erstellen Sie eine Logik-App, und [verknüpfen Sie sie mit Ihrem Integrationskonto](../logic-apps/logic-apps-enterprise-integration-accounts.md "Erfahren Sie, wie Sie ein Integrationskonto mit einer Logik-App verknüpfen"), das das Schema enthält, das Sie zum Überprüfen der XML-Daten verwenden.

2. Fügen Sie Ihrer Logik-App den Trigger **Anforderung – Wenn eine HTTP-Anforderung empfangen wird** hinzu.

    ![](./media/logic-apps-enterprise-integration-xml/xml-1.png)

3. Fügen Sie die Aktion **XML-Überprüfung** hinzu, indem Sie **Aktion hinzufügen** auswählen.

4. Geben Sie *xml* in das Suchfeld ein, um die Aktion herauszufiltern, die Sie verwenden möchten. Wählen Sie **XML-Überprüfung** aus.

    ![](./media/logic-apps-enterprise-integration-xml/xml-2.png)

5. Um den XML-Inhalt anzugeben, den Sie überprüfen möchten, wählen Sie **INHALT**.

    ![](./media/logic-apps-enterprise-integration-xml/xml-1-5.png)

6. Wählen Sie das „Body“-Tag als den Inhalt aus, den Sie überprüfen möchten.

    ![](./media/logic-apps-enterprise-integration-xml/xml-3.png)

7. Um das Schema anzugeben, das Sie für die Überprüfung der vorherigen Eingabe in *Inhalt* verwenden möchten, wählen Sie **SCHEMANAME**.

    ![](./media/logic-apps-enterprise-integration-xml/xml-4.png)

8. Speichern Sie Ihre Arbeit.  

    ![](./media/logic-apps-enterprise-integration-xml/xml-5.png)

Das Einrichten des Connectors für die Überprüfung ist damit abgeschlossen. In einer realen Anwendung werden die überprüften Daten in einer branchenspezifischen App wie Salesforce gespeichert. Um die überprüfte Ausgabe an Salesforce zu senden, fügen Sie eine Aktion hinzu.

Sie können Ihre Überprüfungsaktion testen, indem Sie eine Anforderung an den HTTP-Endpunkt richten.

## <a name="next-steps"></a>Nächste Schritte
[Weitere Informationen zum Enterprise Integration Pack](../logic-apps/logic-apps-enterprise-integration-overview.md "Informationen zum Enterprise Integration Pack")   



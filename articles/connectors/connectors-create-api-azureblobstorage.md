---
title: "Hinzufügen des Azure Blob Storage-Connectors in Ihren Logik-Apps | Microsoft Docs"
description: Verwenden und Konfigurieren des Azure Blob Storage-Connectors in einer Logik-App
services: 
documentationcenter: 
author: MandiOhlinger
manager: anneta
editor: 
tags: connectors
ms.assetid: b5dc3f75-6bea-420b-b250-183668d2848d
ms.service: logic-apps
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 05/02/2017
ms.author: mandia; ladocs
ms.translationtype: Human Translation
ms.sourcegitcommit: c785ad8dbfa427d69501f5f142ef40a2d3530f9e
ms.openlocfilehash: bc7908868828bd1628633cf9e57f8c44f8000827
ms.contentlocale: de-de
ms.lasthandoff: 05/26/2017


---
# <a name="use-the-azure-blob-storage-connector-in-a-logic-app"></a>Verwenden des Azure Blob Storage-Connectors in einer Logik-App
Mit dem Azure Blob Storage-Connector können Sie Blobs in Ihr(em) Speicherkonto hochladen, aktualisieren, abrufen und löschen – und all das in einer Logik-App.  

Azure-Blobspeicher ermöglicht Folgendes:

* Gestalten Sie Ihren Workflow, indem Sie neue Projekte hochladen oder kürzlich aktualisierte Dateien abrufen.
* Mithilfe von Aktionen können Sie unter anderem Dateimetadaten abrufen, eine Datei löschen und Dateien kopieren. Wenn also beispielsweise ein Tool auf einer Azure-Website aktualisiert wird (Trigger), kann eine Datei in Blob Storage aktualisiert werden (Aktion). 

In diesem Thema wird beschrieben, wie Sie den Blob Storage-Connector in einer Logik-App verwenden.

Weitere Informationen zu Logik-Apps finden Sie unter [Was sind Logik-Apps](../logic-apps/logic-apps-what-are-logic-apps.md) sowie unter [Erstellen einer Logik-App zum Verbinden von SaaS-Diensten](../logic-apps/logic-apps-create-a-logic-app.md).

Weitere Informationen zu Logik-Apps finden Sie unter [Was sind Logik-Apps](../logic-apps/logic-apps-what-are-logic-apps.md) sowie unter [Erstellen einer Logik-App zum Verbinden von SaaS-Diensten](../logic-apps/logic-apps-create-a-logic-app.md).

## <a name="connect-to-azure-blob-storage"></a>Herstellen einer Verbindung mit Azure Blob Storage
Damit Ihre Logik-App überhaupt auf einen Dienst zugreifen kann, muss zunächst eine *Verbindung* mit dem Dienst hergestellt werden. Eine Verbindung stellt den Kontakt zwischen einer Logik-App und einem anderen Dienst her. Um eine Verbindung mit einem Speicherkonto herzustellen, müssen Sie beispielsweise zuerst eine Blob Storage-*Verbindung* erstellen. Hierzu geben Sie die Anmeldeinformationen ein, mit denen Sie normalerweise auf den Dienst zugreifen, mit dem Sie eine Verbindung herstellen möchten. Geben Sie für Azure Storage also die Anmeldeinformationen Ihres Speicherkontos ein, um die Verbindung zu erstellen. 

#### <a name="create-the-connection"></a>Erstellen der Verbindung
> [!INCLUDE [Create a connection to Azure blob storage](../../includes/connectors-create-api-azureblobstorage.md)]

## <a name="use-a-trigger"></a>Verwenden eines Triggers
Dieser Connector verfügt über keine Trigger. Verwenden Sie andere Trigger, um die Logik-App zu starten – beispielsweise einen Wiederholungstrigger, einen HTTP-Webhook-Trigger oder einen Trigger eines anderen Connectors. Unter [Erstellen einer Logik-App zum Verbinden von SaaS-Diensten](../logic-apps/logic-apps-create-a-logic-app.md) finden Sie ein Beispiel.

## <a name="use-an-action"></a>Verwenden einer Aktion
Eine Aktion ist ein Vorgang, der durch den in einer Logik-App definierten Workflow ausgeführt wird.

1. Wählen Sie das Pluszeichen aus. Es stehen mehrere Auswahlmöglichkeiten zur Verfügung: **Aktion hinzufügen**, **Bedingung hinzufügen** oder eine der Optionen unter **Mehr**.
   
    ![](./media/connectors-create-api-azureblobstorage/add-action.png)
2. Wählen Sie **Aktion hinzufügen**aus.
3. Geben Sie im Textfeld die Zeichenfolge „blob“ ein, um eine Liste mit allen verfügbaren Aktionen zu erhalten.
   
    ![](./media/connectors-create-api-azureblobstorage/actions.png) 
4. In unserem Beispiel wählen wir **AzureBlob - Get file metadata using path** (AzureBlob – Dateimetadaten anhand des Pfads abrufen) aus. Falls bereits eine Verbindung vorhanden ist, wählen Sie die Schaltfläche **...** aus. (Auswahl anzeigen) aus, um eine Datei auszuwählen.
   
    ![](./media/connectors-create-api-azureblobstorage/sample-file.png)
   
    Wenn Sie zur Eingabe der Verbindungsinformationen aufgefordert werden, geben Sie die Details ein, um die Verbindung zu erstellen. Die Eigenschaften werden in diesem Thema unter [Erstellen der Verbindung](connectors-create-api-azureblobstorage.md#create-the-connection) beschrieben. 
   
   > [!NOTE]
   > In diesem Beispiel rufen wir die Metadaten einer Datei ab. Fügen Sie zum Anzeigen der Metadaten eine weitere Aktion hinzu, die unter Verwendung eines anderen Connectors eine neue Datei erstellt. Fügen Sie beispielsweise eine OneDrive-Aktion hinzu, die auf der Grundlage der Metadaten eine neue Testdatei erstellt. 


5. Speichern Sie Ihre Änderungen. (Die Option **Speichern** befindet sich links oben auf der Symbolleiste.) Ihre Logik-App wird gespeichert und ggf. automatisch aktiviert.

> [!TIP]
> Der [Speicher-Explorer](http://storageexplorer.com/) ist ein praktisches Verwaltungstool für mehrere Speicherkonten.

## <a name="connector-specific-details"></a>Connectorspezifische Details

Zeigen Sie die in Swagger definierten Trigger und Aktionen sowie mögliche Beschränkungen in den [Connectordetails](/connectors/azureblobconnector/) an. 

## <a name="next-steps"></a>Nächste Schritte
[Erstellen Sie eine Logik-App](../logic-apps/logic-apps-create-a-logic-app.md). Informieren Sie sich in unserer [API-Liste](apis-list.md)über die anderen verfügbaren Connectors für Logik-Apps.



---
title: "Aktivieren von Event Hubs Capture über das Portal | Microsoft-Dokumentation"
description: "Aktivieren Sie die Event Hubs Capture-Feature über das Azure-Portal."
services: event-hubs
documentationcenter: 
author: sethmanheim
manager: timlt
editor: 
ms.assetid: 
ms.service: event-hubs
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 08/28/2017
ms.author: sethm
ms.translationtype: HT
ms.sourcegitcommit: 2c6cf0eff812b12ad852e1434e7adf42c5eb7422
ms.openlocfilehash: 4a4ab1ee022b6b33d35217df916d01f32e04d3a4
ms.contentlocale: de-de
ms.lasthandoff: 09/13/2017

---

# <a name="enable-event-hubs-capture-using-the-azure-portal"></a>Aktivieren von Event Hubs Capture über das Azure-Portal

Mit Azure [Event Hubs Capture][capture-overview] können Sie die Streamingdaten in Event Hubs automatisch an ein [Azure Blob Storage](https://azure.microsoft.com/services/storage/blobs/)- oder [Azure Data Lake Store](https://azure.microsoft.com/services/data-lake-store/)-Konto Ihrer Wahl übermitteln.

Sie können Capture über das [Azure-Portal](https://portal.azure.com) zum Zeitpunkt der Event Hub-Erstellung konfigurieren. Die Daten können entweder in einem Azure [Blob Storage](https://azure.microsoft.com/services/storage/blobs/)-Container oder in einem [Azure Data Lake Store](https://azure.microsoft.com/services/data-lake-store/)-Konto erfasst werden.

Weitere Informationen finden Sie in der [Übersicht über Event Hubs Capture][capture-overview].

## <a name="capture-data-to-an-azure-storage-account"></a>Erfassen von Daten in einem Azure Storage-Konto  

Beim Erstellen eines Event Hubs können Sie Capture aktivieren, indem Sie auf dem Portalblatt **Event Hub erstellen** auf die Schaltfläche **Ein** klicken. Anschließend geben Sie ein Speicherkonto und einen Container an, indem Sie im Feld **Capture Provider** (Capture-Anbieter) auf **Azure Storage** klicken. Da Event Hubs Capture die Dienst-zu-Dienst-Authentifizierung mit Speicher verwendet, müssen Sie keine Speicherverbindungszeichenfolge angeben. Die Ressourcenauswahl wählt automatisch den Ressourcen-URI für Ihr Speicherkonto. Wenn Sie Azure Resource Manager verwenden, müssen Sie diesen URI explizit als Zeichenfolge angeben.

Das Standadzeitfenster beträgt fünf Minuten. Der Mindestwert ist 1, der Höchstwert 15. Für das **Größenfenster** gilt ein Bereich von 10 bis 500 MB.

![][1]

## <a name="capture-data-to-an-azure-data-lake-store-account"></a>Erfassen von Daten in einem Azure Data Lake Store-Konto

Wenn Sie Daten in Azure Data Lake Store erfassen möchten, müssen Sie ein Data Lake Store-Konto und einen Event Hub erstellen:

### <a name="create-an-azure-data-lake-store-account-and-folders"></a>Erstellen eines Azure Data Lake Store-Kontos und Erstellen von Ordnern

1. Erstellen Sie gemäß der Anleitung unter [Erste Schritte mit Azure Data Lake Store über das Azure-Portal](../data-lake-store/data-lake-store-get-started-portal.md) ein Data Lake Store-Konto.
2. Führen Sie die Anleitungsschritte im Abschnitt [Zuweisen von Berechtigungen für Event Hubs](../data-lake-store/data-lake-store-archive-eventhub-capture.md#assign-permissions-to-event-hubs) aus, um unter dem Data Lake Store-Konto einen Ordner zu erstellen, in dem Sie die Daten aus Event Hubs erfassen möchten. Sie können für Event Hubs auch Berechtigungen zuweisen, damit diese Daten in Ihr Data Lake Store-Konto schreiben können.  

### <a name="create-an-event-hub"></a>Erstellen eines Ereignis-Hubs

1. Der Event Hub muss dem gleichen Azure-Abonnement angehören wie die Azure Data Lake Store-Instanz, die Sie soeben erstellt haben. Erstellen Sie den Event Hub, indem Sie auf der Portalseite **Event Hub erstellen** unter **Capture** auf die Schaltfläche **Ein** klicken. 
2. Wählen Sie auf der Portalseite **Event Hub erstellen** im Feld **Capture Provider** (Capture-Anbieter) die Option **Azure Data Lake Store** aus.
3. Geben Sie unter **Data Lake Store auswählen** das zuvor erstellte Data Lake Store-Konto und im Feld für den **Data Lake-Pfad** den Pfad zu dem von Ihnen erstellten Datenordner an.

    ![][3]

## <a name="add-or-configure-capture-on-an-existing-event-hub"></a>Hinzufügen oder Konfigurieren von Capture für einen bereits vorhandenen Event Hub

Capture kann für vorhandene Event Hubs in Event Hubs-Namespaces konfiguriert werden. Wenn Sie Capture für einen vorhandenen Event Hub aktivieren oder Ihre Capture-Einstellungen ändern möchten, klicken Sie auf den Namespace, um den Bildschirm **Zusammenfassung** zu laden. Klicken Sie dann auf den Event Hub, den Sie aktivieren bzw. dessen Capture-Einstellung Sie ändern möchten. Klicken Sie abschließend auf den Abschnitt **Eigenschaften** der geöffneten Seite, und bearbeiten Sie die Capture-Einstellungen, wie in der folgenden Abbildung dargestellt:

### <a name="azure-blob-storage"></a>Azure Blob Storage

![][2]

### <a name="azure-data-lake-store"></a>Azure Data Lake Store

![][4]

[1]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture1.png
[2]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture2.png
[3]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture3.png
[4]: ./media/event-hubs-capture-enable-through-portal/event-hubs-capture4.png

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen zu Event Hubs Capture finden Sie in der [Übersicht über Event Hubs Capture][capture-overview].
- Sie können Event Hubs Capture auch über Azure Resource Manager-Vorlagen konfigurieren. Weitere Informationen finden Sie unter [Aktivieren von Capture über eine Azure Resource Manager-Vorlage](event-hubs-resource-manager-namespace-event-hub-enable-capture.md).
- [Erste Schritte mit Azure Data Lake Store über das Azure-Portal](../data-lake-store/data-lake-store-get-started-portal.md)

[capture-overview]: event-hubs-capture-overview.md

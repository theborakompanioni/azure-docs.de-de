---
title: Verschieben von Daten in den und aus dem Blobspeicher mithilfe des Azure-Speicher-Explorers | Microsoft-Dokumentation
description: Verschieben von Daten in den und aus dem Azure-Blobspeicher mithilfe des Azure-Speicher-Explorers
services: machine-learning,storage
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: 10bd283f-0875-4c67-af63-6492270b7656
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/24/2017
ms.author: bradsev
translationtype: Human Translation
ms.sourcegitcommit: 69bb7e5fb0dafa3a9d0f6ad4eb271ab1af2edc50
ms.openlocfilehash: a258404e659bd6d6e7e0f468a5f3ce5e184be2a3
ms.lasthandoff: 01/31/2017


---
# <a name="move-data-to-and-from-azure-blob-storage-using-azure-storage-explorer"></a>Verschieben von Daten in und aus Azure Blob Storage mithilfe des Azure-Speicher-Explorers
Der Azure-Speicher-Explorer ist ein kostenloses Tool von Microsoft, das Ihnen das Arbeiten mit Azure Storage-Daten unter Windows, MacOS und Linux ermöglicht. Dieses Thema beschreibt Verwendung des Tools zum Hoch- und Herunterladen von Daten aus einem Azure-Blobspeicher. Das Tool kann von der Seite [Microsoft Azure Storage Explorer](http://storageexplorer.com/)heruntergeladen werden.

[!INCLUDE [blob-storage-tool-selector](../../includes/machine-learning-blob-storage-tool-selector.md)]

> [!NOTE]
> Wenn Sie einen virtuellen Computer verwenden, der mit den von den [virtuellen Data Science-Computern in Azure](machine-learning-data-science-virtual-machines.md)bereitgestellten Skripts eingerichtet wurde, ist der Azure-Speicher-Explorer bereits auf dem virtuellen Computer installiert.
> 
> [!NOTE]
> Eine umfassende Einführung in Azure-Blobspeicher finden Sie unter [Grundlagen zu Azure-Blobspeicher](../storage/storage-dotnet-how-to-use-blobs.md) und [Azure-Blobdienst](https://msdn.microsoft.com/library/azure/dd179376.aspx).   
> 
> 

## <a name="prerequisites"></a>Voraussetzungen
In diesem Dokument wird davon ausgegangen, dass Sie über ein Azure-Abonnement, ein Speicherkonto und den zugehörigen Speicherschlüssel für dieses Konto verfügen. Bevor Sie Daten hoch- und herunterladen können, müssen Sie den Namen Ihres Azure-Speicherkontos und den Kontoschlüssel kennen. 

* Informationen zum Einrichten eines Azure-Abonnements finden Sie unter [Kostenlose Testversion](https://azure.microsoft.com/pricing/free-trial/).
* Anleitungen zum Erstellen eines Speicherkontos und zum Abrufen von Konto- und Schlüsselinformationen finden Sie unter [Informationen zu Azure-Speicherkonten](../storage/storage-create-storage-account.md). Notieren Sie sich den Zugriffsschlüssel für Ihr Speicherkonto, da Sie diesen Schlüssel benötigen, um über das Tool Azure-Speicher-Explorer eine Verbindung mit dem Konto herzustellen.
* Der Azure-Speicher-Explorer kann von der Seite [Microsoft Azure Storage Explorer](http://storageexplorer.com/)heruntergeladen werden. Übernehmen Sie während der Installation die Standardwerte.

<a id="explorer"></a>

## <a name="use-azure-storage-explorer"></a>Verwenden von Azure Storage Explorer
Die folgenden Schritte beschreiben das Hoch- und Herunterladen von Daten mithilfe von Azure Storage Explorer. 

1. Starten Sie den Microsoft Azure-Speicher-Explorer.
2. Um den Assistenten **Bei Ihrem Konto anmelden...** zu öffnen, wählen Sie das Symbol **Azure-Kontoeinstellungen** und dann die Option **Konto hinzufügen** aus, und geben Sie Ihre Anmeldeinformationen ein. ![Azure Storage-Konto hinzufügen](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/add-an-azure-store-account.png)
3. Um den Assistenten **Verbindung mit Azure-Speicher herstellen** zu öffnen, wählen Sie das Symbol **Verbindung mit Azure-Speicher herstellen** aus. ![Verbindung mit Azure-Speicher herstellen](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/connect-to-azure-storage-1.png)
4. Geben Sie den Zugriffsschlüssel Ihres Azure-Speicherkontos in den Assistenten **Verbindung mit Azure-Speicher herstellen** ein, und klicken Sie dann auf **Weiter**. ![Verbindung mit Azure-Speicher herstellen](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/connect-to-azure-storage-2.png)
5. Geben Sie im Feld **Kontoname** den Namen des Speicherkontos ein, und wählen Sie **Weiter** aus. ![Externen Speicher anfügen](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/attach-external-storage.png)
6. Das hinzugefügte Speicherkonto sollte jetzt aufgeführt werden. Um in einem Speicherkonto einen Blobcontainer zu erstellen, klicken Sie mit der rechten Maustaste auf den Knoten **Blobcontaine**r in diesem Konto, wählen Sie die Option **Blobcontainer erstellen** aus, und geben Sie einen Namen ein.
7. Um Daten in einen Container hochzuladen, wählen Sie den Zielcontainer aus, und klicken Sie auf die Schaltfläche **Hochladen**.![Speicherkonten](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/storage-accounts.png)
8. Klicken Sie rechts neben dem Feld **Dateien** auf die Schaltfläche **...**, wählen Sie im Dateisystem eine oder mehrere Dateien zum Hochladen aus, und klicken Sie auf **Hochladen**, um mit dem Hochladen der Dateien zu beginnen.![Dateien hochladen](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/upload-files-to-blob.png)
9. Um Daten herunterzuladen, wählen Sie das Blob im entsprechenden Container aus und klicken auf **Herunterladen**. ![Dateien herunterladen](./media/machine-learning-data-science-move-data-to-azure-blob-using-azure-storage-explorer/download-files-from-blob.png)



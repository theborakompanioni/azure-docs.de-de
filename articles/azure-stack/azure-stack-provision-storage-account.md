---
title: Speicherkonten in Azure Stack | Microsoft-Dokumentation
description: Hier erfahren Sie, wie Sie ein Azure Stack-Speicherkonto erstellen.
services: azure-stack
documentationcenter: 
author: vhorne
manager: byronr
editor: 
ms.assetid: e1152110-b756-4c1a-9fa2-73fe3ab0ad8e
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 3/1/2017
ms.author: victorh
ms.translationtype: HT
ms.sourcegitcommit: 83f19cfdff37ce4bb03eae4d8d69ba3cbcdc42f3
ms.openlocfilehash: de32c0ce79e8357274cc19cd1ea4ec23b85b918f
ms.contentlocale: de-de
ms.lasthandoff: 09/15/2017

---
# <a name="storage-accounts-in-azure-stack"></a>Speicherkonten in Azure Stack
Speicherkonten enthalten Blob-und Tabellenspeicherdienste und den eindeutigen Namespace für Ihre Speicherdatenobjekte. Standardmäßig sind die Daten in Ihrem Konto nur für Sie als Speicherkontobesitzer verfügbar.

1. Melden Sie sich auf dem Azure Stack POC-Computer unter `https://adminportal.local.azurestack.external` als [Administrator](azure-stack-connect-azure-stack.md) an, und klicken Sie auf **Neu** > **Daten und Speicher** > **Speicherkonto**.

   ![](media/azure-stack-provision-storage-account/image01.png)
2. Geben Sie auf dem Blatt **Speicherkonto erstellen** einen Namen für das Speicherkonto ein. Erstellen Sie eine neue **Ressourcengruppe**, oder wählen Sie eine vorhandene aus, und klicken Sie auf **Erstellen**, um das Speicherkonto zu erstellen.

   ![](media/azure-stack-provision-storage-account/image02.png)
3. Klicken sie zum Anzeigen des neuen Speicherkontos auf **Alle Ressourcen**, suchen Sie dann nach dem Speicherkonto, und klicken Sie auf seinen Namen.

    ![](media/azure-stack-provision-storage-account/image03.png)

### <a name="next-steps"></a>Nächste Schritte
[Verwenden von Azure-Ressourcen-Manager-Vorlagen](azure-stack-arm-templates.md)

[Informationen zu Azure-Speicherkonten](../storage/common/storage-create-storage-account.md)

[Herunterladen des Azure Stack-Überprüfungshandbuchs für mit Azure konsistentem Speicher](http://aka.ms/azurestacktp1doc)


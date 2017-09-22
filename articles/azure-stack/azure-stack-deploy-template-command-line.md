---
title: Bereitstellen von Vorlagen mithilfe der Befehlszeile in Azure Stack | Microsoft-Dokumentation
description: "Hier erfahren Sie, wie Sie mit der plattformübergreifenden Befehlszeilenschnittstelle (Command Line Interface, CLI) Vorlagen in Azure Stack bereitstellen."
services: azure-stack
documentationcenter: 
author: heathl17
manager: byronr
editor: 
ms.assetid: 9584177f-4af3-4834-864d-930b09ae0995
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: helaw
ms.translationtype: HT
ms.sourcegitcommit: d941879aee6042b38b7f5569cd4e31cb78b4ad33
ms.openlocfilehash: cd1b61899ead7b4e86a81125841c1b37d019280b
ms.contentlocale: de-de
ms.lasthandoff: 09/15/2017

---
# <a name="deploy-templates-in-azure-stack-using-the-command-line"></a>Bereitstellen von Vorlagen in Azure Stack mithilfe der Befehlszeile
Verwenden Sie die Befehlszeile zum Bereitstellen von Azure Resource Manager-Vorlagen im Azure Stack Development Kit. Azure Resource Manager-Vorlagen stellen alle Ressourcen für Ihre Anwendung in einem einzigen, koordinierten Vorgang bereit.

## <a name="before-you-begin"></a>Voraussetzungen
 - Verwenden Sie die Azure CLI, um Azure Stack zu [installieren und eine Verbindung herzustellen](azure-stack-connect-cli.md).
 - Laden Sie die Dateien *azuredeploy.json* und *azuredeploy.parameters.json* aus der [Beispielvorlage zum Erstellen von Speicherkonten](https://github.com/Azure/AzureStack-QuickStart-Templates/tree/master/101-create-storage-account) herunter.
 
## <a name="deploy-template"></a>Bereitstellen der Vorlage
Navigieren Sie zu dem Ordner, in den diese Dateien heruntergeladen wurden, und führen Sie den folgenden Befehl zum Bereitstellen der Vorlage aus:

    azure group create "cliRG" "local" –f azuredeploy.json –d "testDeploy" –e azuredeploy.parameters.json

Dieser Befehl stellt die Vorlage für die Ressourcengruppe **cliRG** am Standardspeicherort für Azure Stack POC bereit.

## <a name="validate-template-deployment"></a>Überprüfen der Vorlagenbereitstellung
Um diese Ressourcengruppe und das Speicherkonto anzuzeigen, verwenden Sie die folgenden Befehle:

    azure group list

    azure storage account list

## <a name="next-steps"></a>Nächste Schritte
[Verwalten von Benutzerberechtigungen](azure-stack-manage-permissions.md)



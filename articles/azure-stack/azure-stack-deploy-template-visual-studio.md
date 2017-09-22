---
title: Bereitstellen von Vorlagen mithilfe von Visual Studio in Azure Stack | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Vorlagen mit Visual Studio in Azure Stack bereitstellen.
services: azure-stack
documentationcenter: 
author: HeathL17
manager: byronr
editor: 
ms.assetid: 628da2ae-64cc-42e0-b8b7-a6a3724cb974
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: helaw
ms.translationtype: HT
ms.sourcegitcommit: d941879aee6042b38b7f5569cd4e31cb78b4ad33
ms.openlocfilehash: e9b467f47f166198d9790f19dbdd3d1d0fd79947
ms.contentlocale: de-de
ms.lasthandoff: 09/15/2017

---
# <a name="deploy-templates-in-azure-stack-using-visual-studio"></a>Bereitstellen von Vorlagen in Azure Stack mithilfe von Visual Studio

Verwenden Sie Visual Studio zum Bereitstellen von Azure Resource Manager-Vorlagen im Azure Stack Development Kit.

1. Verwenden Sie Visual Studio, um Azure Stack zu [installieren und eine Verbindung herzustellen](azure-stack-install-visual-studio.md).
2. Öffnen Sie Visual Studio.
3. Klicken Sie auf **Datei**, auf **Neu** und dann im Dialogfeld **Neues Projekt** auf **Azure-Ressourcengruppe**.
4. Geben Sie einen **Namen** für das neue Projekt ein, und klicken Sie auf **OK**.
5. Wählen Sie im Dialogfeld **Azure-Vorlage auswählen** in der Dropdownliste *Vorlagen aus diesem Speicherort anzeigen* den Eintrag **Azure Stack-Schnellstart**.
6. Klicken Sie auf **101-create-storage-account** und dann auf **OK**.  
7. Im neuen Projekt sehen Sie eine Liste der verfügbaren Vorlagen, wenn Sie den Knoten **Vorlagen** im Bereich **Projektmappen-Explorer** erweitern.
8. Klicken Sie im Bereich **Projektmappen-Explorer** mit der rechten Maustaste auf den Namen des Projekts, klicken Sie anschließend auf **Bereitstellen** und dann auf **Neue Bereitstellung**.
9. Wählen Sie im Dialogfeld **Für Ressourcengruppe bereitstellen** in der Dropdownliste **Abonnement** Ihr Microsoft Azure Stack-Abonnement aus.
10. Wählen Sie in der Liste **Ressourcengruppe** eine vorhandene Ressourcengruppe aus, oder erstellen Sie eine neue.
11. Wählen Sie in der Liste **Ressourcengruppenstandort** einen Standort aus, und klicken Sie dann auf **Bereitstellen**.
12. Geben Sie im Dialogfeld **Parameter bearbeiten** Werte für die Parameter ein (je nach Vorlage unterschiedlich), und klicken Sie dann auf **Speichern**.

## <a name="next-steps"></a>Nächste Schritte
[Bereitstellen von Vorlagen mithilfe der Befehlszeile](azure-stack-deploy-template-command-line.md)

[Entwickeln von Vorlagen für Azure Stack](azure-stack-develop-templates.md)



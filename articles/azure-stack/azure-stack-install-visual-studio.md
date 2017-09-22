---
title: Installieren von Visual Studio und Herstellen einer Verbindung mit Azure Stack | Microsoft-Dokumentation
description: Erfahren Sie, welche Schritte zum Installieren von Visual Studio und zum Herstellen einer Verbindung mit Azure Stack erforderlich sind.
services: azure-stack
documentationcenter: 
author: heathl17
manager: byronr
editor: 
ms.assetid: 2022dbe5-47fd-457d-9af3-6c01688171d7
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/10/2017
ms.author: sngun
ms.translationtype: HT
ms.sourcegitcommit: 25e4506cc2331ee016b8b365c2e1677424cf4992
ms.openlocfilehash: 62ad9ebfd829d9555b9e4bc70f8a7f0c8ff0f901
ms.contentlocale: de-de
ms.lasthandoff: 09/15/2017

---

# <a name="install-visual-studio-and-connect-to-azure-stack"></a>Installieren von Visual Studio und Herstellen einer Verbindung mit Azure Stack

Verwenden Sie Visual Studio zum Erstellen und Bereitstellen von Azure Resource Manager-[Vorlagen](azure-stack-arm-templates.md) in Azure Stack. Führen Sie die in diesem Artikel beschriebenen Schritte aus, um Visual Studio entweder über das [Azure Stack Development Kit](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-remote-desktop) oder über einen Windows-basierten externen Client (wenn Sie über [VPN](azure-stack-connect-azure-stack.md#connect-to-azure-stack-with-vpn) verbunden sind) zu installieren. Mit diesen Schritten führen Sie eine neue Installation von Visual Studio 2015 Community Edition durch. Lesen Sie mehr über die [Koexistenz](https://msdn.microsoft.com/library/ms246609.aspx) zwischen anderen Versionen von Visual Studio.

## <a name="install-visual-studio"></a>Installieren von Visual Studio
1. Laden Sie den [Webplattform-Installer](https://www.microsoft.com/web/downloads/platform.aspx) herunter, und führen Sie ihn aus.             
2. Suchen Sie nach **Visual Studio Community 2015 with Microsoft Azure SDK - 2.9.6**, klicken Sie auf **Hinzufügen** und anschließend auf **Installieren**.

    ![Screenshot der WebPI-Installationsschritte](./media/azure-stack-install-visual-studio/image1.png) 

3. Deinstallieren Sie das **Microsoft Azure PowerShell**, das als Teil des Azure SDKs installiert ist.

    ![Screenshot der Oberfläche zum Hinzufügen/Entfernen von Programmen für Azure PowerShell](./media/azure-stack-install-visual-studio/image2.png) 

4. [Installieren von PowerShell für Azure Stack](azure-stack-powershell-install.md)

5. Starten Sie nach Abschluss der Installation das Betriebssystem neu.

## <a name="connect-to-azure-stack"></a>Herstellen einer Verbindung mit Azure Stack

1. Starten Sie Visual Studio.

2. Wählen Sie im Menü **Ansicht** die Option **Cloud-Explorer** aus.

3. Wählen Sie im neuen Bereich die Option **Konto hinzufügen** aus, und melden Sie sich mit Ihren Azure Active Directory-Anmeldeinformationen an.  
    ![Screenshot von Cloud-Explorer nach dem Anmelden und dem Herstellen einer Verbindung mit Azure Stack](./media/azure-stack-install-visual-studio/image6.png)

Nach der Anmeldung können Sie [Vorlagen bereitstellen](azure-stack-deploy-template-visual-studio.md) oder die verfügbaren Ressourcentypen und Ressourcengruppen durchsuchen, um Ihre eigenen Vorlagen zu erstellen.  

## <a name="next-steps"></a>Nächste Schritte

 - [Entwickeln von Vorlagen für Azure Stack](azure-stack-develop-templates.md)


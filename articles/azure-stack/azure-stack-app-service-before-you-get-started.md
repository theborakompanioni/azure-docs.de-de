---
title: Vor der Bereitstellung von App Service in Azure Stack | Microsoft-Dokumentation
description: "Vor der Bereitstellung von App Service in Azure Stack auszuführende Schritte"
services: azure-stack
documentationcenter: 
author: apwestgarth
manager: stefsch
editor: 
ms.assetid: 
ms.service: azure-stack
ms.workload: app-service
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 7/3/2017
ms.author: anwestg
ms.translationtype: HT
ms.sourcegitcommit: f5c887487ab74934cb65f9f3fa512baeb5dcaf2f
ms.openlocfilehash: 3cba11acc6279f24d0a47af8978610180724c0a2
ms.contentlocale: de-de
ms.lasthandoff: 09/15/2017

---
# <a name="before-you-get-started-with-app-service-on-azure-stack"></a>Vor den ersten Schritten mit App Service in Azure Stack

Für die Installation von Azure App Service in Azure Stack benötigen Sie einige Elemente:

- Eine abgeschlossene Bereitstellung des [Azure Stack Development Kits](azure-stack-run-powershell-script.md).
- Ausreichend Speicherplatz in Ihrem Azure Stack-System für eine kleine Bereitstellung von App Service in Azure Stack.  Hierfür benötigen Sie etwa 20 GB Speicherplatz.
- Ein Windows Server-VM-Image, das Sie beim Erstellen virtueller Computer für App Service in Azure Stack verwenden.
- [Ein Server, der SQL Server ausführt](#SQL-Server).

>[!NOTE] 
> *Alle* der folgenden Schritte werden auf dem Azure Stack-Hostcomputer ausgeführt.

Um einen Ressourcenanbieter bereitzustellen, müssen Sie PowerShell Integrated Scripting Environment (ISE) als Administrator ausführen. Daher müssen Sie Cookies und JavaScript in dem Internet Explorer-Profil zulassen, mit dem Sie sich bei Azure Active Directory anmelden.

## <a name="turn-off-internet-explorer-enhanced-security"></a>Deaktivieren der erweiterten Sicherheit von Internet Explorer

1.  Melden Sie sich als **AzureStack/administrator** beim Azure Stack Development Kit an, und öffnen Sie den **Server-Manager**.

2.  Deaktivieren Sie die **verstärkte Sicherheitskonfiguration für Internet Explorer** für Administratoren und Benutzer.

3.  Melden Sie sich als Administrator beim Azure Stack Development Kit an, und öffnen Sie den **Server-Manager**.

4.  Deaktivieren Sie die **verstärkte Sicherheitskonfiguration für Internet Explorer** für Administratoren und Benutzer.

## <a name="enable-cookies"></a>Aktivieren von Cookies

1.  Wählen Sie **Start** > **Alle Apps** > **Windows-Zubehör**. Klicken Sie mit der rechten Maustaste auf **Internet Explorer** > **Mehr** > **Als Administrator ausführen**.

2.  Wenn Sie dazu aufgefordert werden, wählen Sie **Use recommended security** (Empfohlene Sicherheit verwenden), und wählen Sie anschließend **OK**.

3.  Wählen Sie im Internet Explorer **Tools** (das Zahnradsymbol) > **Internetoptionen** > **Datenschutz** > **Erweitert**.

4.  Wählen Sie **Advanced** (Erweitert). Stellen Sie sicher, dass die beiden **Annehmen**-Kontrollkästchen aktiviert sind. Wählen Sie zweimal **OK**.

5.  Schließen Sie Internet Explorer, und starten Sie PowerShell ISE als Administrator erneut.

## <a name="install-powershell-for-azure-stack"></a>Installieren von PowerShell für Azure Stack

Führen Sie die unter [Install PowerShell](azure-stack-powershell-install.md) (Installieren von PowerShell) beschriebenen Schritte aus, um PowerShell für Azure Stack zu installieren.

## <a name="use-visual-studio-with-azure-stack"></a>Verwenden von Visual Studio mit Azure Stack

Führen Sie die unter [Install Visual Studio](azure-stack-install-visual-studio.md) (Installieren von Visual Studio) beschriebenen Schritte aus, um Visual Studio für Azure Stack zu installieren.

## <a name="add-a-windows-server-2016-vm-image-to-azure-stack"></a>Hinzufügen eines Windows Server 2016-VM-Images zu Azure Stack

Da App Service mehrere virtuelle Computer bereitstellt, ist ein Windows Server 2016-VM-Image in Azure Stack erforderlich. Führen Sie die unter [Add a default virtual machine image](azure-stack-add-default-image.md) (Hinzufügen eines Standard-VM-Images) beschriebenen Schritte aus, um ein VM-Image zu installieren.

## <a name="SQL-Server"></a>SQL Server

App Service in Azure Stack erfordert Zugriff auf eine SQL Server-Instanz, um zwei Datenbanken zum Ausführen des App Service-Ressourcenanbieters zu erstellen und zu hosten.  Wenn Sie eine SQL Server-VM in Azure Stack bereitstellen möchten, muss die SQL-Konnektivitätsebene auf **Öffentlich** festgelegt sein.  Sie können die zu verwendende SQL Server-Instanz auswählen, wenn Sie die Optionen in App Service im Azure Stack-Installer abschließen.

## <a name="next-steps"></a>Nächste Schritte

- [Installieren Sie den App Service-Ressourcenanbieter](azure-stack-app-service-deploy.md).

<!--Image references-->
[1]: ./media/azure-stack-app-service-before-you-get-started/PSGallery.png
[2]: ./media/azure-stack-app-service-before-you-get-started/WebPI_InstalledProducts.png


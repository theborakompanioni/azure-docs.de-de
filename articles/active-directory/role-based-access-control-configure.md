---
title: Rollenbasierte Zugriffssteuerung im klassischen Azure-Portal | Microsoft-Dokumentation
description: "Führen Sie die ersten Schritte der Zugriffsverwaltung mit der rollenbasierten Zugriffssteuerung im Azure-Portal aus. Verwenden Sie Rollenzuweisungen, um Ihren Ressourcen Berechtigungen zuzuweisen."
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
editor: 
ms.assetid: 8078f366-a2c4-4fbb-a44b-fc39fd89df81
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 02/17/2017
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: bb894c38de63d0eac4066eeabaed7ba791021cc4
ms.openlocfilehash: 05da6cd034a387b54eff0790996662223e4b8bab
ms.lasthandoff: 02/18/2017


---
# <a name="use-role-assignments-to-manage-access-to-your-azure-subscription-resources"></a>Verwenden von Rollenzuweisungen zum Verwalten Ihrer Azure-Abonnementressourcen
> [!div class="op_single_selector"]
> * [Verwalten des Zugriffs nach Benutzer oder Gruppe](role-based-access-control-manage-assignments.md)
> * [Verwalten des Zugriffs nach Ressource](role-based-access-control-configure.md)

Die rollenbasierte Access Control in Azure (RBAC) ermöglicht eine präzise Zugriffsverwaltung für Azure. Mit RBAC können Sie den Benutzern nur die Zugriffsrechte gewähren, die diese zum Ausführen ihrer Aufgaben benötigen. Dieser Artikel hilft Ihnen bei der Einrichtung von RBAC im Azure-Portal. Weitere Informationen dazu, wie RBAC Sie bei der Zugriffsverwaltung unterstützt, finden Sie unter [Get started with access management in the Azure portal](role-based-access-control-what-is.md)(Erste Schritte mit der Zugriffsverwaltung im Azure-Portal).

## <a name="view-access"></a>Zugriff anzeigen
Im Hauptblatt im [Azure-Portal](https://portal.azure.com)können Sie anzeigen, wer Zugriff auf eine Ressource, Ressourcengruppe oder ein Abonnement hat. Wir möchten beispielsweise sehen, wer Zugriff auf eine unserer Ressourcengruppen hat:

1. Wählen Sie in der Navigationsleiste auf der linken Seite **Ressourcengruppe** aus.  
    ![Ressourcengruppen – Symbole](./media/role-based-access-control-configure/resourcegroups_icon.png)
2. Wählen Sie im Blatt **Ressourcengruppen** den Namen der Ressourcengruppe aus.
3. Wählen Sie im linken Menü **Zugriffssteuerung (IAM)**.  
4. Auf dem Blatt für die Zugriffssteuerung werden alle Benutzer, Gruppen und Anwendungen aufgeführt, denen Zugriff auf die Ressourcengruppe gewährt wurde.  
   
    ![Blatt „Benutzer“ – geerbter und zugewiesener Zugriff – Screenshot](./media/role-based-access-control-configure/view-access.png)

Beachten Sie, dass einigen Benutzern der Zugriff **zugewiesen** wurde, während andere ihn **geerbt** haben. Der Zugriff wird entweder speziell der Ressourcengruppe zugewiesen oder von einer Zuweisung des übergeordneten Abonnements geerbt.

> [!NOTE]
> Klassische Administratoren und Co-Admins für Abonnements werden im neuen RBAC-Modell als Besitzer des Abonnements betrachtet.

## <a name="add-access"></a>Zugriff hinzufügen
Sie gewähren Zugriff aus der Ressource, der Ressourcengruppe oder dem Abonnement, die bzw. das als Bereich der Rollenzuweisung gilt.

1. Wählen Sie auf dem Blatt für die Zugriffssteuerung die Option **Hinzufügen**.  
2. Wählen Sie auf dem Blatt **Rolle auswählen** auf die Rolle, die Sie zuweisen möchten.
3. Wählen Sie den Benutzer, die Gruppe oder die Anwendung als das Element in Ihrem Verzeichnis aus, für das Sie Zugriff gewähren möchten. Sie können das Verzeichnis mit Anzeigenamen, E-Mail-Adressen und Objektbezeichnern durchsuchen.  
   
    ![Blatt „Benutzer hinzufügen“ – Durchsuchen – Screenshot](./media/role-based-access-control-configure/grant-access2.png)
4. Wählen Sie **OK** , um die Zuweisung zu erstellen. Das Popupfenster **Der Benutzer wird hinzugefügt** zeigt den Fortschritt an.  
    ![Statusanzeige „Der Benutzer wird hinzugefügt“ – Screenshot](./media/role-based-access-control-configure/addinguser_popup.png)

Wenn die Rollenzuweisung hinzugefügt wurde, erscheint sie auf dem Blatt **Benutzer** .

## <a name="remove-access"></a>Zugriff entfernen
1. Verwenden Sie die Kontrollkästchen auf dem Blatt „Zugriffssteuerung“, um mindestens eine Rollenzuweisung auszuwählen.
2. Wählen Sie **Entfernen**.  
3. Ein Feld mit der Aufforderung zum Bestätigen der Aktion wird angezeigt. Wählen Sie **Ja**, um die Rollenzuweisungen zu entfernen.

Geerbte Zuweisungen können nicht entfernt werden. Wenn Sie eine geerbte Zuweisung entfernen möchten, müssen Sie dies für den Bereich durchführen, in dem die Rollenzuweisung erstellt wurde. Die Spalte **Bereich** neben **Geerbt** enthält einen Link, mit dem Sie zu den Ressourcen gelangen, denen diese Rolle zugewiesen wurde. Sie können die Rollenzuweisung entfernen, indem Sie zu der dort aufgelisteten Ressource gehen.

![Blatt „Benutzer“ – Schaltfläche „Entfernen“ bei geerbtem Zugriff deaktiviert – Screenshot](./media/role-based-access-control-configure/remove-access2.png)

## <a name="other-tools-to-manage-access"></a>Andere Tools zum Verwalten des Zugriffs
Sie können auch mit Azure RBAC-Befehlen in anderen Tools als dem Azure-Portal Rollen zuweisen und den Zugriff verwalten.  Folgen Sie den Links, um weitere Informationen zu den Voraussetzungen und Hilfe bei den ersten Schritten mit Azure RBAC-Befehlen zu erhalten.

* [Azure PowerShell](role-based-access-control-manage-access-powershell.md)
* [Azure-Befehlszeilenschnittstelle](role-based-access-control-manage-access-azure-cli.md)
* [REST-API](role-based-access-control-manage-access-rest.md)

## <a name="next-steps"></a>Nächste Schritte
* [Erstellen eines Verlaufsberichts zu Zugriffsänderungen](role-based-access-control-access-change-history-report.md)
* Weitere Informationen finden Sie unter [Integrierte RBAC-Rollen in Azure](role-based-access-built-in-roles.md)
* Definieren Sie Ihre eigenen [benutzerdefinierten Rollen in Azure RBAC](role-based-access-control-custom-roles.md)



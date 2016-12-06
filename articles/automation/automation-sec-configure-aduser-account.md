---
title: Konfigurieren eines Azure AD-Benutzerkontos | Microsoft Docs
description: "In diesem Artikel wird beschrieben, wie Sie Azure AD-Benutzerkonto-Anmeldeinformationen für Runbooks in Azure Automation zum Authentifizieren konfigurieren."
services: automation
documentationcenter: 
author: MGoedtel
manager: jwhit
editor: tysonn
keywords: Azure Active Directory-Benutzer, Azure Service Management, Azure AD-Benutzerkonto
ms.assetid: fcfe266d-b22e-4dfb-8272-adcab09fc0cf
ms.service: automation
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/14/2016
ms.author: magoedte
translationtype: Human Translation
ms.sourcegitcommit: 0078d544d5c30e31874d75fece62ca560d2aa2d6
ms.openlocfilehash: 0544d4df8f50db2182375aec8ec2a0a9d47ed644


---
# <a name="authenticate-runbooks-with-azure-service-management-and-resource-manager"></a>Authentifizieren von Runbooks mit Azure-Dienstverwaltung und Resource Manager
In diesem Artikel werden die Schritte beschrieben, die Sie zum Konfigurieren eines Azure AD-Benutzerkontos für Azure Automation-Runbooks ausführen müssen, die für Ressourcen der Azure-Dienstverwaltung oder von Azure Resource Manager ausgeführt werden.  Diese Authentifizierungsidentität wird für Azure Resource Manager-basierte Runbooks zwar weiterhin unterstützt, aber die empfohlene Methode ist die Verwendung des neuen ausführenden Kontos von Azure.       

## <a name="create-a-new-azure-active-directory-user"></a>Erstellen eines neuen Azure Active Directory-Benutzers
1. Melden Sie sich im klassischen Azure-Portal als Dienstadministrator für das Azure-Abonnement an, das Sie verwalten möchten.
2. Wählen Sie **Active Directory**und dann den Namen des Verzeichnisses Ihrer Organisation.
3. Wählen Sie die Registerkarte **Benutzer** und dann im Befehlsbereich die Option **Benutzer hinzufügen**.
4. Wählen Sie auf der Seite **Informationen über diesen Benutzer** unter **Art des Benutzers** die Option **Neuer Benutzer in Ihrem Unternehmen**.
5. Geben Sie einen Benutzernamen ein.  
6. Wählen Sie den Verzeichnisnamen aus, der Ihrem Azure-Abonnement auf der Active Directory-Seite zugeordnet ist.
7. Geben Sie auf der Seite **Benutzerprofil** den Vornamen und Nachnamen, einen Anzeigenamen sowie über die Liste **Rollen** einen Benutzer an.  Wählen Sie nicht die Option **Multi-Factor Authentication aktivieren**aus.
8. Notieren Sie sich den vollständigen Namen des Benutzers und das vorläufige Kennwort.
9. Wählen Sie **Einstellungen > Administratoren > Hinzufügen**.
10. Geben Sie den vollständigen Namen des Benutzers ein, den Sie erstellt haben.
11. Wählen Sie das Abonnement aus, das der Benutzer verwalten soll.
12. Melden Sie sich bei Azure ab, und melden Sie sich anschließend mit den soeben erstellten Anmeldeinformationen wieder an. Sie werden aufgefordert, das Kennwort des Benutzers zu ändern.

## <a name="create-an-automation-account-in-azure-classic-portal"></a>Erstellen eines Automation-Kontos im klassischen Azure-Portal
In diesem Abschnitt führen Sie die folgenden Schritte aus, um ein neues Azure Automation-Konto im Azure-Portal zu erstellen, das für Ihre Runbooks zum Verwalten von Ressourcen im Modus „Azure Service Management“ und „Azure Resource Manager“ verwendet wird.  

> [!NOTE]
> Mit dem klassischen Azure-Portal erstellte Automation-Konten können sowohl mit dem klassischen Azure-Portal als auch mit dem Azure-Portal und beiden Cmdlet-Sätzen verwaltet werden. Nach dem Erstellen des Kontos macht es keinen Unterschied, wie Sie Ressourcen innerhalb des Kontos erstellen und verwalten. Wenn Sie planen, auch weiterhin das klassische Azure-Portal zu verwenden, sollten Sie dieses anstelle des Azure-Portals für das Erstellen von Automation-Konten verwenden.
> 
> 

1. Melden Sie sich im klassischen Azure-Portal als Dienstadministrator für das Azure-Abonnement an, das Sie verwalten möchten.
2. Wählen Sie **Automation**.
3. Wählen Sie auf der Seite **Automation** die Option **Automation-Konto erstellen**.
4. Geben Sie im Feld **Automation-Konto erstellen** einen Namen für das neue Automation-Konto ein, und wählen Sie in der Dropdownliste eine **Region** aus.  
5. Klicken Sie auf **OK** , um die Einstellungen zu übernehmen und das Konto zu erstellen.
6. Nach der Erstellung wird es auf der Seite **Automation** aufgeführt.
7. Wenn Sie auf das Konto klicken, gelangen Sie auf die Dashboardseite.  
8. Wählen Sie auf der Seite mit dem Automation-Dashboard die Option **Ressourcen**.
9. Wählen Sie auf der Seite **Ressourcen** unten die Option **Einstellungen hinzufügen**.
10. Wählen Sie auf der Seite **Einstellungen hinzufügen** die Option **Anmeldeinformationen hinzufügen**.
11. Wählen Sie auf der Seite **Anmeldeinformationen definieren** in der Dropdownliste **Typ der Anmeldeinformationen** die Option **Windows PowerShell-Anmeldeinformationen**, und geben Sie einen Namen für die Anmeldeinformationen an.
12. Geben Sie auf der folgenden Seite **Anmeldeinformationen definieren** im Feld **Benutzername** den Benutzernamen des zuvor erstellten AD-Benutzerkontos und in den Feldern **Kennwort** und **Kennwort bestätigen** das Kennwort ein. Klicken Sie zum Speichern der Änderungen auf **OK** .

## <a name="create-an-automation-account-in-the-azure-portal"></a>Erstellen eines Automation-Kontos im Azure-Portal
In diesem Abschnitt führen Sie die folgenden Schritte aus, um ein neues Azure Automation-Konto im Azure-Portal zu erstellen, das für Ihre Runbooks zum Verwalten von Ressourcen im Azure Resource Manager-Modus verwendet wird.  

1. Melden Sie sich im Azure-Portal als Dienstadministrator für das Azure-Abonnement an, das Sie verwalten möchten.
2. Wählen Sie die Option **Automation-Konten**.
3. Klicken Sie auf dem Blatt „Automation-Konten“ auf **Hinzufügen**.<br>![Automation-Konto hinzufügen](media/automation-sec-configure-azure-runas-account/add-automation-acct-properties.png)
4. Geben Sie auf dem Blatt **Automation-Konto hinzufügen** im Feld **Name** einen Namen für das neue Automation-Konto ein.
5. Wenn Sie über mehrere Abonnements verfügen, geben Sie das Abonnement für das neue Konto sowie eine neue oder vorhandene **Ressourcengruppe** und den **Standort** eines Azure-Rechenzentrums an.
6. Wählen Sie für die Option **Ausführendes Azure-Konto erstellen** den Wert **Nein** aus, und klicken Sie auf die Schaltfläche **Erstellen**.  
   
   > [!NOTE]
   > Wenn Sie **Nein** auswählen und das ausführende Konto nicht erstellen, wird auf dem Blatt **Automation-Konto hinzufügen** eine Warnmeldung angezeigt.  Das Konto wird zwar erstellt und der Rolle **Mitwirkender** des Abonnements zugewiesen, es verfügt aber über keine entsprechende Authentifizierungsidentität in Ihrem Abonnementverzeichnisdienst und somit auch nicht über Zugriffsressourcen in Ihrem Abonnement.  So wird verhindert, dass Runbooks, die auf dieses Konto verweisen, sich authentifizieren und Aufgaben für Azure Resource Manager-Ressourcen durchführen können.
   > 
   > 
   
    ![Automation-Konto hinzufügen – Warnung](media/automation-sec-configure-azure-runas-account/add-automation-acct-properties-error.png)
7. Während das Automation-Konto in Azure erstellt wird, können Sie den Status unter **Benachrichtigungen** im Menü nachverfolgen.

Nachdem die Erstellung der Anmeldeinformationen abgeschlossen ist, müssen Sie ein Anmeldeinformationsobjekt erstellen, um das Automation-Konto dem zuvor erstellten AD-Benutzerkonto zuzuordnen.  Beachten Sie, dass wir das Automation-Konto bisher nur erstellt und noch keiner Authentifizierungsidentität zugeordnet haben.  Führen Sie die Schritte im Artikel [Anmeldeinformationsobjekte in Azure Automation](automation-credentials.md#creating-a-new-credential-asset) aus, und geben Sie den Wert für **Benutzername** im Format **Domäne\Benutzer** ein.

## <a name="use-the-credential-in-a-runbook"></a>Verwenden der Anmeldeinformationen in einem Runbook
Sie können die Anmeldeinformationen in einem Runbook mithilfe der Aktivität [Get-AutomationPSCredential](http://msdn.microsoft.com/library/dn940015.aspx) abrufen und dann diese Informationen verwenden, um mit [Add-AzureAccount](http://msdn.microsoft.com/library/azure/dn722528.aspx) eine Verbindung mit Ihrem Azure-Abonnement herzustellen. Wenn die Anmeldeinformationen mit einem Administrator für mehrere Azure-Abonnements verknüpft sind, sollten Sie mithilfe von [Select-AzureSubscription](http://msdn.microsoft.com/library/dn495203.aspx) das richtige Abonnement angeben. Dies wird im nachstehenden Windows PowerShell-Beispiel gezeigt, das typischerweise im oberen Bereich der meisten Azure Automation-Runbooks angezeigt wird.

    $cred = Get-AutomationPSCredential –Name "myuseraccount.onmicrosoft.com"
    Add-AzureAccount –Credential $cred
    Select-AzureSubscription –SubscriptionName "My Subscription"

Diese Zeilen sollten nach jedem [Prüfpunkt](http://technet.microsoft.com/library/dn469257.aspx#bk_Checkpoints) wiederholt werden. Wenn das Runbook angehalten und später von einem anderen Benutzer fortgesetzt wird, muss die Authentifizierung erneut durchgeführt werden.

## <a name="next-steps"></a>Nächste Schritte
* Informieren Sie sich im Artikel [Azure Automation-Runbooktypen](automation-runbook-types.md) über die unterschiedlichen Runbooktypen und die Schritte zum Erstellen Ihrer eigenen Runbooks.




<!--HONumber=Nov16_HO3-->



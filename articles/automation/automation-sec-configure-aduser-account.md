<properties
   pageTitle="Konfigurieren eines Azure AD-Benutzerkontos | Microsoft Azure"
   description="In diesem Artikel wird beschrieben, wie Sie Azure AD-Benutzerkonto-Anmeldeinformationen für Runbooks in Azure Automation zum Authentifizieren für ARM und ASM konfigurieren."
   services="automation"
   documentationCenter=""
   authors="MGoedtel"
   manager="jwhit"
   editor="tysonn" />
<tags
   ms.service="automation"
   ms.devlang="na"
   ms.topic="get-started-article"
   ms.tgt_pltfrm="na"
   ms.workload="infrastructure-services"
   ms.date="04/08/2016"
   ms.author="magoedte" />

# Authentifizieren von Runbooks mit Azure-Dienstverwaltung und Resource Manager

In diesem Artikel werden die Schritte beschrieben, die Sie zum Konfigurieren eines Azure AD-Benutzerkontos für Azure Automation-Runbooks ausführen müssen, die für Ressourcen der Azure-Dienstverwaltung (Azure Service Management, ASM) oder von Azure Resource Manager (ARM) ausgeführt werden. Diese Authentifizierungsidentität wird für ARM-basierte Runbooks zwar weiterhin unterstützt, aber die empfohlene Methode ist die Verwendung des neuen ausführenden Kontos von Azure.

## Erstellen eines neuen Azure Active Directory-Benutzers

1. Melden Sie sich im klassischen Azure-Portal als Dienstadministrator für das Azure-Abonnement an, das Sie verwalten möchten.
2. Wählen Sie **Active Directory** und dann den Namen des Verzeichnisses Ihrer Organisation.
3. Wählen Sie die Registerkarte **Benutzer** und dann im Befehlsbereich die Option **Benutzer hinzufügen**.
4. Wählen Sie auf der Seite **Informationen über diesen Benutzer** unter **Art des Benutzers** die Option **Neuer Benutzer in Ihrem Unternehmen**.
5. Geben Sie einen Benutzernamen ein.  
6. Wählen Sie den Verzeichnisnamen aus, der Ihrem Azure-Abonnement auf der Active Directory-Seite zugeordnet ist.
7. Geben Sie auf der Seite **Benutzerprofil** den Vornamen und Nachnamen, einen benutzerfreundlichen Namen und über die Liste **Rollen** einen Benutzer an. Wählen Sie nicht die Option **Multi-Factor Authentication aktivieren** aus.
8. Notieren Sie sich den vollständigen Namen des Benutzers und das vorläufige Kennwort.
9. Wählen Sie **Einstellungen > Administratoren > Hinzufügen**.
10. Geben Sie den vollständigen Namen des Benutzers ein, den Sie erstellt haben.
11. Wählen Sie das Abonnement aus, das der Benutzer verwalten soll.
12. Melden Sie sich bei Azure ab, und melden Sie sich anschließend mit den soeben erstellten Anmeldeinformationen wieder an. Sie werden aufgefordert, das Kennwort des Benutzers zu ändern.


## Erstellen eines Automation-Kontos im klassischen Azure-Portal
In diesem Abschnitt führen Sie die folgenden Schritte aus, um ein neues Azure Automation-Konto im Azure-Portal zu erstellen, das für Ihre Runbooks zum Verwalten von Ressourcen im ASM- und ARM-Modus verwendet wird.

>[AZURE.NOTE] Mit dem klassischen Azure-Portal erstellte Automation-Konten können sowohl mit dem klassischen Azure-Portal als auch mit dem Azure-Portal und beiden Cmdlet-Sätzen verwaltet werden. Nach dem Erstellen des Kontos macht es keinen Unterschied, wie Sie Ressourcen innerhalb des Kontos erstellen und verwalten. Wenn Sie planen, auch weiterhin das klassische Azure-Portal zu verwenden, sollten Sie dieses anstelle des Azure-Portals für das Erstellen von Automation-Konten verwenden.


1. Melden Sie sich im klassischen Azure-Portal als Dienstadministrator für das Azure-Abonnement an, das Sie verwalten möchten.
2. Wählen Sie **Automation**.
3. Wählen Sie auf der Seite **Automation** die Option **Automation-Konto erstellen**.
4. Geben Sie im Feld **Automation-Konto erstellen** einen Namen für das neue Automation-Konto ein, und wählen Sie in der Dropdownliste eine **Region** aus.  
5. Klicken Sie auf **OK**, um die Einstellungen zu übernehmen und das Konto zu erstellen.
6. Nach der Erstellung wird es auf der Seite **Automation** aufgeführt.
7. Wenn Sie auf das Konto klicken, gelangen Sie auf die Dashboardseite.  
8. Wählen Sie auf der Seite mit dem Automation-Dashboard die Option **Ressourcen**.
9. Wählen Sie auf der Seite **Ressourcen** unten auf der Seite die Option **Einstellungen hinzufügen**.
10. Wählen Sie auf der Seite **Einstellungen hinzufügen** die Option **Anmeldeinformationen hinzufügen**.
11. Wählen Sie auf der Seite **Anmeldeinformationen hinzufügen** in der Dropdownliste **Typ der Anmeldeinformationen** die Option **Windows PowerShell-Anmeldeinformationen**, und geben Sie einen Namen für die Anmeldeinformationen an.
12. Geben Sie auf der folgenden Seite **Anmeldeinformationen hinzufügen** den Benutzernamen des zuvor erstellten AD-Benutzerkontos im Feld **Benutzername** und das Kennwort in den Feldern **Kennwort** und **Kennwort bestätigen** ein. Klicken Sie zum Speichern der Änderungen auf **OK**.

## Erstellen eines Automation-Kontos im Azure-Portal

In diesem Abschnitt führen Sie die folgenden Schritte aus, um ein neues Azure Automation-Konto im Azure-Portal zu erstellen, das für Ihre Runbooks zum Verwalten von Ressourcen im ARM-Modus verwendet wird.

1. Melden Sie sich im Azure-Portal als Dienstadministrator für das Azure-Abonnement an, das Sie verwalten möchten.
2. Wählen Sie die Option **Automation-Konten**.
3. Klicken Sie im Blatt „Automation-Konten“ auf **Hinzufügen**.<br>![Automation-Konto hinzufügen](media/automation-sec-configure-azure-runas-account/add-automation-acct-properties.png)
2. Geben Sie im Blatt **Automation-Konto hinzufügen** im Feld **Name** einen Namen für das neue Automation-Konto ein.
5. Wenn Sie über mehrere Abonnements verfügen, geben Sie das für das neue Konto geltende Abonnement sowie eine neue oder vorhandene **Ressourcengruppe** und den **Speicherort** eines Azure-Rechenzentrums an.
3. Wählen Sie **Nein** für die Option **Ausführendes Azure-Konto erstellen**, und klicken Sie auf die Schaltfläche **Erstellen**.  

    >[AZURE.NOTE] Wenn Sie sich dafür entscheiden, das ausführende Konto nicht zu erstellen, indem Sie die Option **Nein** wählen, wird im Blatt **Automation-Konto hinzufügen** eine Warnmeldung angezeigt. Das Konto wird zwar erstellt und der Rolle **Mitwirkender** des Abonnements zugewiesen, aber es verfügt nicht über eine entsprechende Authentifizierungsidentität in Ihrem Abonnementverzeichnisdienst und somit auch nicht über Zugriffsressourcen in Ihrem Abonnement. So wird verhindert, dass Runbooks, die auf dieses Konto verweisen, sich authentifizieren und Aufgaben für ARM-Ressourcen durchführen können.

    ![Automation-Konto hinzufügen – Warnung](media/automation-sec-configure-azure-runas-account/add-automation-acct-properties-error.png)

4. Während das Automation-Konto in Azure erstellt wird, können Sie den Status unter **Benachrichtigungen** im Menü nachverfolgen.

Nachdem die Erstellung der Anmeldeinformationen abgeschlossen ist, müssen Sie ein Anmeldeinformationsobjekt erstellen, um das Automation-Konto dem zuvor erstellten AD-Benutzerkonto zuzuordnen. Beachten Sie, dass wir das Automation-Konto bisher nur erstellt und noch keiner Authentifizierungsidentität zugeordnet haben. Führen Sie die Schritte im Artikel[Anmeldeinformationsobjekte in Azure Automation](../automation/automation-credentials.md#creating-a-new-credential) aus, und geben Sie den Wert für **Benutzername** im Format **Domäne\\Benutzer** ein.

## Verwenden der Anmeldeinformationen in einem Runbook

Sie können die Anmeldeinformationen in einem Runbook mithilfe der Aktivität [Get-AutomationPSCredential](http://msdn.microsoft.com/library/dn940015.aspx) abrufen und dann mit [Add-AzureAccount](http://msdn.microsoft.com/library/azure/dn722528.aspx) eine Verbindung mit Ihrem Azure-Abonnement herstellen. Wenn die Anmeldeinformationen mit einem Administrator für mehrere Azure-Abonnements verknüpft sind, sollten Sie mithilfe von [Select-AzureSubscription](http://msdn.microsoft.com/library/dn495203.aspx) das richtige Abonnement angeben. Dies wird im nachstehenden Windows PowerShell-Beispiel gezeigt, das typischerweise im oberen Bereich der meisten Azure Automation-Runbooks angezeigt wird.

    $cred = Get-AutomationPSCredential –Name "myuseraccount.onmicrosoft.com"
	Add-AzureAccount –Credential $cred
	Select-AzureSubscription –SubscriptionName "My Subscription"

Diese Zeilen sollten nach jedem [Prüfpunkt](http://technet.microsoft.com/library/dn469257.aspx#bk_Checkpoints) wiederholt werden. Wenn das Runbook angehalten und später von einem anderen Benutzer fortgesetzt wird, muss die Authentifizierung erneut durchgeführt werden.

## Nächste Schritte
* Informieren Sie sich über die unterschiedlichen Runbooktypen und die Schritte zum Erstellen Ihrer eigenen Runbooks im Artikel [Azure Automation-Runbooktypen](../automation/automation-runbook-types.md).

<!---HONumber=AcomDC_0413_2016-->
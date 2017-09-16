---
title: "Häufig gestellte Fragen zu Azure DevTest Labs | Microsoft Docs"
description: "Hier finden Sie Antworten auf häufig gestellte Fragen zu Azure DevTest Labs."
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: afe83109-b89f-4f18-bddd-b8b4a30f11b4
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/22/2017
ms.author: tarcher
ms.translationtype: HT
ms.sourcegitcommit: 3eb68cba15e89c455d7d33be1ec0bf596df5f3b7
ms.openlocfilehash: 17ad81945deef8d4803898cfe4fc9ddbd88a2a54
ms.contentlocale: de-de
ms.lasthandoff: 09/01/2017

---
# <a name="azure-devtest-labs-faq"></a>Häufig gestellte Fragen zu Azure DevTest Labs
Im Folgenden werden einige der am häufigsten gestellten Fragen zu Azure DevTest Labs beantwortet.

**Allgemein**
## <a name="what-if-my-question-isnt-answered-here"></a>Was kann ich tun, wenn meine Frage hier nicht beantwortet wird?
Wenn Ihre Frage hier nicht aufgeführt wird, informieren Sie uns, damit wir Ihnen helfen können, eine Antwort zu finden.

* Veröffentlichen Sie eine Frage am Ende dieses Artikels zu häufig gestellten Fragen. Tauschen Sie sich mit dem Azure Cache-Team und anderen Communitymitgliedern über diesen Artikel aus.
* Um eine größere Zielgruppe zu erreichen, veröffentlichen Sie eine Frage im [Azure DevTest Labs MSDN-Forum](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureDevTestLabs). Tauschen Sie sich mit dem Azure DevTest Labs-Team und anderen Communitymitgliedern aus.
* Um Funktionsanforderungen einzureichen, übermitteln Sie Ihre Anfragen und Ideen an [Azure DevTest Labs User Voice](https://feedback.azure.com/forums/320373-azure-devtest-labs).

## <a name="why-should-i-use-azure-devtest-labs"></a>Warum sollte ich Azure DevTest Labs verwenden?
Mit Azure DevTest Labs kann Ihr Team Zeit und Geld sparen. Entwickler können eigene Umgebungen auf unterschiedlichen Grundlagen erstellen. Zudem können sie mithilfe von Artefakten schnell Anwendungen bereitstellen und konfigurieren. Durch benutzerdefinierte Images und Formeln können Sie virtuelle Computer (VMs) als Vorlagen speichern und diese problemlos innerhalb des Teams reproduzieren. DevTest Labs bieten mehrere konfigurierbare Richtlinien, mit denen Labadministratoren die Vergeudung von Ressourcen reduzieren und die Umgebungen eines Teams verwalten können. Diese Richtlinien beinhalten u.a. das automatische Herunterfahren, Kostenschwellenwerte, eine maximale Anzahl von VMs pro Benutzer und eine maximale VM-Größe. Eine ausführlichere Erläuterung von DevTest Labs finden Sie in der [Übersicht](devtest-lab-overview.md) oder im [Einführungsvideo](https://channel9.msdn.com/Blogs/Azure/what-is-azure-devtest-labs).

## <a name="what-does-worry-free-self-service-mean"></a>Was bedeutet „Self-Service-Bereitstellung für sorgenfreies Arbeiten“?
„Self-Service-Bereitstellung für sorgenfreies Arbeiten“ bedeutet, dass Entwickler und Tester ihrer eigenen Umgebungen nach Bedarf erstellen. Administratoren können sich dabei darauf verlassen, dass durch DevTest Labs die Verschwendung von Ressourcen reduziert werden kann und die Kosten unter Kontrolle behalten werden können. Administratoren können die zulässigen VM-Größen und die maximale Anzahl von VMs angeben und festlegen, wann VMs gestartet und beendet werden. DevTest Labs vereinfacht auch die Überwachung der Kosten und das Festlegen von Warnungen, sodass sie stets den Überblick darüber behalten, wie Ressourcen verwendet werden.

## <a name="how-can-i-use-devtest-labs"></a>Wie kann ich DevTest Labs verwenden?
DevTest Labs eignet sich, wenn Sie Entwicklungs- oder Testumgebungen benötigen und diese schnell reproduzieren oder mit kostensparenden Richtlinien verwalten möchten.

Unsere Kunden verwenden DevTest Labs beispielsweise für folgende Szenarien:

* Zentrale Verwaltung von Entwicklungs- und Testumgebungen. Senken Sie mithilfe von Richtlinien die Kosten, und erstellen Sie benutzerdefinierte Images, um Builds innerhalb des Teams freizugeben.
* Entwicklung von Anwendungen mit benutzerdefinierten Images, um den Datenträgerzustand in den Entwicklungsphasen zu speichern
* Nachverfolgung der Kosten im Verhältnis zum Status
* Erstellung von Massentestumgebungen für Qualitätssicherungstests
* Verwendung von Artefakten und Formeln zum einfachen Konfigurieren und Reproduzieren von Anwendungen in verschiedenen Umgebungen
* Verteilung von VMs für Hackathons (gemeinschaftliche Entwicklungs- oder Testarbeiten) und einfache Aufhebung der Bereitstellung nach Beendigung des Ereignisses

## <a name="how-am-i-billed-for-devtest-labs"></a>Wie wird die Nutzung von DevTest Labs abgerechnet?
DevTest Labs ist ein kostenloser Dienst. Für das Erstellen von Labs sowie das Konfigurieren von Richtlinien, Vorlagen und Artefakten in DevTest Labs fallen keinerlei Kosten an. Sie bezahlen nur für die Azure-Ressourcen, die Sie in den Labs verwenden, beispielsweise VMs, Speicherkonten und virtuelle Netzwerke. Unter [Preise von Azure DevTest Labs](https://azure.microsoft.com/pricing/details/devtest-lab/) erfahren Sie mehr über die Kosten von Labressourcen.


**Sicherheit**
## <a name="what-are-the-different-security-levels-in-devtest-labs"></a>Wie lauten die verschiedenen Sicherheitsstufen in DevTest Labs?
Der Sicherheitszugriff wird durch die [rollenbasierte Zugriffssteuerung (Role-Based Access Control, RBAC)](../active-directory/role-based-access-built-in-roles.md) bestimmt. Die Funktionsweise des Zugriffs ist leichter zu verstehen, wenn Sie die Unterschiede zwischen Berechtigungen, Rollen und Bereichen in der RBAC kennen.

* **Berechtigung**: Eine Berechtigung ermöglicht einen definierten Zugriff auf eine bestimmte Aktion. Zum Beispiel kann eine Berechtigung den Lesezugriff auf alle VMs ermöglichen.
* **Rolle**: Eine Rolle ist ein Satz von Berechtigungen, die gruppiert und einem Benutzer zugewiesen werden können. Zum Beispiel verfügt die Rolle „Abonnementbesitzer“ über Zugriff auf alle Ressourcen in einem Abonnement.
* **Bereich**: Ein Bereich ist eine Stufe innerhalb der Hierarchie einer Azure-Ressource. Zum Beispiel kann eine Ressourcengruppe, ein einzelnes Lab oder das gesamte Abonnement ein Bereich sein.

Innerhalb des Bereichs von DevTest Labs gibt es zwei Arten von Rollen zum Definieren von Benutzerberechtigungen:

* **Labbesitzer**: Ein Labbesitzer verfügt über Zugriff auf alle Ressourcen im Lab. Ein Labbesitzer kann u.a. Richtlinien sowie das virtuelle Netzwerk ändern und verfügt über Lese- und Schreibberechtigungen für alle VMs.
* **Labbenutzer**: Ein Labbenutzer kann alle Labressourcen wie VMs, Richtlinien und virtuelle Netzwerke einsehen. Allerdings kann ein Labbenutzer keine Richtlinien oder VMs ändern, die von anderen Benutzern erstellt wurden. 

Sie können auch benutzerdefinierte Rollen in DevTest Labs erstellen. Informationen zum Erstellen benutzerdefinierter Rollen in DevTest Labs finden Sie im Artikel [Gewähren von Benutzerberechtigungen für bestimmte Labrichtlinien](devtest-lab-grant-user-permissions-to-specific-lab-policies.md).

Da Bereiche hierarchisch sind, gelten die Berechtigungen, die einem Benutzer für einen bestimmten Bereich gewährt werden, automatisch für alle darin enthaltenen Bereiche auf niedrigerer Ebene. Ist einem Benutzer beispielsweise die Rolle „Abonnementbesitzer“ zugewiesen, hat der Benutzer Zugriff auf alle Ressourcen in einem Abonnement. Zu diesen Ressourcen gehören alle VMs, alle virtuellen Netzwerke und alle Labs. Ein Abonnementbesitzer erbt automatisch die Rolle des Labbesitzers. Umgekehrt ist dies jedoch nicht der Fall. Ein Labbesitzer hat Zugriff auf ein Lab, d.h. auf einen Bereich, der sich innerhalb der Hierarchie auf einer niedrigeren Ebene befindet als das Abonnement. Daher kann ein Labbesitzer keine VMs, virtuellen Netzwerke oder anderen Ressourcen außerhalb des Labs sehen.

## <a name="how-do-i-create-a-role-to-allow-users-to-perform-a-specific-task"></a>Wie erstelle ich eine bestimmte Rolle, um Benutzern das Ausführen einer einzelnen Aufgabe zu ermöglichen?
Einen umfassenden Artikel zum Erstellen von benutzerdefinierten Rollen und Zuweisen von Berechtigungen zu einer Rolle finden Sie unter [Erteilen von Benutzerberechtigungen zu bestimmten Labrichtlinien](devtest-lab-grant-user-permissions-to-specific-lab-policies.md). Das folgende Beispielskript erstellt die Rolle *DevTest Labs Advanced User*, die über die Berechtigung zum Starten und Beenden alle VMs im Lab verfügt:

    $policyRoleDef = Get-AzureRmRoleDefinition "DevTest Labs User"
    $policyRoleDef.Actions.Remove('Microsoft.DevTestLab/Environments/*')
    $policyRoleDef.Id = $null
    $policyRoleDef.Name = "DevTest Labs Advanced User"
    $policyRoleDef.IsCustom = $true
    $policyRoleDef.AssignableScopes.Clear()
    $policyRoleDef.AssignableScopes.Add("subscriptions/<subscription Id>")
    $policyRoleDef.Actions.Add("Microsoft.DevTestLab/labs/virtualMachines/Start/action")
    $policyRoleDef.Actions.Add("Microsoft.DevTestLab/labs/virtualMachines/Stop/action")
    $policyRoleDef = New-AzureRmRoleDefinition -Role $policyRoleDef  


**CI/CD-Integration und Automatisierung**
## <a name="does-devtest-labs-integrate-with-my-cicd-toolchain"></a>Lässt sich DevTest Labs in meine CI/CD-Toolkette integrieren?
Wenn Sie Visual Studio Team Services verwenden, können Sie Ihre Releasepipeline in DevTest Labs mithilfe einer [DevTest Labs-Erweiterung für Aufgaben](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks) automatisieren. Mit dieser Erweiterung können Sie u.a. folgende Aufgaben durchführen:

* Automatisches Erstellen und Bereitstellen einer VM. Sie können die VM auch durch den Azure-Dateikopiervorgang oder durch PowerShell Team Services-Aufgaben mit dem neuesten Build konfigurieren.
* Automatisches Erfassen des Zustands einer VM nach dem Testen, um zur weiteren Untersuchung einen Fehler auf derselben VM zu reproduzieren
* Löschen der VM am Ende der Releasepipeline, wenn sie nicht mehr benötigt wird

Die folgenden Blogbeiträge enthalten Anleitungen und Informationen zur Verwendung der Team Services-Erweiterung:

* [DevTest Labs und die Visual Studio Team Services-Erweiterung](https://blogs.msdn.microsoft.com/devtestlab/2016/06/15/azure-devtest-labs-vsts-extension/)
* [Bereitstellen einer neuen VM in einem vorhandenen DevTest Labs-Lab über Team Services](http://www.visualstudiogeeks.com/blog/DevOps/Deploy-New-VM-To-Existing-AzureDevTestLab-From-VSTS)
* [Verwenden der Team Services-Releaseverwaltung für Continuous Deployments in DevTest Labs](http://www.visualstudiogeeks.com/blog/DevOps/Use-VSTS-ReleaseManagement-to-Deploy-and-Test-in-AzureDevTestLabs)

Für andere Continuous Integration(CI)/Continuous Delivery(CD)-Toolketten können Sie dieselben Szenarien erreichen, indem Sie [Azure Resource Manager-Vorlagen](https://aka.ms/dtlquickstarttemplate) mit [Azure PowerShell-Cmdlets](../azure-resource-manager/resource-group-template-deploy.md) und [.NET SDKs](https://www.nuget.org/packages/Microsoft.Azure.Management.DevTestLabs/) bereitstellen. Sie können auch [REST-APIs für DevTest Labs](http://aka.ms/dtlrestapis) zur Integration in Ihre Toolkette verwenden.  


**Virtuelle Computer**
## <a name="why-cant-i-see-vms-on-the-virtual-machines-blade-that-i-see-in-devtest-labs"></a>Warum kann ich auf dem Blatt „Virtuelle Computer“ nicht die VMs sehen, die in DevTest Labs angezeigt werden?
Beim Erstellen einer VM in DevTest Labs wird Ihnen eine Berechtigung für den Zugriff auf diese VM gewährt. Sie können die VM sowohl auf dem Blatt „Labs“ als auch auf dem Blatt **Virtuelle Computer** sehen. Benutzer, denen die DevTest Labs-Labbenutzerrolle zugewiesen ist, können alle im Lab erstellten VMs auf dem Blatt **Alle virtuellen Computer** des Labs sehen. Allerdings erhalten Benutzer mit der DevTest Labs-Labbenutzerrolle nicht automatisch Lesezugriff auf die VM-Ressourcen, die andere Benutzer erstellt haben. Deshalb werden diese VMs nicht auf dem Blatt **Virtuelle Computer** angezeigt.

## <a name="what-is-the-difference-between-a-custom-image-and-a-formula"></a>Was ist der Unterschied zwischen einem benutzerdefinierten Image und einer Formel?
Ein benutzerdefiniertes Image ist eine virtuelle Festplatte (VHD). Eine Formel ist ein Image, das Sie mit zusätzlichen Einstellungen konfigurieren und anschließend speichern und reproduzieren können. Ein benutzerdefiniertes Image kann vorteilhafter sein, wenn Sie schnell mehrere Umgebungen mit demselben grundlegenden, unveränderlichen Image erstellen möchten. Eine Formel eignet sich möglicherweise besser, wenn Sie die Konfiguration Ihrer VM mit den neuesten Komponenten als Teil eines virtuellen Netzwerks oder Subnetzes oder als VM mit einer bestimmten Größe reproduzieren möchten. Eine ausführlichere Erklärung finden Sie im Artikel [Vergleich zwischen benutzerdefinierten Images und Formeln in DevTest Labs](devtest-lab-comparing-vm-base-image-types.md).

## <a name="how-do-i-create-multiple-vms-from-the-same-template-at-once"></a>Wie kann ich mehrere VMs gleichzeitig anhand derselben Vorlage erstellen?
Sie haben zwei Optionen, um gleichzeitig mehrere VMs anhand derselben Vorlage zu erstellen:
* Sie können die [Visual Studio Team Services-Erweiterung für Aufgaben](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks) verwenden. 
* Sie können beim Erstellen einer VM [eine Resource Manager-Vorlage generieren](devtest-lab-add-vm.md#save-azure-resource-manager-template) und [die Resource Manager-Vorlage über Windows PowerShell bereitstellen](../azure-resource-manager/resource-group-template-deploy.md).

## <a name="how-do-i-move-my-existing-azure-vms-into-my-devtest-labs-lab"></a>Wie verschiebe ich meine vorhandenen Azure-VMs in mein DevTest Labs-Lab?
So kopieren Sie Ihre vorhandenen VMs in DevTest Labs

1. Kopieren Sie die VHD-Datei Ihrer vorhandenen VM mithilfe eines [Windows PowerShell-Skripts](https://github.com/Azure/azure-devtestlab/blob/master/Scripts/CopyVHDFromVMToLab.ps1).
2. [Erstellen Sie das benutzerdefinierte Image](devtest-lab-create-template.md) in Ihrem DevTest Labs-Lab.
3. Erstellen Sie anhand des benutzerdefinierten Image eine VM im Lab.

## <a name="can-i-attach-multiple-disks-to-my-vms"></a>Kann ich mehrere Datenträger an meine VMs anfügen?
Ja, Sie können mehrere Datenträger an Ihre VMs anfügen.  

## <a name="if-i-want-to-use-a-windows-os-image-for-my-testing-do-i-have-to-purchase-an-msdn-subscription"></a>Wenn ich ein Windows-Betriebssystemimage für meine Tests verwenden möchte, muss ich dann ein MSDN-Abonnement erwerben?
Um Windows-Clientbetriebssystem-Images (Windows 7 oder höher) für Ihre Entwicklung oder Ihre Tests in Azure zu verwenden, müssen Sie eine der folgenden Aktionen durchführen:

- [Ein MSDN-Abonnement erwerben](https://www.visualstudio.com/products/how-to-buy-vs).
- Wenn Sie über Enterprise Agreement verfügen, erstellen Sie mit dem [Enterprise Dev/Test-Angebot](https://azure.microsoft.com/en-us/offers/ms-azr-0148p) ein Azure-Abonnement.

Weitere Informationen zu den Azure-Gutschriften für die einzelnen MSDN-Angebote finden Sie unter [Monatliche Azure-Gutschrift für Visual Studio-Abonnenten](https://azure.microsoft.com/en-us/pricing/member-offers/msdn-benefits-details/).

## <a name="how-do-i-automate-the-process-of-uploading-vhd-files-to-create-custom-images"></a>Wie automatisiere ich das Hochladen von VHD-Dateien zum Erstellen benutzerdefinierter Images?
Um das Hochladen von VHD-Dateien zum Erstellen benutzerdefinierter Images automatisieren, haben Sie zwei Optionen:

* Verwenden Sie [AzCopy](../storage/common/storage-use-azcopy.md#blob-upload), um VHD-Dateien in das dem Lab zugeordneten Speicherkonto zu kopieren oder hochzuladen.
* Verwenden Sie den [Azure Storage Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md). Der Storage Explorer ist eine eigenständige App, die unter Windows, OS X und Linux ausgeführt wird.   

So suchen Sie nach dem Zielspeicherkonto, das Ihrem Lab zugeordnet ist:

1. Melden Sie sich beim [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040)an.
2. Wählen Sie im Menü auf der linken Seite die Option **Ressourcengruppen** aus.
3. Suchen Sie nach der Ressourcengruppe, die Ihrem Lab zugeordnet ist, und wählen Sie sie aus.
4. Wählen Sie unter **Übersicht** eines der Speicherkonten aus.
5. Wählen Sie **Blobs**aus.
6. Suchen Sie in der Liste nach Uploads. Falls kein Upload vorhanden ist, kehren Sie zu Schritt 4 zurück, und versuchen Sie es mit einem anderen Speicherkonto.
7. Verwenden Sie die **URL** im AzCopy-Befehl als Ziel.

## <a name="how-do-i-automate-the-process-of-deleting-all-the-vms-in-my-lab"></a>Wie kann ich den Löschvorgang für alle VMs in meinem Lab automatisieren?
Sie können VMs aus Ihrem Lab im Azure-Portal löschen. Mit einem PowerShell-Skript können Sie ebenfalls alle VMs im Lab löschen. Ändern Sie im folgenden Beispiel die Werte der Parameter unter dem Kommentar **Zu ändernde Werte**. Sie können die Werte `subscriptionId`, `labResourceGroup` und `labName` über den Labbereich im Azure-Portal abrufen.

    # Delete all the VMs in a lab.

    # Values to change:
    $subscriptionId = "<Enter Azure subscription ID here>"
    $labResourceGroup = "<Enter lab's resource group here>"
    $labName = "<Enter lab name here>"

    # Sign in to your Azure account.
    Login-AzureRmAccount

    # Select the Azure subscription that has the lab. This step is optional
    # if you have only one subscription.
    Select-AzureRmSubscription -SubscriptionId $subscriptionId

    # Get the lab that has the VMs that you want to delete.
    $lab = Get-AzureRmResource -ResourceId ('subscriptions/' + $subscriptionId + '/resourceGroups/' + $labResourceGroup + '/providers/Microsoft.DevTestLab/labs/' + $labName)

    # Get the VMs from that lab.
    $labVMs = Get-AzureRmResource | Where-Object {
              $_.ResourceType -eq 'microsoft.devtestlab/labs/virtualmachines' -and
              $_.ResourceName -like "$($lab.ResourceName)/*"}

    # Delete the VMs.
    foreach($labVM in $labVMs)
    {
        Remove-AzureRmResource -ResourceId $labVM.ResourceId -Force
    }

**Artefakte**
## <a name="what-are-artifacts"></a>Was sind Artefakte?
Artefakte sind anpassbare Elemente, mit denen Sie die neuesten Komponenten oder Ihre Entwicklungstools auf einer VM bereitstellen können. Fügen Sie beim Erstellen der VM Artefakte an Ihre VM an. Nach der Bereitstellung der VM wird Ihre VM durch die Artefakte bereitgestellt und konfiguriert. In unserem [öffentlichen GitHub-Repository](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts) stehen verschiedene bereits vorhandene Artefakte zur Verfügung. Sie können auch [eigene Artefakte erstellen](devtest-lab-artifact-author.md).


**Labkonfiguration**
## <a name="how-do-i-create-a-lab-from-a-resource-manager-template"></a>Wie erstelle ich ein Lab anhand einer Resource Manager-Vorlage?
Wir stellen ein [GitHub-Repository von Azure Resource Manager-Labvorlagen](https://aka.ms/dtlquickstarttemplate) zur Verfügung, die Sie in vorliegender oder geänderter Form zum Erstellen benutzerdefinierter Vorlagen für Ihre Labs bereitstellen können. Jede Vorlage enthält einen Link, um das Lab in Ihrem eigenen Azure-Abonnement bereitzustellen. Alternativ können Sie die Vorlage auch anpassen und [mithilfe von PowerShell oder der Azure CLI bereitstellen](../azure-resource-manager/resource-group-template-deploy.md).

## <a name="why-are-my-vms-created-in-different-resource-groups-with-arbitrary-names-can-i-rename-or-modify-these-resource-groups"></a>Warum werden meine VMs in verschiedenen Ressourcengruppen mit willkürlichen Namen erstellt? Kann ich diese Ressourcengruppen umbenennen oder ändern?
Ressourcengruppen werden auf diese Weise erstellt, um DevTest Labs die Verwaltung von Benutzerberechtigungen und den Zugriff auf VMs zu ermöglichen. Sie können eine VM zwar in eine andere Ressourcengruppe verschieben und den gewünschten Namen verwenden. Es wird jedoch davon abgeraten, Änderungen an den Ressourcengruppen vorzunehmen. Wir arbeiten daran, diese Situation zu verbessern, um mehr Flexibilität zu ermöglichen.   

## <a name="how-many-labs-can-i-create-under-the-same-subscription"></a>Wie viele Labs kann ich unter demselben Abonnement erstellen?
Es gibt keine bestimmte Beschränkung für die Anzahl von Labs, die pro Abonnement erstellt werden können. Allerdings ist die Menge der pro Abonnement nutzbaren Ressourcen begrenzt. Weitere Informationen finden Sie bei Bedarf in den Artikeln zu den [Einschränkungen für Azure-Abonnements und Kontingente](../azure-subscription-service-limits.md) und zum [Erhöhen dieser Limits](https://azure.microsoft.com/blog/azure-limits-quotas-increase-requests).

## <a name="how-many-vms-can-i-create-per-lab"></a>Wie viele VMs kann ich pro Lab erstellen?
Es gibt keine bestimmte Beschränkung für die Anzahl von virtuellen Computern (VMs), die pro Abonnement erstellt werden können. Die Menge an Ressourcen pro Abonnement (VM-Kerne, öffentliche IP-Adressen usw.) ist jedoch begrenzt. Weitere Informationen finden Sie bei Bedarf in den Artikeln zu den [Einschränkungen für Azure-Abonnements und Kontingente](../azure-subscription-service-limits.md) und zum [Erhöhen dieser Limits](https://azure.microsoft.com/blog/azure-limits-quotas-increase-requests).

## <a name="how-do-i-share-a-direct-link-to-my-lab"></a>Wie gebe ich einen direkten Link zu meinem Lab frei?

1. Navigieren Sie im Azure-Portal zum Lab.
2. Kopieren Sie die Lab-URL aus Ihrem Browser, und geben Sie sie dann für Ihre Labbenutzer frei.

> [!NOTE]
> Wenn es sich bei einem Labbenutzer um einen externen Benutzer handelt, der ein [Microsoft-Konto](#what-is-a-microsoft-account) besitzt, jedoch nicht Mitglied der Active Directory-Instanz Ihrer Organisation ist, wird dem Benutzer bei dem Versuch, auf den freigegebenen Link zuzugreifen, möglicherweise eine Fehlermeldung angezeigt. Wenn einem externen Benutzer eine Fehlermeldung angezeigt wird, fordern Sie den Benutzer auf, oben rechts im Azure-Portal zuerst seinen Namen auszuwählen. Dann kann der Benutzer im Abschnitt **Verzeichnis** des Menüs das Verzeichnis auswählen, in dem sich das Lab befindet.
>
>

## <a name="what-is-a-microsoft-account"></a>Was ist ein Microsoft-Konto?
Ein Microsoft-Konto ist ein Konto, das Sie für nahezu jede Aktion verwenden, die Sie mit Geräten und Diensten von Microsoft ausführen. Es umfasst eine E-Mail-Adresse und ein Kennwort, mit dem Sie sich bei Skype, Outlook.com, OneDrive, Windows Phone und Xbox Live anmelden. Durch ein einzelnes Konto können Sie auf jedem Gerät auf Ihre Dateien, Fotos, Kontakte und Einstellungen zugreifen.

> [!NOTE]
> Früher wurde ein Microsoft-Konto als *Windows Live ID* bezeichnet.
>
>


**Problembehandlung**
## <a name="my-artifact-failed-during-vm-creation-how-do-i-troubleshoot-it"></a>Für mein Artefakt ist während der VM-Erstellung ein Fehler aufgetreten. Wie kann ich das Problem beheben?
Weitere Informationen zum Abrufen von Protokollen für das fehlerhafte Artefakt finden Sie unter [Diagnostizieren von Artefaktfehlern in DevTest Labs](devtest-lab-troubleshoot-artifact-failure.md).

## <a name="why-isnt-my-existing-virtual-network-saving-properly"></a>Warum wird mein vorhandenes virtuelles Netzwerk nicht korrekt gespeichert?
Möglicherweise enthält der Name des virtuellen Netzwerks Punkte. Wenn dies der Fall ist, entfernen Sie die Punkte, oder ersetzen Sie sie durch Trennstriche. Versuchen Sie dann erneut, das virtuelle Netzwerk zu speichern.

## <a name="why-do-i-get-a-parent-resource-not-found-error-when-i-provision-a-vm-from-powershell"></a>Warum erhalte ich bei der Bereitstellung einer VM von PowerShell den Fehler „Übergeordnete Ressource wurde nicht gefunden“?
Wenn eine Ressource einer anderen übergeordnet ist, muss die übergeordnete Ressource vor dem Erstellen der untergeordneten Ressource bereits vorhanden sein. Wenn die übergeordnete Ressource nicht vorhanden ist, wird eine **ParentResourceNotFound**-Meldung angezeigt. Wenn Sie keine Abhängigkeit von der übergeordneten Ressource angeben, wird die untergeordnete Ressource möglicherweise vor der übergeordneten bereitgestellt.

VMs sind untergeordnete Ressourcen unter einem Lab in einer Ressourcengruppe. Wenn Sie Resource Manager-Vorlagen zur Bereitstellung von VMs über PowerShell verwenden, sollte der im PowerShell-Skript bereitgestellte Name der Ressourcengruppe der Name der Ressourcengruppe des Labs sein. Weitere Informationen finden Sie unter [Beheben verbreiteter Azure-Bereitstellungsfehler](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-manager-common-deployment-errors#parentresourcenotfound).

## <a name="where-can-i-find-more-error-information-if-a-vm-deployment-fails"></a>Wo finde ich weitere Fehlerinformationen zu Fehlern bei einer VM-Bereitstellung?
VM-Bereitstellungsfehler werden in Aktivitätsprotokollen erfasst. Sie finden die Aktivitätsprotokolle von virtuellen Labcomputern unter **Überwachungsprotokolle** oder **Diagnose des virtuellen Computers** auf dem VM-Blatt des Labs im Ressourcenmenü (das Blatt wird angezeigt, nachdem Sie die VM in der Liste **Meine virtuellen Computer** ausgewählt haben).

Gelegentlich tritt der Bereitstellungsfehler vor Beginn der VM-Bereitstellung auf. Dies ist beispielsweise der Fall, wenn das Abonnementlimit für eine Ressource, die mit der VM erstellt wurde, überschritten wird. In diesem Fall werden die Fehlerdetails in den Aktivitätsprotokollen auf Labebene erfasst. Aktivitätsprotokolle befinden sich im unteren Bereich der Einstellungen von **Konfiguration und Richtlinien**. Weitere Informationen zum Verwenden von Aktivitätsprotokollen in finden Sie unter [Anzeigen von Aktivitätsprotokollen, um Aktionen an Ressourcen zu überwachen](https://docs.microsoft.com/en-us/azure/azure-resource-manager/resource-group-audit).

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]


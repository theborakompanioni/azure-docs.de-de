<properties
	pageTitle="Häufig gestellte Fragen zu Azure DevTest Labs | Microsoft Azure"
	description="Hier finden Sie Antworten auf häufig gestellte Fragen zu Azure DevTest Labs."
	services="devtest-lab,virtual-machines"
	documentationCenter="na"
	authors="tomarcher"
	manager="douge"
	editor=""/>

<tags
	ms.service="devtest-lab"
	ms.workload="na"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="09/01/2016"
	ms.author="tarcher"/>

# Häufig gestellte Fragen zu Azure DevTest Labs

In diesem Artikel werden einige der am häufigsten gestellten Fragen zu Azure DevTest Labs beantwortet.

[AZURE.INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## Allgemein
- [Was kann ich tun, wenn meine Frage hier nicht beantwortet wird?](#what-if-my-question-isnt-answered-here)
- [Warum sollte ich Azure DevTest Labs verwenden?](#why-should-i-use-azure-devtest-labs)
- [Was bedeutet „Self-Service für sorgenfreies Arbeiten“?](#what-does-quotworry-free-self-servicequot-mean)
- [Wie kann ich Azure DevTest Labs verwenden?](#how-can-i-use-azure-devtest-labs)
- [Wie wird die Nutzung von Azure DevTest Labs abgerechnet?](#how-am-i-billed-for-azure-devtest-labs)
 
## Sicherheit 
- [Wie lauten die verschiedenen Sicherheitsstufen in Azure DevTest Labs?](#what-are-the-different-security-levels-in-azure-devtest-labs)
- [Wie erstelle ich eine bestimmte Rolle, um Benutzern das Ausführen einer einzelnen Aufgabe zu ermöglichen?](#how-do-i-create-a-role-to-allow-users-to-perform-a-specific-task)
 
## CI/CD-Integration und Automatisierung 
 
- [Lässt sich Azure DevTest Labs in meine CI/CD-Toolkette integrieren?](#does-azure-devtest-labs-integrate-with-my-cicd-toolchain)
 
## Virtual Machines 
 
- [Warum werden bestimmte VMs, die in Azure DevTest Labs angezeigt werden, nicht auf dem Blatt „Virtuelle Azure-Computer“ angezeigt?](#why-cant-i-see-certain-vms-in-the-azure-virtual-machines-blade-that-i-see-within-azure-devtest-labs)
- [Was ist der Unterschied zwischen benutzerdefinierten Images und Formeln?](#what-is-the-difference-between-custom-images-and-formulas)
- [Wie kann ich mehrere VMs gleichzeitig anhand derselben Vorlage erstellen?](#how-do-i-create-multiple-vms-from-the-same-template-at-once)
- [Wie verschiebe ich meine vorhandenen Azure-VMs in mein Azure DevTest Labs-Lab?](#how-do-i-move-my-existing-azure-vms-into-my-azure-devtest-labs-lab)
- [Kann ich mehrere Datenträger an meine VMs anfügen?](#can-i-attach-multiple-disks-to-my-vms)
- [Wie automatisiere ich das Hochladen von VHD-Dateien zum Erstellen benutzerdefinierter Images?](#how-do-i-automate-the-process-of-uploading-vhd-files-to-create-custom-images)
 
## Artefakte 
 
- [Was sind Artefakte?](#what-are-artifacts)
 
## Labkonfiguration 
 
- [Wie erstelle ich ein Lab anhand einer Azure Resource Manager-Vorlage?](#how-do-i-create-a-lab-from-an-azure-resource-manager-template)
- [Warum werden meine VMs in verschiedenen Ressourcengruppen mit willkürlichen Namen erstellt? Kann ich diese Ressourcengruppen umbenennen oder ändern?](#why-are-my-vms-created-in-different-resource-groups-with-arbitrary-names-can-i-rename-or-modify-these-resource-groups)
- [Wie viele Labs kann ich unter demselben Abonnement erstellen?](#how-many-labs-can-i-create-under-the-same-subscription)
- [Wie viele VMs kann ich pro Lab erstellen?](#how-many-vms-can-i-create-per-lab)
 
## Problembehandlung 
 
- [Für mein Artefakt ist während der VM-Erstellung ein Fehler aufgetreten. Wie kann ich das Problem beheben?](#my-artifact-failed-during-vm-creation-how-do-i-troubleshoot-it)
- [Warum wird mein vorhandenes virtuelles Netzwerk nicht korrekt gespeichert?](#why-isnt-my-existing-virtual-network-saving-properly)

### Was kann ich tun, wenn meine Frage hier nicht beantwortet wird?
Wenn Ihre Frage hier nicht aufgeführt wird, informieren Sie uns, und wir helfen Ihnen dabei, eine Antwort zu finden.

- Stellen Sie eine Frage im [Disqus-Thread](#comments) am Ende dieser FAQ, und diskutieren Sie mit dem Azure Cache-Team und anderen Mitgliedern der Community über diesen Artikel.
- Um eine größere Benutzergruppe zu erreichen, stellen Sie eine Frage im [MSDN-Forum zu Azure DevTest Labs](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureDevTestLabs), und diskutieren mit dem Azure DevTest Labs-Team und anderen Mitgliedern der Community.
- Um ein neues Feature vorzuschlagen, übermitteln Sie Ihre Anfragen und Ideen in [Azure DevTest Labs User Voice](https://feedback.azure.com/forums/320373-azure-devtest-labs).

### Warum sollte ich Azure DevTest Labs verwenden? 
Mit Azure DevTest Labs kann Ihr Team Zeit und Geld sparen. Entwickler können mit verschiedenen Basiskonfigurationen eigene Umgebungen erstellen und Artefakte zum schnellen Bereitstellen und Konfigurieren von Anwendungen verwenden. Mithilfe von benutzerdefinierten Images und Formeln können virtuelle Computer als Vorlagen gespeichert und mühelos reproduziert werden. Darüber hinaus bieten Labs mehrere konfigurierbare Richtlinien, mit denen Labadministratoren die Vergeudung von Ressourcen reduzieren und die Umgebungen eines Teams verwalten können. Diese Richtlinien beinhalten u.a. das automatische Herunterfahren, Kostenschwellenwerte, eine maximale Anzahl von VMs pro Benutzer und maximale VM-Größen. Eine ausführlichere Erläuterung von Azure DevTest Labs finden Sie in der [Übersicht](devtest-lab-overview.md) oder im [Einführungsvideo](/documentation/videos/videos/what-is-azure-devtest-labs).

### Was bedeutet „Self-Service für sorgenfreies Arbeiten“?
„Self-Service für sorgenfreies Arbeiten“ bedeutet, dass die Entwickler und Tester nach Bedarf eigene Umgebungen erstellen können. Die Administratoren wissen dabei genau, dass bei Azure DevTest Labs keine Ressourcen vergeudet werden und dass sie die Kosten vollständig kontrollieren können. Administratoren können die zulässigen VM-Größen und die maximale Anzahl von VMs angeben und festlegen, wann VMs gestartet und beendet werden. Azure DevTest Labs erleichtert auch die Überwachung der Kosten und das Festlegen von Warnungen, sodass sie stets wissen, wie Ressourcen im Lab verwendet werden.

### Wie kann ich Azure DevTest Labs verwenden? 
Azure DevTest Labs eignet sich, wenn Sie Entwicklungs- oder Testumgebungen benötigen und diese schnell reproduzieren und/oder mit kostensparenden Richtlinien verwalten möchten.

Unsere Kunden verwenden Azure DevTest Labs beispielsweise für folgende Szenarien:

- Verwaltung von Entwicklungs- und Testumgebungen an einer zentralen Stelle mithilfe von Richtlinien zum Reduzieren der Kosten und benutzerdefinierten Images zum Freigeben von Builds innerhalb des Teams
- Entwicklung von Anwendungen mit benutzerdefinierten Images, um den Datenträgerzustand in den Entwicklungsphasen zu speichern
- Nachverfolgung der Kosten im Verhältnis zum Status
- Erstellung von Massentestumgebungen für Qualitätssicherungstests
- Verwendung von Artefakten und Formeln zum einfachen Konfigurieren und Reproduzieren von Anwendungen für verschiedene Umgebungen
- Verteilung von VMs für Hackathons (gemeinschaftliche Entwicklungs- oder Testarbeiten) und einfache Aufhebung der Bereitstellung nach Beendigung des Ereignisses

### Wie wird die Nutzung von Azure DevTest Labs abgerechnet? 
Azure DevTest Labs ist ein kostenloser Service. Für das Erstellen von Labs sowie das Konfigurieren der Richtlinien, Vorlagen und Artefakte fallen keinerlei Kosten an. Sie bezahlen nur für die Azure-Ressourcen, die Sie in den Labs verwenden, beispielsweise virtuelle Computer, Speicherkonten und virtuelle Netzwerke. Informieren Sie sich über die [Preise von Azure DevTest Labs](https://azure.microsoft.com/pricing/details/devtest-lab/), um mehr über die Kosten von Labressourcen zu erfahren.

### Wie lauten die verschiedenen Sicherheitsstufen in Azure DevTest Labs?  
Der Sicherheitszugriff wird durch die [rollenbasierte Zugriffssteuerung von Azure (Role-Based Access Control, RBAC)](../active-directory/role-based-access-built-in-roles.md) bestimmt. Die Funktionsweise des Zugriffs ist leichter zu verstehen, wenn Sie die Unterschiede zwischen Berechtigungen, Rollen und Bereichen in RBAC kennen.

- **Berechtigung**: Eine Berechtigung ermöglicht einen definierten Zugriff auf eine bestimmte Aktion. Zum Beispiel kann eine Berechtigung den Lesezugriff auf alle virtuellen Computer ermöglichen.
- **Rolle**: Eine Rolle ist ein Satz von Berechtigungen, die gruppiert und einem Benutzer zugewiesen werden können. Zum Beispiel verfügt die Rolle „Abonnementbesitzer“ über Zugriff auf alle Ressourcen in einem Abonnement.
- **Bereich**: Ein Bereich ist eine Stufe innerhalb der Hierarchie einer Azure-Ressource. Zum Beispiel kann eine Ressourcengruppe oder ein einzelnes Lab oder das gesamte Abonnement ein Bereich sein.
 
Innerhalb des Bereichs von Azure DevTest Labs gibt es zwei Arten von Rollen, mit denen Benutzerberechtigungen definiert werden: Labbesitzer und Labbenutzer.

- **Labbesitzer**: Ein Labbesitzer verfügt über Zugriff auf alle Ressourcen innerhalb des Labs. Daher kann er Richtlinien und das virtuelle Netzwerk ändern, verfügt über Lese- und Schreibberechtigungen für alle virtuellen Computer usw.
- **Labbenutzer**: Ein Labbenutzer kann alle Labressourcen anzeigen (z.B. VMs, Richtlinien und virtuelle Netzwerke), aber keine Richtlinien oder von anderen Benutzern erstellte virtuelle Computer ändern. Es ist auch möglich, benutzerdefinierte Rollen in Azure DevTest Labs zu erstellen. Informationen dazu finden Sie im Artikel [Gewähren von Benutzerberechtigungen für bestimmte Labrichtlinien](devtest-lab-grant-user-permissions-to-specific-lab-policies.md).
 
Da Bereiche hierarchisch sind, gelten die Berechtigungen, die einem Benutzer für einen bestimmten Bereich gewährt werden, automatisch für alle darin enthaltenen Bereiche auf niedrigerer Ebene. Ist einem Benutzer beispielsweise die Rolle „Abonnementbesitzer“ zugewiesen, hat er Zugriff auf alle Ressourcen in einem Abonnement. Zu diesen Ressourcen gehören alle virtuellen Computer, alle virtuellen Netzwerke und alle Labs. Der Abonnementbesitzer erbt somit automatisch die Rolle des Labbesitzers. Umgekehrt ist dies jedoch nicht der Fall. Ein Labbesitzer hat Zugriff auf ein Lab, d.h. auf einen Bereich, der sich innerhalb der Hierarchie auf einer niedrigeren Ebene befindet als das Abonnement. Aus diesem Grund kann ein Labbesitzer keine virtuellen Computer, virtuellen Netzwerke oder anderen Ressourcen außerhalb des Labs anzeigen.

### Wie erstelle ich eine bestimmte Rolle, um Benutzern das Ausführen einer einzelnen Aufgabe zu ermöglichen?
Einen ausführlichen Artikel zum Erstellen von benutzerdefinierten Rollen und Zuweisen von Berechtigungen zu dieser Rolle finden Sie hier. Das folgende Beispielskript erstellt die Rolle „DevTest Labs Advanced User“, die über die Berechtigung zum Starten und Beenden alle VMs im Lab verfügt:
 
	$policyRoleDef = (Get-AzureRmRoleDefinition "DevTest Labs User") 
	$policyRoleDef.Actions.Remove('Microsoft.DevTestLab/Environments/*') 
	$policyRoleDef.Id = $null 
	$policyRoleDef.Name = "DevTest Labs Advance User" 
	$policyRoleDef.IsCustom = $true 
	$policyRoleDef.AssignableScopes.Clear() 
	$policyRoleDef.AssignableScopes.Add("subscriptions/<subscription Id>") 
	$policyRoleDef.Actions.Add("Microsoft.DevTestLab/labs/virtualMachines/Start/action") 
	$policyRoleDef.Actions.Add("Microsoft.DevTestLab/labs/virtualMachines/Stop/action") 
	$policyRoleDef = (New-AzureRmRoleDefinition -Role $policyRoleDef)  
 
### Lässt sich Azure DevTest Labs in meine CI/CD-Toolkette integrieren? 
Wenn Sie Visual Studio Team Services (VSTS) verwenden, ist eine [Azure DevTest Labs-Erweiterung für Aufgaben](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks) verfügbar, mit der Sie Ihre Releasepipeline in Azure DevTest Labs automatisieren können. Die Erweiterung kann beispielsweise für folgende Zwecke verwendet werden:

- Automatisches Erstellen und Bereitstellen einer VM und Konfigurieren der VM mit dem aktuellen Build mithilfe des Azure-Dateikopiervorgangs oder von PowerShell-VSTS-Aufgaben
- Automatisches Erfassen des Zustands einer VM nach dem Testen, um zur weiteren Untersuchung einen Fehler auf derselben VM zu reproduzieren
- Löschen der VM am Ende der Releasepipeline, wenn sie nicht mehr benötigt wird

Die folgenden Blogbeiträge enthalten Anleitungen und Informationen zur Verwendung der VSTS-Erweiterung:
 
- [Azure DevTest Labs – VSTS extension (Azure DevTest Labs – VSTS-Erweiterung)](https://blogs.msdn.microsoft.com/devtestlab/2016/06/15/azure-devtest-labs-vsts-extension/)
- [Deploying a new VM in an existing AzureDevTestLab from VSTS (Bereitstellen einer neuen VM in einem vorhandenen AzureDevTestLab über VSTS)](http://www.visualstudiogeeks.com/blog/DevOps/Deploy-New-VM-To-Existing-AzureDevTestLab-From-VSTS)
- [Using VSTS Release Management for Continuous Deployments to AzureDevTestLabs (Verwenden der VSTS-Releaseverwaltung für kontinuierliche Bereitstellungen in AzureDevTestLabs)](http://www.visualstudiogeeks.com/blog/DevOps/Use-VSTS-ReleaseManagement-to-Deploy-and-Test-in-AzureDevTestLabs)
 
Für andere CI/CD-Toolketten können alle der oben genannten Szenarien, die mit der Erweiterung für VSTS-Aufgaben möglich sind, auf ähnliche Weise durch die Bereitstellung von [Azure Resource Manager-Vorlagen](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates) mit [Azure PowerShell-Cmdlets](../resource-group-template-deploy.md) und [.NET SDKs](https://www.nuget.org/packages/Microsoft.Azure.Management.DevTestLabs/) umgesetzt werden. Sie können auch [REST-APIs für DevTest Labs](http://aka.ms/dtlrestapis) zur Integration in Ihre Toolkette verwenden.

### Warum werden bestimmte VMs, die in Azure DevTest Labs angezeigt werden, nicht auf dem Blatt „Virtuelle Azure-Computer“ angezeigt?
Wenn ein virtueller Computer in Azure DevTest Labs erstellt wird, wird eine Berechtigung zum Zugriff auf diesen virtuellen Computer erstellt. Sie können ihn sowohl auf dem Blatt für die Labs als auch auf dem Blatt **Virtuelle Computer** anzeigen. Benutzer mit der DevTest Labs-Rolle können alle im Lab erstellten virtuellen Computer auf dem Blatt **Alle virtuellen Computer** des Labs sehen. Allerdings erhalten Benutzer mit der DevTest Labs-Rolle nicht automatisch Lesezugriff auf die VM-Ressourcen, die andere erstellt haben. Deshalb werden diese virtuellen Computer nicht auf dem Blatt **Virtuelle Computer** angezeigt.

### Was ist der Unterschied zwischen benutzerdefinierten Images und Formeln? 
Ein benutzerdefiniertes Image ist eine VHD (virtuelle Festplatte), während eine Formel ein Image ist, das Sie mit zusätzlichen Einstellungen konfigurieren sowie speichern und reproduzieren können. Ein benutzerdefiniertes Image kann vorteilhafter sein, wenn Sie schnell mehrere Umgebungen mit demselben grundlegenden, unveränderlichen Image erstellen möchten. Eine Formel eignet sich möglicherweise besser, wenn Sie die Konfiguration Ihrer VM mit den aktuellsten Komponenten, einem virtuellen Netzwerk/Subnetz oder einer bestimmten Größe reproduzieren möchten. Eine ausführlichere Erklärung finden Sie im Artikel [Vergleich zwischen benutzerdefinierten Images und Formeln in DevTest Labs](devtest-lab-comparing-vm-base-image-types.md).
 
### Wie kann ich mehrere VMs gleichzeitig anhand derselben Vorlage erstellen? 
Sie können die [Erweiterung für VSTS-Aufgaben](https://marketplace.visualstudio.com/items?itemName=ms-azuredevtestlabs.tasks) verwenden oder beim Erstellen einer VM eine [Azure Resource Manager-Vorlage generieren](devtest-lab-add-vm-with-artifacts.md#save-arm-template) und die [Azure Resource Manager-Vorlage über Windows PowerShell bereitstellen](../resource-group-template-deploy.md).
 
### Wie verschiebe ich meine vorhandenen Azure-VMs in mein Azure DevTest Labs-Lab? 
Wir arbeiten an einer Lösung, die das direkte Verschieben von VMs nach Azure DevTest Labs ermöglicht. Zurzeit können Sie Ihre vorhandenen VMs jedoch wie folgt nach Azure DevTest Labs verschieben:

1. Kopieren Sie die VHD-Datei der vorhandenen VM mithilfe des folgenden [Windows PowerShell-Skripts](https://github.com/Azure/azure-devtestlab/blob/master/Scripts/CopyVHDFromVMToLab.ps1).
1. [Erstellen Sie das benutzerdefinierte Image](devtest-lab-create-template.md) in Ihrem Azure DevTest Labs-Lab.
1. Erstellen Sie anhand des benutzerdefinierten Images eine VM im Lab.
 
### Kann ich mehrere Datenträger an meine VMs anfügen? 
Das Anfügen mehrerer Datenträger an VMs wird unterstützt.
 
### Wie automatisiere ich das Hochladen von VHD-Dateien zum Erstellen benutzerdefinierter Images? 
Es gibt zwei Optionen:

- [Azure AzCopy](../storage/storage-use-azcopy.md#blob-upload) kann verwendet werden, um VHD-Dateien in das dem Lab zugeordnete Speicherkonto zu kopieren oder hochzuladen.
- [Microsoft Azure-Speicher-Explorer](../vs-azure-tools-storage-manage-with-storage-explorer.md) ist eine eigenständige App, die unter Windows, OSX und Linux ausgeführt wird.
 
Führen Sie folgende Schritte aus, um nach dem Zielspeicherkonto zu suchen, das Ihrem Lab zugeordnet ist:

1. Melden Sie sich auf dem [Azure-Portal](http://go.microsoft.com/fwlink/p/?LinkID=525040) an.
1. Wählen Sie im linken Bereich **Ressourcengruppen** aus.
1. Suchen Sie nach der Ressourcengruppe, die Ihrem Lab zugeordnet ist, und wählen Sie sie aus.
1. Wählen Sie auf dem Blatt **Übersicht** eines der Speicherkonten aus.
1. Wählen Sie **Blobs** aus.
1. Suchen Sie in der Liste nach Uploads. Falls kein Upload vorhanden ist, kehren Sie zu Schritt 4 zurück, und versuchen Sie es mit einem anderen Speicherkonto.
1. Verwenden Sie die **URL** im AzCopy-Befehl als Ziel.

### Was sind Artefakte? 
Artefakte sind anpassbare Elemente, die verwendet werden können, um die neuesten Komponenten oder Ihre Entwicklungstools auf einer VM bereitzustellen. Sie werden während der VM-Erstellung mit ein paar einfachen Klicks an die VM angefügt. Nach der Bereitstellung der VM wird Ihre VM von den Artefakten bereitgestellt und konfiguriert. In unserem [öffentlichen GitHub-Repository](https://github.com/Azure/azure-devtestlab/tree/master/Artifacts) ist bereits eine Reihe von Artefakten verfügbar, Sie können aber auch ganz einfach [eigene Artefakte erstellen](devtest-lab-artifact-author.md).

### Wie erstelle ich ein Lab anhand einer Azure Resource Manager-Vorlage? 
Wir haben ein [Github-Repository mit Azure Resource Manager-Vorlagen für Labs](https://github.com/Azure/azure-devtestlab/tree/master/ARMTemplates). Jede dieser Vorlagen enthält einen Link, auf den Sie klicken können, um das Azure DevTest Labs-Lab unter Ihrem eigenen Azure-Abonnement bereitzustellen.
 
### Warum werden meine VMs in verschiedenen Ressourcengruppen mit willkürlichen Namen erstellt? Kann ich diese Ressourcengruppen umbenennen oder ändern? 
Ressourcengruppen werden auf diese Weise erstellt, damit Azure DevTest Labs die Benutzerberechtigungen und den Zugriff auf virtuelle Computer verwalten kann. Sie können einen virtuellen Computer zwar in eine andere Ressourcengruppe mit dem gewünschten Namen verschieben, dies wird jedoch nicht empfohlen. Wir arbeiten daran, diese Situation zu verbessern, um mehr Flexibilität zu ermöglichen.
 
### Wie viele Labs kann ich unter demselben Abonnement erstellen? 
Es gibt keine bestimmte Beschränkung für die Anzahl von Labs, die pro Abonnement erstellt werden können. Allerdings sind die pro Abonnement nutzbaren Ressourcen begrenzt. Weitere Informationen finden Sie bei Bedarf in den Artikeln zu den [Einschränkungen für Azure-Abonnements und Dienste, Kontingente und Einschränkungen](../azure-subscription-service-limits.md) und zum [Erhöhen dieser Limits](https://azure.microsoft.com/blog/azure-limits-quotas-increase-requests).
 
### Wie viele VMs kann ich pro Lab erstellen? 
Es gibt keine bestimmte Beschränkung für die Anzahl von virtuellen Computern (VMs), die pro Abonnement erstellt werden können. Das Lab unterstützt zurzeit aber nur ca. 40 gleichzeitig ausgeführte VMs für Standardspeicher und 25 gleichzeitig ausgeführte VMs für Storage Premium. Wir arbeiten daran, diese Limits zu erhöhen.
 
### Für mein Artefakt ist während der VM-Erstellung ein Fehler aufgetreten. Wie kann ich das Problem beheben? 
Im Blogbeitrag [How to troubleshoot failing Artifacts in AzureDevTestLabs](http://www.visualstudiogeeks.com/blog/DevOps/How-to-troubleshoot-failing-artifacts-in-AzureDevTestLabs) (Problembehandlung für fehlerhafte Artefakte in AzureDevTestLabs) eines unserer MVPs finden Sie Informationen dazu, wie Sie Protokolle für das fehlgeschlagene Artefakt abrufen können.
 
### Warum wird mein vorhandenes virtuelles Netzwerk nicht korrekt gespeichert?  
Möglicherweise enthält der Name des virtuellen Netzwerks Punkte. Wenn dies der Fall ist, können Sie die Punkte entfernen oder durch Bindestriche ersetzen und dann noch einmal versuchen, das virtuelle Netzwerk zu speichern.

<!---HONumber=AcomDC_0907_2016-->
---
title: "Veröffentlichen von Anwendungen für einzelne Benutzer in einer Azure RemoteApp-Sammlung (Vorschau) | Microsoft Docs"
description: "Erfahren Sie, wie Sie Apps in Azure RemoteApp für einzelne Benutzer veröffentlichen können, und nicht mehr nur für ganze Gruppen."
services: remoteapp-preview
documentationcenter: 
author: piotrci
manager: mbaldwin
editor: 
ms.assetid: 1fd0539d-fa65-4ea5-a98e-0be0cf580690
ms.service: remoteapp
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: compute
ms.date: 11/23/2016
ms.author: piotrci
translationtype: Human Translation
ms.sourcegitcommit: 5cce99eff6ed75636399153a846654f56fb64a68
ms.openlocfilehash: 20f9e9b31498f20c2a27f0d82d434abad73d2454
ms.lasthandoff: 03/31/2017


---
# <a name="publish-applications-to-individual-users-in-an-azure-remoteapp-collection-preview"></a>Veröffentlichen von Anwendungen für einzelne Benutzer in einer Azure RemoteApp-Sammlung (Vorschau)
> [!IMPORTANT]
> Azure RemoteApp wird am 31. August 2017 eingestellt. Details finden Sie in der [Ankündigung](https://go.microsoft.com/fwlink/?linkid=821148) .
> 
> 

Dieser Artikel erklärt, wie Anwendungen für einzelne Benutzer in einer Azure RemoteApp-Sammlung veröffentlicht werden. Dies ist eine neue Funktion in Azure RemoteApp, die derzeit nur bestimmten Erstanwendern zu Evaluierungszwecken in der privaten Vorschau zur Verfügung steht.

Ursprünglich hat Azure RemoteApp nur eine Methode zum Veröffentlichen von Anwendungen ermöglicht: Apps werden vom Administrator aus dem Image veröffentlicht und sind in der Sammlung für alle Benutzern sichtbar.

Häufig sind in einem Image viele Anwendungen enthalten, und zum Verringern der Verwaltungskosten wird eine Sammlung bereitgestellt. Oft sind nicht alle Anwendungen für alle Benutzer relevant, und Administratoren würden es vorziehen, Apps für individuelle Benutzer zu veröffentlichen, damit in deren Anwendungsfeed keine nicht benötigten Anwendungen erscheinen.

Dies ist in Azure RemoteApp nun als beschränkte Vorschaufunktion möglich. Im Folgenden finden Sie eine kurze Zusammenfassung dieser neuen Funktion:

1. Für eine Sammlung können zwei Modi festgelegt werden:
   
   * Der ursprüngliche Sammlungsmodus, in dem alle Benutzer in einer Sammlung alle veröffentlichten Anwendungen sehen können. Dies ist der Standardmodus.
   * Der neue Anwendungsmodus, in dem Benutzern nur ihnen explizit zugewiesene Anwendungen angezeigt werden.
2. Der Anwendungsmodus kann momentan nur mithilfe der Azure RemoteApp-PowerShell-Cmdlets aktiviert werden.
   
   * Im Anwendungsmodus kann die Benutzerzuweisung in der Sammlung nicht über das Azure-Portal verwaltet werden. Die Verwaltung der Benutzerzuweisung erfolgt über PowerShell-Cmdlets.
3. Benutzer sehen nur die Anwendungen, die direkt für sie veröffentlicht wurden. Unter Umständen kann ein Benutzer die anderen auf dem Image verfügbaren Anwendungen weiterhin starten, indem er direkt im Betriebssystem darauf zugreift.
   
   * Die Anwendungen werden durch diese Funktion nicht gesperrt, sie schränkt nur deren Sichtbarkeit im Anwendungsfeed ein.
   * Wenn Sie Benutzer von Anwendungen isolieren müssen, benötigen Sie dafür separate Sammlungen.

## <a name="how-to-get-azure-remoteapp-powershell-cmdlets"></a>Gewusst wie: Abrufen von Azure RemoteApp PowerShell-Cmdlets
Zum Testen der neuen Vorschaufunktion müssen Sie Azure PowerShell-Cmdlets verwenden. Derzeit ist es nicht möglich, das Azure-Verwaltungsportal zum Aktivieren des neuen Anwendungsveröffentlichungsmodus zu aktivieren.

Vergewissern Sie sich zunächst, dass das [Azure PowerShell-Modul](/powershell/azureps-cmdlets-docs) installiert ist.

Starten Sie danach die PowerShell-Konsole im Administratormodus, und führen Sie das folgende Cmdlet aus:

        Add-AzureAccount

Dieses fordert Sie zur Eingabe Ihres Azure-Benutzernamens sowie Ihres Kennworts auf. Nach der Anmeldung können Sie Azure RemoteApp-Cmdlets für Ihre Azure-Abonnements ausführen.

## <a name="how-to-check-which-mode-a-collection-is-in"></a>Überprüfen, in welchem Modus sich eine Auflistung befindet
Führen Sie das folgende Cmdlet aus:

        Get-AzureRemoteAppCollection <collectionName>

![Überprüfen Sie den Modus der Sammlung.](./media/remoteapp-perapp/araacllelvel.png)

Die AclLevel-Eigenschaft kann die folgenden Werte aufweisen:

* Sammlung: der ursprüngliche Veröffentlichungsmodus. Veröffentlichte Apps werden allen Benutzern angezeigt.
* Anwendung: der neue Veröffentlichungsmodus. Benutzer sehen nur die Apps, die direkt für sie veröffentlicht wurden.

## <a name="how-to-switch-to-application-publishing-mode"></a>Gewusst wie: Wechseln zum Anwendungsveröffentlichungsmodus
Führen Sie das folgende Cmdlet aus:

        Set-AzureRemoteAppCollection -CollectionName -AclLevel Application

Der Status der Anwendungsveröffentlichung bleibt beibehalten: Zunächst sehen alle Benutzer alle ursprünglich veröffentlichten Apps.

## <a name="how-to-list-users-who-can-see-a-specific-application"></a>Gewusst wie: Auflisten von Benutzern, denen eine bestimmte Anwendung angezeigt wird
Führen Sie das folgende Cmdlet aus:

        Get-AzureRemoteAppUser -CollectionName <collectionName> -Alias <appAlias>

Dadurch werden alle Benutzer aufgelistet, die die Anwendung sehen können.

Hinweis: Sie können die Anwendungsaliase (in der oben angezeigten Syntax als „app alias“ bezeichnet) anzeigen, indem Sie Get-AzureRemoteAppProgram -CollectionName <collectionName> ausführen.

## <a name="how-to-assign-an-application-to-a-user"></a>Gewusst wie: Zuweisen einer Anwendung für einen Benutzer
Führen Sie das folgende Cmdlet aus:

        Add-AzureRemoteAppUser -CollectionName <collectionName> -UserUpn <user@domain.com> -Type <OrgId|MicrosoftAccount> -Alias <appAlias>

Dem Benutzer wird die Anwendung im Azure RemoteApp-Client nun angezeigt und, er kann eine Verbindung damit herstellen.

## <a name="how-to-remove-an-application-from-a-user"></a>Gewusst wie: Entfernen einer Anwendung von einem Benutzer
Führen Sie das folgende Cmdlet aus:

        Remove-AzureRemoteAppUser -CollectionName <collectionName> -UserUpn <user@domain.com> -Type <OrgId|MicrosoftAccount> -Alias <appAlias>

## <a name="providing-feedback"></a>Senden von Feedback
Wir freuen uns auf Ihr Feedback und Ihre Vorschläge zu dieser Vorschaufunktion. Bitte füllen Sie den [Fragebogen](http://www.instant.ly/s/FDdrb) aus, und teilen Sie uns Ihre Meinung mit.

## <a name="havent-had-a-chance-to-try-the-preview-feature"></a>Konnten Sie die Vorschaufunktion noch nicht ausprobieren?
Falls Sie noch nicht an der Vorschau teilgenommen haben, können Sie über diese [Umfrage](http://www.instant.ly/s/AY83p) Zugriff darauf anfordern.



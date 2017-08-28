---
title: "Einrichten von Stagingumgebungen für Web-Apps in Azure App Service | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie Stagingveröffentlichungen Ihrer Web-Apps in Azure App Service verwenden."
services: app-service
documentationcenter: 
author: cephalin
writer: cephalin
manager: erikre
editor: mollybos
ms.assetid: e224fc4f-800d-469a-8d6a-72bcde612450
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/16/2016
ms.author: cephalin
ms.translationtype: HT
ms.sourcegitcommit: b6c65c53d96f4adb8719c27ed270e973b5a7ff23
ms.openlocfilehash: bc6c77deb02b145cd412e18ba31d724f2b71e4d1
ms.contentlocale: de-de
ms.lasthandoff: 08/17/2017

---
# <a name="set-up-staging-environments-in-azure-app-service"></a>Einrichten von Stagingumgebungen in Azure App Service
<a name="Overview"></a>

Sie können die Bereitstellung Ihrer Web-App, Web-App unter Linux, Ihres mobilen Back-Ends oder Ihrer API-App in [App Service](http://go.microsoft.com/fwlink/?LinkId=529714) in einem separaten Bereitstellungsslot anstelle des Standardproduktionsslots vornehmen, wenn die Ausführung im App Service-Plan **Standard** oder **Premium** erfolgt. Bereitstellungsslots sind tatsächlich aktive Apps mit eigenen Hostnamen. Elemente für App-Inhalte und -Konfigurationen können zwischen zwei Bereitstellungsslots, einschließlich des Produktionsslots, ausgetauscht werden. Die Bereitstellung von Anwendungen in einem Bereitstellungsslot hat die folgenden Vorteile:

* Sie können App-Änderungen in einem Stagingbereitstellungsslot überprüfen, bevor Sie die App in den Produktionsslot überführen.
* Indem Sie eine App zuerst in einem Slot bereitstellen und sie dann in den Produktionsslot überführen, stellen Sie sicher, dass alle Instanzen erst nach einer Anlaufzeit in den Produktionsslot übernommen werden. Dadurch vermeiden Sie Ausfallzeiten bei der Bereitstellung der App. Die Verkehrsweiterleitung ist nahtlos, und es werden keine Anfragen aufgrund von Überführungsoperationen fallengelassen. Dieser gesamte Workflow kann durch Konfigurieren von [Automatisch tauschen](#Auto-Swap) automatisiert werden, wenn keine Überprüfung vor dem Austauschen erforderlich ist.
* Nach der Überführung enthält der Slot mit der vorherigen Staging-App die vorherige Produktions-App. Wenn die in den Produktionsslot überführten Änderungen nicht Ihren Erwartungen entsprechen, können Sie denselben Austausch sofort noch einmal vornehmen, um die "letzte als gut befundene Website" zurückzuerhalten.

Jeder App Service-Planmodus unterstützt eine andere Anzahl von Bereitstellungsslots. Informationen zum Herausfinden, wie viele Slots Ihr App-Modus unterstützt, finden Sie unter [App-Service-Preisdetails](https://azure.microsoft.com/pricing/details/app-service/).

* Wenn Ihre App mehrere Slots aufweist, können Sie den Modus nicht ändern.
* Die Skalierung ist für Nicht-Produktionsslots nicht verfügbar,
* Die Verwaltung verknüpfter Ressourcen wird für Nicht-Produktionsslots nicht unterstützt. Sie können diese negativen Auswirkungen für einen Produktionsslot nur im [Azure-Portal](http://go.microsoft.com/fwlink/?LinkId=529715) vermeiden, indem Sie den Nicht-Produktionsslot vorübergehend in einen anderen App Service-Planmodus verschieben. Beachten Sie, dass der Nicht-Produktionsslot wieder im selben Modus freigegeben werden muss wie der Produktionsslot, bevor Sie die beiden Slots austauschen können.

<a name="Add"></a>

## <a name="add-a-deployment-slot"></a>Hinzufügen eines Bereitstellungsslots
Die App muss im Modus **Standard** oder **Premium** ausgeführt werden, damit mehrere Bereitstellungsslots aktiviert werden können.

1. Öffnen Sie im [Azure-Portal](https://portal.azure.com/) das Blatt [Ressourcen](../azure-resource-manager/resource-group-portal.md#manage-resources) Ihrer App.
2. Wählen Sie die Option **Bereitstellungsslots** aus, und klicken Sie auf **Slot hinzufügen**.
   
    ![Neuen Bereitstellungsslot hinzufügen][QGAddNewDeploymentSlot]
   
   > [!NOTE]
   > Wenn die App nicht bereits im Modus **Standard** oder **Premium** ausgeführt wird, wird eine Meldung angezeigt, in der auf die unterstützten Modi für die Veröffentlichung in einer Stagingumgebung hingewiesen wird. An diesem Punkt können Sie **Upgrade** auswählen und zur Registerkarte **Skalierung** der App navigieren, bevor Sie den Vorgang fortsetzen.
   > 
   > 
3. Weisen Sie dem Slot auf dem Blatt **Slot hinzufügen** einen Namen zu, und wählen Sie aus, ob Sie die App-Konfiguration von einem anderen vorhandenen Bereitstellungsslot klonen möchten. Klicken Sie auf das Häkchen, um fortzufahren.
   
    ![Konfigurationsquelle][ConfigurationSource1]
   
    Beim ersten Hinzufügen eines Slots stehen Ihnen nur zwei Optionen zur Verfügung: Klonen einer Konfiguration vom Standardslot in einen Produktionsslot oder kein Klonen der Konfiguration.
    Nachdem Sie mehrere Slots erstellt haben, können Sie die Konfiguration von einem anderen Slot als dem Produktionsslot klonen:
   
    ![Konfigurationsquellen][MultipleConfigurationSources]
4. Klicken Sie auf dem Blatt „Ressourcen“ der App auf **Bereitstellungsslots** und dann auf einen Bereitstellungsslot, um das Blatt „Ressourcen“ dieses Slots zu öffnen. Darauf befinden sich wie bei anderen Apps verschiedene Metriken und Konfigurationsinformationen. Der Name des Slots wird oben auf dem Blatt gezeigt, um Sie daran zu erinnern, dass Sie den Bereitstellungsslot betrachten.
   
    ![Titel des Bereitstellungsslots][StagingTitle]
5. Klicken Sie auf der Seite des Slots auf die App-URL. Beachten Sie, dass der Bereitstellungsslot über einen eigenen Hostnamen verfügt und eine Live-App ist. Weitere Informationen darüber, wie Sie den öffentlichen Zugriff auf den Bereitstellungsslot beschränken, finden Sie unter [App Service-Web-App – Blockieren des Webzugangs auf Nicht-Produktionsslots](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/).

Nach der Erstellung des Bereitstellungsslots ist kein Inhalt vorhanden. Sie können die Bereitstellung im Slot von einem anderen Repositoryzweig oder einem ganz anderen Repository vornehmen. Darüber hinaus können Sie die Konfiguration des Slots ändern. Verwenden Sie für Aktualisierungen des Inhalts das Veröffentlichungsprofil oder die Bereitstellungsanmeldeinformationen, die dem Bereitstellungsslot zugeordnet sind.  Beispiel: [Für die Veröffentlichung in diesem Slot können Sie Git verwenden](app-service-deploy-local-git.md).

<a name="AboutConfiguration"></a>

## <a name="configuration-for-deployment-slots"></a>Konfiguration für Bereitstellungsslots
Wenn Sie die Konfiguration von einem anderen Bereitstellungsslot klonen, kann die geklonte Konfiguration bearbeitet werden. Darüber hinaus folgen einige Konfigurationselemente bei einem Austausch dem Inhalt (nicht Slot-spezifisch), während andere Konfigurationselemente nach einem Austausch beim Slot verbleiben (Slot-spezifisch). Im Folgenden ist die Konfiguration aufgeführt, die sich beim Austauschen der Slots ändert.

**Einstellungen, die ausgetauscht werden**:

* Allgemeine Einstellungen – z. B. Framework-Version, 32/64-Bit-Angabe, WebSockets
* App-Einstellungen (können so konfiguriert werden, dass sie beim Slot verbleiben)
* Verbindungszeichenfolgen (können so konfiguriert werden, dass sie beim Slot verbleiben)
* Handlerzuordnungen
* Überwachungs- und Diagnoseeinstellungen
* WebJobs-Inhalte

**Einstellungen, die nicht ausgetauscht werden**:

* Veröffentlichungsendpunkte
* Benutzerdefinierte Domänennamen
* SSL-Zertifikate und -Bindungen
* Skalierungseinstellungen
* WebJobs-Planer

Um eine App-Einstellung oder eine Verbindungszeichenfolge so zu konfigurieren, dass sie beim Slot verbleibt (nicht ausgetauscht wird), greifen Sie auf das Blatt **Anwendungseinstellungen** für einen bestimmten Slot zu, und aktivieren Sie dann das Kontrollkästchen **Slot-Einstellung** für die Konfigurationselemente, die beim Slot verbleiben sollten. Beachten Sie, dass das Markieren eines Konfigurationselements als slotspezifisch dafür sorgt, dass dieses Element in allen Bereitstellungsslots für die App als nicht austauschbar festgelegt wird.

![Slot-Einstellungen][SlotSettings]

<a name="Swap"></a>

## <a name="swap-deployment-slots"></a>Überführen von Bereitstellungsslots 
Auf dem Blatt „Ressourcen“ Ihrer App können Sie Bereitstellungsslots in der Ansicht **Übersicht** oder **Bereitstellungsslots** austauschen.

> [!IMPORTANT]
> Bevor Sie eine App aus einem Bereitstellungsslot in die Produktion überführen, stellen Sie sicher, dass alle nicht slotspezifischen Einstellungen im Austauschziel genau wie gewünscht konfiguriert sind.
> 
> 

1. Um Bereitstellungsslots auszutauschen, klicken Sie auf der Befehlsleiste der App oder der Befehlsleiste eines Bereitstellungsslots auf die Schaltfläche **Austauschen**.
   
    ![Schaltfläche Swap][SwapButtonBar]

2. Stellen Sie sicher, dass die Austauschquelle und das Austauschziel ordnungsgemäß festgelegt wurden. Normalerweise ist das Austauschziel ein Produktionsslot. Klicken Sie auf **OK** , um den Vorgang abzuschließen. Wenn der Vorgang abgeschlossen ist, wurden die Bereitstellungsslots ausgetauscht.

    ![Vollständiger Austausch](./media/web-sites-staged-publishing/SwapImmediately.png)

    Informationen zum Austauschtyp **Mit Vorschau austauschen** finden Sie unter [Mit Vorschau austauschen (Austausch mit mehreren Phasen)](#Multi-Phase).  

<a name="Multi-Phase"></a>

## <a name="swap-with-preview-multi-phase-swap"></a>Mit Vorschau austauschen (Austausch mit mehreren Phasen)

Das Feature „Mit Vorschau austauschen“ bzw. Austausch mit mehreren Phasen vereinfacht die Überprüfung slotspezifischer Konfigurationselemente, wie z.B. Verbindungszeichenfolgen.
Für unternehmenswichtige Workloads möchten Sie ggf. prüfen, ob sich die App wie erwartet verhält, wenn die Konfiguration des Produktionsslots angewendet wird. Eine solche Überprüfung müssen Sie durchführen, *ehe* die App in die Produktionsumgebung überführt wird. „Mit Vorschau austauschen“ ist das Feature, das Sie hierfür brauchen.

> [!NOTE]
> „Mit Vorschau austauschen“ wird in Web-Apps unter Linux nicht unterstützt.

Bei Verwendung der Option **Mit Vorschau austauschen** (siehe [Austauschen von Bereitstellungsslots](#Swap)) führt App Service Folgendes aus:

- Der Zielslot bleibt unverändert, sodass die in diesem Slot (z.B. Produktion) vorhandene Workload nicht beeinträchtigt wird.
- Die Konfigurationselemente des Zielslots werden auf den Quellslot angewendet, einschließlich der slotspezifischen Verbindungszeichenfolgen und App-Einstellungen.
- Die Workerprozesse für den Quellslot werden unter Verwendung der zuvor erwähnten Konfigurationselemente neu gestartet.
- Nach Abschluss des Austauschs wird der vorbereitete Quellslot in den Zielslot verschoben. Der Zielslot wird wie bei einem manuellen Austausch in den Quellslot verschoben.
- Wenn Sie den Austausch abbrechen, werden die Konfigurationselemente des Quellslots erneut auf den Quellslot angewendet.

Sie können eine Vorschau anzeigen, wie genau sich die App mit der Konfiguration des Zielslot verhält. Schließen Sie den Austausch nach Abschluss der Validierung in einem gesonderten Schritt ab. Dieser Schritt hat den zusätzlichen Vorteil, dass der Quellslot bereits mit der gewünschten Konfiguration vorbereitet ist, sodass auf den Clients keine Ausfallzeiten anfallen.  

Beispiele für Azure PowerShell-Cmdlets für den mehrstufigen Austausch sind im Abschnitt „Azure-PowerShell-Cmdlets für Bereitstellungsslots“ enthalten.

<a name="Auto-Swap"></a>

## <a name="configure-auto-swap"></a>Konfigurieren des automatischen Austauschs
Das Feature „Automatisch tauschen“ rationalisiert DevOps-Szenarien, bei denen Ihre App ständig ohne Kaltstarts und ohne Ausfallzeiten für Endkunden der App bereitgestellt werden soll. Wenn ein Bereitstellungsslot in der Produktion für „Automatisch tauschen“ konfiguriert wurde, überführt App Service bei jeder Codeaktualisierung die App per Push in die Produktion, nachdem sie bereits im Slot vorbereitet wurde.

> [!IMPORTANT]
> Wenn Sie Auto Swap für einen Slot aktivieren, stellen Sie sicher, dass die Slotkonfiguration genau der erforderlichen Konfiguration für den Zielslot (normalerweise der Produktionsslot) entspricht.
> 
> 

> [!NOTE]
> „Automatisch tauschen“ wird in Web-Apps unter Linux nicht unterstützt.

Das Konfigurieren von Auto Swap für einen Slot ist einfach. Führen Sie die folgenden Schritte aus:

1. Wählen Sie in **Bereitstellungsslots** einen Nicht-Produktionsslot und auf dem Blatt „Ressourcen“ dieses Slots **Anwendungseinstellungen** aus.  
   
    ![][Autoswap1]
2. Wählen Sie**Ein** für **Automatisch tauschen** sowie unter **Slot für automatischen Tausch** den gewünschten Zielslot aus, und klicken Sie auf der Befehlsleiste auf **Speichern**. Stellen Sie sicher, dass die Konfiguration für den Slot genau der für den Zielslot vorgesehenen Konfiguration entspricht.
   
    Auf der Registerkarte **Benachrichtigungen** wird in grüner Schrift der Text **ERFOLGREICH** angezeigt, sobald der Vorgang abgeschlossen wurde.
   
    ![][Autoswap2]
   
   > [!NOTE]
   > Um „Automatisch tauschen“ für Ihre App zu testen, können Sie zunächst unter **Slot für automatischen Tausch** einen Nicht-Produktionszielslot auswählen, um sich mit dem Feature vertraut zu machen.  
   > 
   > 
3. Führen Sie einen Code-Push für diesen Bereitstellungsslot aus. Auto Swap erfolgt nach kurzer Zeit, und die Aktualisierung wird an der URL des Zielslots wiedergegeben.

<a name="Rollback"></a>

## <a name="to-rollback-a-production-app-after-swap"></a>So setzen Sie eine Produktions-App nach dem Austausch wieder zurück
Wenn nach dieser Aktion Fehler in der Produktionswebsite feststellen, führen Sie ein Rollback in den Zustand vor dem Austausch aus, indem Sie dieselben beiden Slots sofort austauschen.

<a name="Warm-up"></a>

## <a name="custom-warm-up-before-swap"></a>Benutzerdefiniertes Aufwärmen vor dem Austausch
Einige Apps erfordern benutzerdefinierte Aufwärmaktionen. Über das Konfigurationselement `applicationInitialization` in der Datei „web.config“ können Sie benutzerdefinierte Initialisierungsaktionen angeben, die vor dem Empfang einer Anforderung ausgeführt werden. Der Austauschvorgang wartet dann, bis diese benutzerdefinierte Aufwärmphase abgeschlossen ist. Im Folgenden finden Sie ein Beispielfragment aus der Datei „Web.config“.

    <applicationInitialization>
        <add initializationPage="/" hostName="[app hostname]" />
        <add initializationPage="/Home/About" hostname="[app hostname]" />
    </applicationInitialization>

<a name="Delete"></a>

## <a name="to-delete-a-deployment-slot"></a>So löschen Sie einen Bereitstellungsslot
Klicken Sie auf dem geöffneten Blatt eines Bereitstellungsslots auf **Übersicht** (die Standardseite) und dann auf der Befehlsleiste auf **Löschen**.  

![Löschen eines Bereitstellungsslots][DeleteStagingSiteButton]

<!-- ======== AZURE POWERSHELL CMDLETS =========== -->

<a name="PowerShell"></a>

## <a name="azure-powershell-cmdlets-for-deployment-slots"></a>Azure PowerShell-Cmdlets für Bereitstellungsslots
Azure PowerShell ist ein Modul, das Cmdlets für die Verwaltung von Azure über Windows PowerShell bietet, einschließlich Unterstützung der Verwaltung von Bereitstellungsslots für Azure App Service.

* Informationen zum Installieren und Konfigurieren von Azure PowerShell sowie zur Authentifizierung von Azure PowerShell mit Ihrem Azure-Abonnement finden Sie unter [Installieren und Konfigurieren von Microsoft Azure PowerShell](/powershell/azure/overview).  

- - -
### <a name="create-a-web-app"></a>Erstellen einer Web-App
```
New-AzureRmWebApp -ResourceGroupName [resource group name] -Name [app name] -Location [location] -AppServicePlan [app service plan name]
```

- - -
### <a name="create-a-deployment-slot"></a>Erstellen eines Bereitstellungsslots
```
New-AzureRmWebAppSlot -ResourceGroupName [resource group name] -Name [app name] -Slot [deployment slot name] -AppServicePlan [app service plan name]
```

- - -
### <a name="initiate-a-swap-with-review-multi-phase-swap-and-apply-destination-slot-configuration-to-source-slot"></a>Einleiten eines Austauschs mit Überprüfung (Austausch in mehreren Phasen) und Anwenden der Konfiguration des Zielslots auf den Quellslot
```
$ParametersObject = @{targetSlot  = "[slot name – e.g. “production”]"}
Invoke-AzureRmResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [app name]/[slot name] -Action applySlotConfig -Parameters $ParametersObject -ApiVersion 2015-07-01
```

- - -
### <a name="cancel-a-pending-swap-swap-with-review-and-restore-source-slot-configuration"></a>Abbrechen eines ausstehenden Austauschs (Austausch mit Überprüfung) und Wiederherstellen der Konfiguration des Quellslots
```
Invoke-AzureRmResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [app name]/[slot name] -Action resetSlotConfig -ApiVersion 2015-07-01
```

- - -
### <a name="swap-deployment-slots"></a>Überführen von Bereitstellungsslots
```
$ParametersObject = @{targetSlot  = "[slot name – e.g. “production”]"}
Invoke-AzureRmResourceAction -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots -ResourceName [app name]/[slot name] -Action slotsswap -Parameters $ParametersObject -ApiVersion 2015-07-01
```

- - -
### <a name="delete-deployment-slot"></a>Löschen von Bereitstellungsslots
```
Remove-AzureRmResource -ResourceGroupName [resource group name] -ResourceType Microsoft.Web/sites/slots –Name [app name]/[slot name] -ApiVersion 2015-07-01
```

- - -
<!-- ======== Azure CLI =========== -->

<a name="CLI"></a>

## <a name="azure-command-line-interface-azure-cli-commands-for-deployment-slots"></a>Befehle der Azure-Befehlszeilenschnittstelle (Azure-CLI) für Bereitstellungsslots
Die Azure CLI bietet plattformübergreifende Befehle für das Arbeiten mit Azure, einschließlich Unterstützung der Verwaltung von App Service-Bereitstellungsslots.

* Anweisungen zur Installation und Konfiguration der Azure-Befehlszeilenschnittstelle, einschließlich Informationen zur Verbindung der Azure-Befehlszeilenschnittstelle mit Ihrem Azure-Abonnement, finden Sie unter [Installieren und Konfigurieren der Azure-Befehlszeilenschnittstelle](../cli-install-nodejs.md).
* Um eine Liste der verfügbaren Befehle für Azure App Service in der Azure-Befehlszeilenschnittstelle zu erhalten, rufen Sie `azure site -h`auf.

> [!NOTE] 
> Informationen zu [Azure CLI 2.0](https://github.com/Azure/azure-cli)-Befehlen für Bereitstellungsslots finden Sie unter [Azure App Service-Webbereitstellungsslot](/cli/azure/appservice/web/deployment/slot).

- - -
### <a name="azure-site-list"></a>azure site list
Um Informationen zu den Apps im aktuellen Abonnement zu erhalten, rufen Sie, wie in folgendem Beispiel, **azure site list** auf.

`azure site list webappslotstest`

- - -
### <a name="azure-site-create"></a>azure site create
Um einen Bereitstellungsslot zu erstellen, rufen Sie **azure site create** auf, und geben Sie den Namen einer vorhandenen App und den Namen des zu erstellenden Slots wie im folgenden Beispiel an.

`azure site create webappslotstest --slot staging`

Um die Quellcodeverwaltung für den neuen Steckplatz zu aktivieren, verwenden Sie die Option **--git** wie in folgendem Beispiel.

`azure site create --git webappslotstest --slot staging`

- - -
### <a name="azure-site-swap"></a>azure site swap
Um den aktualisierten Bereitstellungsslot zur Produktions-App zu machen, verwenden Sie für die Überführung den Befehl **azure site swap** wie im folgenden Beispiel. Die Produktions-App weist keine Ausfallzeiten auf und durchläuft keinen Kaltstart.

`azure site swap webappslotstest`

- - -
### <a name="azure-site-delete"></a>azure site delete
Um einen nicht mehr benötigten Bereitstellungsslot zu löschen, verwenden Sie wie im folgenden Beispiel den Befehl **azure site delete** .

`azure site delete webappslotstest --slot staging`

- - -
> [!NOTE]
> Sehen Sie sich eine Web-App in Aktion an: [Probieren Sie App Service gleich aus](https://azure.microsoft.com/try/app-service/), und erstellen Sie eine kurzzeitige Start-App – ohne Kreditkarte oder weitere Verpflichtungen.
> 
> 

## <a name="next-steps"></a>Nächste Schritte
[Azure Web Sites – block web access to non-production deployment slots (Azure Web Sites – Blockieren des Webzugriffs auf nicht für die Produktion bestimmte Bereitstellungsslots)](http://ruslany.net/2014/04/azure-web-sites-block-web-access-to-non-production-deployment-slots/)
[Einführung in App Service unter Linux](./app-service-linux-intro.md)
[Kostenlose Microsoft Azure-Testversion](https://azure.microsoft.com/pricing/free-trial/)

<!-- IMAGES -->
[QGAddNewDeploymentSlot]:  ./media/web-sites-staged-publishing/QGAddNewDeploymentSlot.png
[AddNewDeploymentSlotDialog]: ./media/web-sites-staged-publishing/AddNewDeploymentSlotDialog.png
[ConfigurationSource1]: ./media/web-sites-staged-publishing/ConfigurationSource1.png
[MultipleConfigurationSources]: ./media/web-sites-staged-publishing/MultipleConfigurationSources.png
[SiteListWithStagedSite]: ./media/web-sites-staged-publishing/SiteListWithStagedSite.png
[StagingTitle]: ./media/web-sites-staged-publishing/StagingTitle.png
[SwapButtonBar]: ./media/web-sites-staged-publishing/SwapButtonBar.png
[SwapConfirmationDialog]:  ./media/web-sites-staged-publishing/SwapConfirmationDialog.png
[DeleteStagingSiteButton]: ./media/web-sites-staged-publishing/DeleteStagingSiteButton.png
[SwapDeploymentsDialog]: ./media/web-sites-staged-publishing/SwapDeploymentsDialog.png
[Autoswap1]: ./media/web-sites-staged-publishing/AutoSwap01.png
[Autoswap2]: ./media/web-sites-staged-publishing/AutoSwap02.png
[SlotSettings]: ./media/web-sites-staged-publishing/SlotSetting.png



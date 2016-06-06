<properties 
	pageTitle="Verschieben von Ressourcen in eine neue Ressourcengruppe | Microsoft Azure" 
	description="Verwenden Sie Azure Resource Manager, um Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement verschieben." 
	services="azure-resource-manager" 
	documentationCenter="" 
	authors="tfitzmac" 
	manager="timlt" 
	editor="tysonn"/>

<tags 
	ms.service="azure-resource-manager" 
	ms.workload="multiple" 
	ms.tgt_pltfrm="na" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="05/12/2016" 
	ms.author="tomfitz"/>

# Verschieben von Ressourcen in eine neue Ressourcengruppe oder ein neues Abonnement

In diesem Thema erfahren Sie, wie Sie Ressourcen aus einer Ressourcengruppe in eine andere Ressourcengruppe verschieben. Sie können Ressourcen auch in ein neues Abonnement verschieben (es muss jedoch im gleichen [Mandanten](./active-directory/active-directory-howto-tenant.md) vorhanden sein). Möglicherweise müssen Sie Ressourcen aus Folgenden Gründen verschieben:

1. Zu Abrechnungszwecken muss eine Ressource sich in einem anderen Abonnement befinden.
2. Eine Ressource hat nicht mehr den gleichen Lebenszyklus wie die Ressourcen, mit denen sie zuvor gruppiert wurde. Sie möchten sie in eine neue Ressourcengruppe verschieben, damit Sie diese Ressource separat von anderen Ressourcen verwalten können.

Beim Verschieben von Ressourcen werden die Quellgruppe und die Zielgruppe für die Dauer des Vorgangs gesperrt. Schreib- und Löschvorgänge in den Gruppen werden bis zum Abschluss der Verschiebung blockiert.

Sie können nicht den Speicherort der Ressource ändern. Wenn Sie ein Ressource verschieben, wird sie nur in eine neue Ressourcengruppe verschoben. Die neue Ressourcengruppe hat möglicherweise einen anderen Speicherort, das heißt jedoch nicht, dass der Speicherort der Ressource geändert wird.

## Checkliste vor dem Verschieben von Ressourcen

Beim Verschieben einer Ressource sollten Sie einige wichtige Schritte ausführen: Indem Sie diese Bedingungen überprüfen, können Sie Fehler vermeiden.

1. Der Dienst muss die Möglichkeit unterstützen, Ressourcen zu verschieben. In der Liste unten finden Sie Informationen, welche [Dienste das Verschieben von Ressourcen unterstützen](#services-that-support-move).
2. Das Zielabonnement muss für den Ressourcenanbieter der verschobenen Ressource registriert sein. Andernfalls erhalten Sie eine Fehlermeldung, die besagt, dass das **Abonnement nicht für einen Ressourcentyp registriert ist**. Dieses Problem kann auftreten, wenn eine Ressource zu einem neuen Abonnement verschoben wird, dieses aber noch nie mit diesem Ressourcentyp verwendet wurde. Weitere Informationen zum Überprüfen des Registrierungsstatus und zum Registrieren von Ressourcenanbietern finden Sie unter [Ressourcenanbieter und -typen](../resource-manager-supported-services.md#resource-providers-and-types).
3. Wenn Sie Azure PowerShell oder die Azure-Befehlszeilenschnittstelle verwenden, verwenden Sie die neueste Version. Führen Sie zum Aktualisieren Ihrer Version den Microsoft-Webplattform-Installer aus, und überprüfen Sie, ob eine neue Version verfügbar ist. Weitere Informationen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](powershell-install-configure.md) und [Installieren der Azure-Befehlszeilenschnittstelle](xplat-cli-install.md).
4. Wenn Sie eine App Service-App verschieben, müssen Sie die [App Service-Einschränkungen](#app-service-limitations) kennen.
5. Wenn Sie Ressourcen verschieben, die Sie mithilfe des klassischen Bereitstellungsmodells bereitgestellt haben, müssen Sie [Einschränkungen bei der klassischen Bereitstellung](#classic-deployment-limitations) kennen.

## Dienste, die das Verschieben unterstützen

Derzeit unterstützen die folgenden Dienste das Verschieben in eine neue Ressourcengruppe und ein neues Abonnement:

- API Management
- App Service-Apps – siehe [App Service-Einschränkungen](#app-service-limitations)
- Automation
- Batch
- CDN
- Cloud Services – siehe [Einschränkungen bei der klassischen Bereitstellung](#classic-deployment-limitations)
- Data Factory
- DNS
- DocumentDB
- HDInsight-Cluster
- Schlüsseltresor
- Mobile Engagement
- Notification Hubs
- Operational Insights
- Redis-Cache
- Scheduler
- Suche
- Storage (klassisch) – siehe [Einschränkungen bei der klassischen Bereitstellung](#classic-deployment-limitations)
- SQL-Datenbankserver – die Datenbank und der Server müssen sich in derselben Ressourcengruppe befinden. Wenn Sie eine SQL Server-Instanz verschieben, werden auch alle zugehörigen Datenbanken verschoben.
- Virtual Machines (klassisch) – siehe [Einschränkungen bei der klassischen Bereitstellung](#classic-deployment-limitations)

## Dienste, die das Verschieben teilweise unterstützen

Die folgenden Dienste unterstützen das Verschieben in eine neue Ressourcengruppe, jedoch nicht in ein neues Abonnement:

- Virtuelle Netzwerke

## Dienste, die das Verschieben nicht unterstützen

Die folgenden Dienste unterstützen derzeit nicht das Verschieben einer Ressource:

- Application Insights
- ExpressRoute
- Speicher
- Virtual Machines
- Virtual Networks (klassisch) – siehe [Einschränkungen bei der klassischen Bereitstellung](#classic-deployment-limitations)

## App Service-Einschränkungen

Bei der Arbeit mit App Service-Apps können Sie nicht nur einen App Service-Plan verschieben. Zum Verschieben von App Service-Apps stehen folgende Optionen bereit:

- Verschieben Sie den App Service-Plan und alle anderen App Service-Ressourcen in dieser Ressourcengruppe in eine neue Ressourcengruppe, die noch nicht über App Service-Ressourcen verfügt. Das bedeutet auch das Verschieben von App Service-Ressourcen, die nicht dem App Service-Plan zugeordnet sind. 
- Verschieben Sie Apps in eine andere Ressourcengruppe, behalten Sie jedoch alle App Services-Pläne in der ursprünglichen Ressourcengruppe bei.

Wenn die ursprüngliche Ressourcengruppe auch eine Application Insights-Ressource enthält, können Sie diese Ressource nicht verschieben, da Application Insights das Verschieben derzeit nicht unterstützt. Wenn Sie die Application Insights-Ressource beim Verschieben von App Service-Apps einschließen, misslingt der gesamte Verschiebevorgang. Der Application Insights- und App Service-Plan müssen sich allerdings nicht in derselben Ressourcengruppe wie die App befinden, damit die App ordnungsgemäß funktioniert.

Angenommen, Ihre Ressourcengruppe enthält folgende Ressourcen:

- **web-a**, die **plan-a** und **app-insights-a** zugeordnet ist
- **web-b**, die **plan-b** und **app-insights-b** zugeordnet ist

Die Optionen sind wie folgt:

- Verschieben von **web-a**, **plan-a**, **web-b** und **plan-b**
- Verschieben von **web-a** und **web-b**
- Verschieben von **web-a**
- Verschieben von **web-b**

Allen anderen Kombinationen umfassen entweder das Verschieben einer Ressource, die nicht verschoben werden kann (Application Insights), oder das Hinterlassen eines Ressourcentyps, der beim Verschieben eines App Service-Plans (beliebiger Typ von App Service-Ressource) nicht hinterlassen werden darf.

Wenn sich Ihre Web-App in einer anderen Ressourcengruppe als ihr App Service-Plan befindet, aber Sie beide in eine neue Ressourcengruppe verschieben möchten, muss das Verschieben in zwei Schritten erfolgen. Beispiel:

- **web-a** befindet sich in **web-group**.
- **plan-a** befindet sich in **plan-group**.
- Sie möchten, dass sich **web-a** und **plan-a** in **combined-group** befinden.

Um dies zu erreichen, führen Sie zwei getrennte Verschiebevorgänge in der folgenden Reihenfolge aus:

1. Verschieben Sie **web-a** in **plan-group**.
2. Verschieben Sie **web-a** und **plan-a** in **combined-group**.

## Einschränkungen bei der klassischen Bereitstellung

Die Optionen zum Verschieben von Ressourcen, die über das klassische Modell bereitgestellt wurden, unterscheiden sich abhängig davon, ob Sie die Ressourcen innerhalb eines Abonnements oder in ein neues Abonnement verschieben.

Beim Verschieben von Ressourcen aus einer Ressourcengruppe in eine andere **innerhalb desselben Abonnements** gelten die folgenden Einschränkungen:

- Virtuelle Netzwerke (klassisch) können nicht verschoben werden.
- Virtuelle Computer (klassisch) müssen mit dem Clouddienst verschoben werden. 
- Der Clouddienst kann nur verschoben werden, wenn der Verschiebevorgang alle dazugehörigen virtuellen Computer umfasst.
- Es kann immer nur ein Clouddienst gleichzeitig verschoben werden.
- Es kann immer nur ein Speicherkonto (klassisch) gleichzeitig verschoben werden.
- Ein Speicherkonto (klassisch) kann nicht im selben Vorgang mit einem virtuellen Computer oder Clouddienst verschoben werden.

Beim Verschieben von Ressourcen in ein **neues Abonnement** gelten die folgenden Einschränkungen:

- Alle klassische Ressourcen im Abonnement müssen im selben Vorgang verschoben werden.
- Der Verschiebevorgang kann nur über das Portal oder über eine separate REST-API für klassisches Verschieben angefordert werden. Die Resource Manager-Standardbefehle für das Verschieben funktionieren beim Verschieben klassischer Ressourcen in ein neues Abonnement nicht. In den folgenden Abschnitten werden die Schritte für das Portal oder die REST-API gezeigt.

## Verschieben von Ressourcen über das Portal

Sie können einige Ressourcen über das Portal verschieben. Allerdings werden nicht alle Ressourcenanbieter, die den Vorgang unterstützen, diese Funktion über das Portal bereitstellen.

Um eine Ressource zu verschieben, wählen Sie die Ressource aus, und klicken Sie dann auf die Schaltfläche **Verschieben**.

![Ressourcen verschieben](./media/resource-group-move-resources/move-resources.png)

Beim Verschieben der Ressourcen geben Sie das Zielabonnement und die Ressourcengruppe an. Wenn andere Ressourcen mit der Ressource verschoben werden müssen, werden diese aufgeführt.

![Ziel auswählen](./media/resource-group-move-resources/select-destination.png)

In **Benachrichtigungen** wird angezeigt, dass der Verschiebevorgang ausgeführt wird.

![Verschiebestatus anzeigen](./media/resource-group-move-resources/show-status.png)

Sobald der Vorgang abgeschlossen ist, werden Sie über das Ergebnis informiert.

![Ergebnis des Verschiebens anzeigen](./media/resource-group-move-resources/show-result.png)

Wählen Sie für eine andere Möglichkeit zum Verschieben von Ressourcen in eine neue Ressourcengruppe (nicht in ein Abonnement) die zu verschiebende Ressource aus.

![Zu verschiebende Ressourcen auswählen](./media/resource-group-move-resources/select-resource.png)

Wählen Sie deren **Eigenschaften** aus.

![Eigenschaften auswählen](./media/resource-group-move-resources/select-properties.png)

Falls für diesen Ressourcentyp verfügbar, wählen Sie **Ressourcengruppe ändern** aus.

![Ressourcengruppe ändern](./media/resource-group-move-resources/change-resource-group.png)

Sie können die zu verschiebenden Ressourcen und die Ressourcengruppe auswählen, in die das Verschieben erfolgen soll.

![Ressourcen verschieben](./media/resource-group-move-resources/select-group.png)

Beim Verschieben von Ressourcen, die über das klassische Modell bereitgestellt wurden, in eine neue Ressourcengruppe können Sie das Symbol „Bearbeiten“ neben dem Namen der Ressourcengruppe verwenden.

![Klassische Ressourcen verschieben](./media/resource-group-move-resources/edit-rg-icon.png)

Wählen Sie unter Berücksichtigung der [Einschränkungen bei der klassischen Bereitstellung](#classic-deployment-limitations) die zu verschiebenden Ressourcen aus. Klicken Sie auf **OK**, um mit dem Verschieben zu beginnen.

 ![Klassische Ressourcen auswählen](./media/resource-group-move-resources/select-classic-resources.png)
 
 Beim Verschieben von Ressourcen, die über das klassische Modell bereitgestellt wurden, in ein neues Abonnement können Sie das Symbol „Bearbeiten“ neben dem Namen des Abonnements verwenden.
 
 ![In neues Abonnement verschieben](./media/resource-group-move-resources/edit-subscription-icon.png)
 
 Alle klassischen Ressourcen werden automatisch für das Verschieben ausgewählt.

## Verschieben von Ressourcen mithilfe von PowerShell

Verwenden Sie zum Verschieben vorhandener Ressourcen in eine andere Ressourcengruppe oder ein anderes Abonnement den Befehl **Move-AzureRmResource**.

Das erste Beispiel zeigt, wie Sie eine Ressource in eine neue Ressourcengruppe verschieben.

    $resource = Get-AzureRmResource -ResourceName ExampleApp -ResourceGroupName OldRG
    Move-AzureRmResource -DestinationResourceGroupName NewRG -ResourceId $resource.ResourceId

Im zweiten Beispiel wird veranschaulicht, wie mehrere Ressourcen in eine neue Ressourcengruppe verschoben werden.

    $webapp = Get-AzureRmResource -ResourceGroupName OldRG -ResourceName ExampleSite
    $plan = Get-AzureRmResource -ResourceGroupName OldRG -ResourceName ExamplePlan
    Move-AzureRmResource -DestinationResourceGroupName NewRG -ResourceId $webapp.ResourceId, $plan.ResourceId

Um Ressourcen in ein neues Abonnement zu verschieben, schließen Sie einen Wert für den **DestinationSubscriptionId**-Parameter ein.

Sie werden aufgefordert zu bestätigen, dass die angegebenen Ressourcen verschoben werden soll.

    Confirm
    Are you sure you want to move these resources to the resource group
    '/subscriptions/{guid}/resourceGroups/newRG' the resources:

    /subscriptions/{guid}/resourceGroups/destinationgroup/providers/Microsoft.Web/serverFarms/exampleplan
    /subscriptions/{guid}/resourceGroups/destinationgroup/providers/Microsoft.Web/sites/examplesite
    [Y] Yes  [N] No  [S] Suspend  [?] Help (default is "Y"): y

## Verwenden der Azure-Befehlszeilenschnittstelle zum Verschieben von Ressourcen

Verwenden Sie zum Verschieben vorhandener Ressourcen in eine andere Ressourcengruppe oder ein anderes Abonnement den Befehl **azure resource move**. Das folgende Beispiel zeigt, wie Sie ein Redis-Cache in eine neue Ressourcengruppe verschieben. Geben Sie im **-i**-Parameter eine durch Kommas getrennte Liste der zu verschiebenden Ressourcen-IDs an.

    azure resource move -i "/subscriptions/{guid}/resourceGroups/OldRG/providers/Microsoft.Cache/Redis/examplecache" -d "NewRG"
	
Sie werden aufgefordert zu bestätigen, dass die angegebene Ressource verschoben werden soll.
	
    info:    Executing command resource move
    Move selected resources in OldRG to NewRG? [y/n] y
    + Moving selected resources to NewRG
    info:    resource move command OK

## Verschieben von Ressourcen mithilfe der REST-API

Führen Sie zum Verschieben vorhandener Ressourcen in eine andere Ressourcengruppe oder ein anderes Abonnement Folgendes aus:

    POST https://management.azure.com/subscriptions/{source-subscription-id}/resourcegroups/{source-resource-group-name}/moveResources?api-version={api-version} 

Geben Sie im Anforderungstext die Zielgruppe und die zu verschiebenden Ressourcen an. Weitere Informationen zur REST-Verschiebung finden Sie unter [Verschieben von Ressourcen](https://msdn.microsoft.com/library/azure/mt218710.aspx).

Zum Verschieben von **klassischen Ressourcen in ein neues Abonnement** müssen Sie jedoch unterschiedliche REST-Vorgänge ausführen. Um zu überprüfen, ob ein Abonnement als Quell- oder Zielabonnement an einer abonnementübergreifenden Verschiebung klassischer Ressourcen teilnehmen kann, führen Sie den folgenden Vorgang aus:

    POST https://management.azure.com/subscriptions/{subscriptionId}/providers/Microsoft.ClassicCompute/validateSubscriptionMoveAvailability?api-version=2016-04-01
    
Verwenden Sie für das Quellabonnement den Anforderungstext:

    {
        "role": "source"
    }

Verwenden Sie für das Zielabonnement den Anforderungstext:

    {
        "role": "target"
    }

Die Antwort auf beide Überprüfungsvorgänge ist:

    {
        "status": "{status}",
        "reasons": [
            "reason1",
            "reason2"
        ]
    }

Um alle klassischen Ressourcen aus einem Abonnement in ein anderes Abonnement zu verschieben, führen Sie den folgenden Vorgang aus:

    POST https://management.azure.com/subscriptions/{subscription-id}/providers/Microsoft.ClassicCompute/moveSubscriptionResources?api-version=2016-04-01

Mit dem Anforderungstext:

    {
        "target": "/subscriptions/{target-subscription-id}"
    }



## Nächste Schritte
- Weitere Informationen zu PowerShell-Cmdlets zum Verwalten Ihres Abonnements finden Sie unter [Verwenden von Azure PowerShell mit Resource Manager](powershell-azure-resource-manager.md).
- Weitere Informationen zu Befehlen der Azure-Befehlszeilenschnittstelle zum Verwalten Ihres Abonnements finden Sie unter [Verwenden der Azure-Befehlszeilenschnittstelle mit Resource Manager](xplat-cli-azure-resource-manager.md).
- Weitere Informationen zu Portalfeatures zum Verwalten Ihres Abonnements finden Sie unter [Verwenden des Azure-Portals zum Verwalten von Ressourcen](./azure-portal/resource-group-portal.md).
- Informationen zum Anwenden einer logischen Organisation auf Ihre Ressourcen finden Sie unter [Verwenden von Tags zum Organisieren von Ressourcen](resource-group-using-tags.md).

<!---HONumber=AcomDC_0525_2016-->
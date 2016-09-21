### Tag-Cmdlet ist in der neuesten Version von PowerShell geändert

Die August 2016-Version von [Azure PowerShell 2.0][powershell] enthält wichtige Änderungen bezüglich der Art und Weise, wie Sie mit Tags arbeiten. Bevor Sie fortfahren, überprüfen Sie Ihre Version des Moduls AzureRm.Resources.

    Get-Module -ListAvailable -Name AzureRm.Resources | Select Version

Wenn Sie Ihr Azure PowerShell zuletzt vor August 2016 aktualisiert haben, sollte Ihre Überprüfung eine kleinere Version als 3.0 ergeben.

    Version
    -------
    2.0.2

Wenn Sie Ihr Azure PowerShell seit August 2016 aktualisiert haben, sollte Ihre Überprüfung eine 3.0-Version ergeben.

    Version
    -------
    3.0.1
    
Wenn Ihre Version des Moduls 3.0.1 oder höher ist, haben Sie die neuesten Cmdlets für die Arbeit mit Tags. Diese Version des Azure-Ressourcen-Moduls wird beim Installieren oder Aktualisieren von Azure PowerShell mithilfe von PowerShell-Katalog, PowerShellGet oder Webplattform-Installer automatisch installiert. Ist Ihre Version niedriger als 3.0.1, können Sie weiterhin diese Version verwenden, aber Sie sollten erwägen, auf die neueste Version zu aktualisieren. Die neueste Version umfasst Änderungen, die das Arbeiten mit Tags erleichtern. In diesem Thema werden beide Ansätze beschrieben.

### Aktualisieren Ihres Skripts auf Änderungen in der neuesten Version 

In der neuesten Version wurde der Name des Parameters **Tags** in **Tag** geändert und der Typ von **Hashtable** in **Hashtable**. Sie müssen nicht mehr **Name** und **Value** für jeden Eintrag angeben. Stattdessen geben Sie Schlüssel/Wert-Paare im Format **Schlüssel = „Wert“** an.

Um vorhandene Skripts zu aktualisieren, ändern Sie den Parameter **Tags** in **Tag**, und ändern Sie das Tagformat, wie im folgenden Beispiel gezeigt.

    # Old
    New-AzureRmResourceGroup -Tags @{ Name = "testtag"; Value = "testval" } -Name $resourceGroupName -Location $location

    # New
    New-AzureRmResourceGroup -Tag @{ testtag = "testval" } -Name $resourceGroupName -Location $location 

Sie müssen jedoch beachten, dass Ressourcengruppen und Ressourcen in ihren Metadaten immer noch eine Eigenschaft **Tags** zurückgeben. Diese Eigenschaft wird nicht geändert.

### Version 3.0.1 oder höher

Tags sind direkt in Ressourcen und Ressourcengruppen vorhanden. Um die vorhandenen Tags anzuzeigen, rufen Sie eine Ressource mit **Get-AzureRmResource** oder eine Ressourcengruppe mit **Get-AzureRmResourceGroup** ab.

Beginnen wir mit einer Ressourcengruppe.

    Get-AzureRmResourceGroup -Name testrg1

Dieses Cmdlet gibt mehrere Teile der Metadaten für die Ressourcengruppe zurück, einschließlich welche Tags ggf. angewendet wurden.

    ResourceGroupName : testrg1
    Location          : westus
    ProvisioningState : Succeeded
    Tags              :
                    Name         Value
                    ===========  ==========
                    Dept         Finance
                    Environment  Production

Orientieren Sie sich beim Abrufen der Ressourcenmetadaten einschließlich Tags an folgendem Beispiel.

    Get-AzureRmResource -ResourceName tfsqlserver -ResourceGroupName testrg1

Die Tagnamen werden in den Ergebnissen angezeigt.

    Name              : tfsqlserver
    ResourceId        : /subscriptions/{guid}/resourceGroups/tag-demo-group/providers/Microsoft.Sql/servers/tfsqlserver
    ResourceName      : tfsqlserver
    ResourceType      : Microsoft.Sql/servers
    Kind              : v12.0
    ResourceGroupName : testrg1
    Location          : westus
    SubscriptionId    : {guid}
    Tags              : {Dept, Environment}

Verwenden Sie die Eigenschaft **Tags** zum Abrufen von Tagnamen und -werten.

    (Get-AzureRmResource -ResourceName tfsqlserver -ResourceGroupName testrg1).Tags

Hiermit werden folgende Ergebnisse zurückgegeben:

    Name                   Value
    ----                   -----
    Dept                   Finance
    Environment            Production

Anstatt die Tags für eine bestimmte Ressourcengruppe oder Ressource anzuzeigen, möchten Sie wahrscheinlich häufig alle Ressourcen oder Ressourcengruppen abrufen, die ein bestimmtes Tag und einen bestimmten Wert aufweisen. Um Ressourcengruppen mit einem bestimmten Tag abzurufen, verwenden Sie das Cmdlet **Find-AzureRmResourceGroup** mit dem Parameter **-Tag**.

Um Ressourcengruppen mit einem Tagwert abzurufen, verwenden Sie das folgende Format.

    (Find-AzureRmResourceGroup -Tag @{ Dept="Finance" }).Name 

Verwenden Sie zum Abrufen aller Ressourcen mit einem bestimmten Tag und Wert das Cmdlet **Find-AzureRmResource**.

    (Find-AzureRmResource -TagName Dept -TagValue Finance).Name
    
Um einer Ressourcengruppe, die über keine Tags verfügt, ein Tag hinzuzufügen, verwenden Sie den Befehl **Set-AzureRmResourceGroup**, und geben Sie ein Tagobjekt an.

    Set-AzureRmResourceGroup -Name test-group -Tag @{ Dept="IT"; Environment="Test" }

So wird die Ressourcengruppe mit den neuen Tagwerten zurückgegeben.

    ResourceGroupName : test-group
    Location          : southcentralus
    ProvisioningState : Succeeded
    Tags              :
                    Name          Value
                    =======       =====
                    Dept          IT
                    Environment   Test
                    
Mit dem Befehl **Set-AzureRmResource** können Sie einer Ressource, die über keine Tags verfügt, Tags hinzufügen.

    Set-AzureRmResource -Tag @{ Dept="IT"; Environment="Test" } -ResourceId /subscriptions/{guid}/resourceGroups/test-group/providers/Microsoft.Web/sites/examplemobileapp

Tags werden als Ganzes aktualisiert. Um ein Tag einer Ressource hinzuzufügen, die bereits über andere Tags verfügt, verwenden Sie ein Array mit allen Tags, die Sie behalten möchten. Wählen Sie zunächst die vorhandenen Tags, fügen Sie einen dieser Gruppe hinzu, und wenden Sie alle Tags erneut an.

    $tags = (Get-AzureRmResourceGroup -Name tag-demo).Tags
    $tags += @{Status="approved"}
    Set-AzureRmResourceGroup -Name test-group -Tag $tags

Um ein Tag oder mehrere Tags zu entfernen, speichern Sie einfach das Array ohne die Tags, die Sie entfernen möchten.

Der Prozess ist für Ressourcen identisch, Sie verwenden jedoch die Cmdlets **Get-AzureRmResource** und **Set-AzureRmResource**.

Verwenden Sie das Cmdlet **Get-AzureRmTag**, um mithilfe von PowerShell eine Liste aller Markierungen innerhalb eines Abonnements abzurufen.

    Get-AzureRmTag
    
Die Tagnamen und die Anzahl von Ressourcen und Ressourcengruppen werden mit dem Tag zurückgegeben.

    Name                      Count
    ----                      ------
    Dept                       8
    Environment                8

Möglicherweise sehen Sie Tags, die mit "hidden-" und "link:" beginnen. Hierbei handelt es sich um interne Tags, die Sie ignorieren und nicht ändern sollten.

Verwenden Sie das Cmdlet **New-AzureRmTag**, um der Taxonomie neue Markierungen hinzuzufügen. Diese Tags werden in die AutoVervollständigen-Funktion eingeschlossen, obwohl sie noch nicht auf Ressourcen oder Ressourcengruppen angewendet wurden. Um einen Markierungsnamen/Markierungswert zu entfernen, entfernen Sie zuerst die Markierung aus allen Ressourcen, mit denen es möglicherweise verwendet wird, und entfernen Sie es dann mit dem Cmdlet **Remove-AzureRmTag** aus der Taxonomie.

### Versionen vor 3.0.1

Tags sind direkt in Ressourcen und Ressourcengruppen vorhanden. Um die vorhandenen Tags anzuzeigen, rufen Sie eine Ressource mit **Get-AzureRmResource** oder eine Ressourcengruppe mit **Get-AzureRmResourceGroup** ab.

Beginnen wir mit einer Ressourcengruppe.

    Get-AzureRmResourceGroup -Name testrg1

Dieses Cmdlet gibt mehrere Teile der Metadaten für die Ressourcengruppe zurück, einschließlich welche Tags ggf. angewendet wurden.

    ResourceGroupName : testrg1
    Location          : westus
    ProvisioningState : Succeeded
    Tags              :
                    Name         Value
                    ===========  ==========
                    Dept         Finance
                    Environment  Production
                    
Orientieren Sie sich beim Abrufen der Ressourcenmetadaten an folgendem Beispiel. Die Ressourcenmetadaten zeigen nicht direkt Tags an.

    Get-AzureRmResource -ResourceName tfsqlserver -ResourceGroupName testrg1

In den Ergebnissen sehen Sie, dass die Tags nur als Hashtableobjekt angezeigt werden.

    Name              : tfsqlserver
    ResourceId        : /subscriptions/{guid}/resourceGroups/tag-demo-group/providers/Microsoft.Sql/servers/tfsqlserver
    ResourceName      : tfsqlserver
    ResourceType      : Microsoft.Sql/servers
    Kind              : v12.0
    ResourceGroupName : tag-demo-group
    Location          : westus
    SubscriptionId    : {guid}
    Tags              : {System.Collections.Hashtable}

Die eigentlichen Tags können Sie durch Abrufen der **Tags**-Eigenschaft anzeigen.

    (Get-AzureRmResource -ResourceName tfsqlserver -ResourceGroupName tag-demo-group).Tags | %{ $_.Name + ": " + $_.Value }
   
So werden formatierte Ergebnisse zurückgegeben:
    
    Dept: Finance
    Environment: Production
    
Anstatt die Tags für eine bestimmte Ressourcengruppe oder Ressource anzuzeigen, möchten Sie wahrscheinlich häufig alle Ressourcen oder Ressourcengruppen abrufen, die ein bestimmtes Tag und einen bestimmten Wert aufweisen. Um Ressourcengruppen mit einem bestimmten Tag abzurufen, verwenden Sie das Cmdlet **Find-AzureRmResourceGroup** mit dem Parameter **-Tag**.

Um Ressourcengruppen mit einem Tagwert abzurufen, verwenden Sie das folgende Format.

    Find-AzureRmResourceGroup -Tag @{ Name="Dept"; Value="Finance" } | %{ $_.Name }
    
Verwenden Sie zum Abrufen aller Ressourcen mit einem bestimmten Tag und Wert das Cmdlet Find-AzureRmResource.

    Find-AzureRmResource -TagName Dept -TagValue Finance | %{ $_.ResourceName }

Um einer Ressourcengruppe, die über keine Tags verfügt, ein Tag hinzuzufügen, verwenden Sie einfach den Befehl Set-AzureRmResourceGroup, und geben Sie ein Tagobjekt an.

    Set-AzureRmResourceGroup -Name test-group -Tag @( @{ Name="Dept"; Value="IT" }, @{ Name="Environment"; Value="Test"} )
    
So wird die Ressourcengruppe mit den neuen Tagwerten zurückgegeben.

    ResourceGroupName : test-group
    Location          : southcentralus
    ProvisioningState : Succeeded
    Tags              :
                Name          Value
                =======       =====
                Dept          IT
                Environment   Test

Mit dem Befehl Set-AzureRmResource können Sie einer Ressource, die über keine Tags verfügt, Tags hinzufügen.

    Set-AzureRmResource -Tag @( @{ Name="Dept"; Value="IT" }, @{ Name="Environment"; Value="Test"} ) -ResourceId /subscriptions/{guid}/resourceGroups/test-group/providers/Microsoft.Web/sites/examplemobileapp

Tags werden als Ganzes aktualisiert. Um ein Tag einer Ressource hinzuzufügen, die bereits über andere Tags verfügt, verwenden Sie ein Array mit allen Tags, die Sie behalten möchten. Wählen Sie zunächst die vorhandenen Tags, fügen Sie einen dieser Gruppe hinzu, und wenden Sie alle Tags erneut an.

    $tags = (Get-AzureRmResourceGroup -Name tag-demo).Tags
    $tags += @{Name="status";Value="approved"}
    Set-AzureRmResourceGroup -Name test-group -Tag $tags

Um ein Tag oder mehrere Tags zu entfernen, speichern Sie einfach das Array ohne die Tags, die Sie entfernen möchten.

Der Prozess ist für Ressourcen identisch, Sie verwenden jedoch die Cmdlets Get-AzureRmResource und Set-AzureRmResource.

Verwenden Sie das Cmdlet **Get-AzureRmTag**, um mithilfe von PowerShell eine Liste aller Markierungen innerhalb eines Abonnements abzurufen.

    Get-AzureRmTag
    
Die Tagnamen und die Anzahl von Ressourcen und Ressourcengruppen werden mit dem Tag zurückgegeben.

    Name                      Count
    ----                      ------
    Dept                       8
    Environment                8

Möglicherweise sehen Sie Tags, die mit "hidden-" und "link:" beginnen. Hierbei handelt es sich um interne Tags, die Sie ignorieren und nicht ändern sollten.

Verwenden Sie das Cmdlet **New-AzureRmTag**, um der Taxonomie neue Markierungen hinzuzufügen. Diese Tags werden in die AutoVervollständigen-Funktion eingeschlossen, obwohl sie noch nicht auf Ressourcen oder Ressourcengruppen angewendet wurden. Um einen Markierungsnamen/Markierungswert zu entfernen, entfernen Sie zuerst die Markierung aus allen Ressourcen, mit denen es möglicherweise verwendet wird, und entfernen Sie es dann mit dem Cmdlet **Remove-AzureRmTag** aus der Taxonomie.


[powershell]: https://msdn.microsoft.com/library/mt619274(v=azure.200).aspx

<!---HONumber=AcomDC_0907_2016-->
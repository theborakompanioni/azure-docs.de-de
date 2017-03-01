In Version 3.0 des AzureRm.Resources-Moduls haben sich einige Aspekte der Tagverwendung grundlegend geändert. Überprüfen Sie zunächst Ihre Version:

```powershell
Get-Module -ListAvailable -Name AzureRm.Resources | Select Version
```

Falls in den Ergebnissen mindestens Version 3.0 angezeigt wird, sind die Beispiele in diesem Thema für Ihre Umgebung geeignet. Falls Sie nicht über Version 3.0 oder eine höhere Version verfügen, [aktualisieren Sie Ihre Version](/powershell/azureps-cmdlets-docs/) mithilfe des PowerShell-Katalogs oder Webplattform-Installers, bevor Sie mit diesem Thema fortfahren.

```powershell
Version
-------
3.5.0
```

Wenn Sie Tags auf eine Ressource oder Ressourcengruppe anwenden, werden die bereits vorhandenen Tags für diese Ressource oder Ressourcengruppe überschrieben. Daher müssen Sie Ihre Vorgehensweise darauf abstimmen, ob für die Ressource oder Ressourcengruppe bereits Tags vorhanden sind, die Sie beibehalten möchten. Mögliche Szenarien:

* Hinzufügen von Tags zu einer Ressourcengruppe ohne vorhandene Tags

  ```powershell
  Set-AzureRmResourceGroup -Name TagTestGroup -Tag @{ Dept="IT"; Environment="Test" }
  ```

* Hinzufügen von Tags zu einer Ressourcengruppe mit vorhandenen Tags

  ```powershell
  $tags = (Get-AzureRmResourceGroup -Name TagTestGroup).Tags
  $tags += @{Status="Approved"}
  Set-AzureRmResourceGroup -Tag $tags -Name TagTestGroup
  ```

* Hinzufügen von Tags zu einer Ressource ohne vorhandene Tags

  ```powershell
  Set-AzureRmResource -Tag @{ Dept="IT"; Environment="Test" } -ResourceName storageexample -ResourceGroupName TagTestGroup -ResourceType Microsoft.Storage/storageAccounts
  ```

* Hinzufügen von Tags zu einer Ressource mit vorhandenen Tags

  ```powershell
  $tags = (Get-AzureRmResource -ResourceName storageexample -ResourceGroupName TagTestGroup).Tags
  $tags += @{Status="Approved"}
  Set-AzureRmResource -Tag $tags -ResourceName storageexample -ResourceGroupName TagTestGroup -ResourceType Microsoft.Storage/storageAccounts
  ```

Verwenden Sie das folgende Skript, um alle Tags einer Ressourcengruppe auf die darin enthaltenen Ressourcen anzuwenden und **vorhandene Tags für die Ressourcen nicht beizubehalten**:

```powershell
$groups = Get-AzureRmResourceGroup
foreach ($g in $groups) 
{
    Find-AzureRmResource -ResourceGroupNameEquals $g.ResourceGroupName | ForEach-Object {Set-AzureRmResource -ResourceId $_.ResourceId -Tag $g.Tags -Force } 
}
```

Verwenden Sie das folgende Skript, um alle Tags einer Ressourcengruppe auf die darin enthaltenen Ressourcen anzuwenden und **vorhandene Tags für Ressourcen beizubehalten, die keine Duplikate sind**:

```powershell
$groups = Get-AzureRmResourceGroup
foreach ($g in $groups) 
{
    if ($g.Tags -ne $null) {
        $resources = Find-AzureRmResource -ResourceGroupNameEquals $g.ResourceGroupName 
        foreach ($r in $resources)
        {
            $resourcetags = (Get-AzureRmResource -ResourceId $r.ResourceId).Tags
            foreach ($key in $g.Tags.Keys)
            {
                if ($resourcetags.ContainsKey($key)) { $resourcetags.Remove($key) }
            }
            $resourcetags += $g.Tags
            Set-AzureRmResource -Tag $resourcetags -ResourceId $r.ResourceId -Force
        }
    }
}
```

Durch Übergeben einer leeren Hashtabelle können Sie alle Tags löschen.

```powershell
Set-AzureRmResourceGroup -Tag @{} -Name TagTestGgroup
```

Mit dem Cmdlet `Find-AzureRmResourceGroup` können Sie Ressourcengruppen mit einem bestimmten Tag abrufen.

```powershell
(Find-AzureRmResourceGroup -Tag @{ Dept="Finance" }).Name 
```

Mit dem Cmdlet `Find-AzureRmResource` können Sie alle Ressourcen mit einem bestimmten Tag und Wert abrufen.

```powershell
(Find-AzureRmResource -TagName Dept -TagValue Finance).Name
```


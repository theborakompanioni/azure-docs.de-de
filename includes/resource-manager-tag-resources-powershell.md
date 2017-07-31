In Version 3.0 des AzureRm.Resources-Moduls haben sich einige Aspekte der Tagverwendung grundlegend geändert. Überprüfen Sie zunächst Ihre Version:

```powershell
Get-Module -ListAvailable -Name AzureRm.Resources | Select Version
```

Falls in den Ergebnissen mindestens Version 3.0 angezeigt wird, sind die Beispiele in diesem Thema für Ihre Umgebung geeignet. Falls Sie nicht mindestens über Version 3.0 verfügen, [aktualisieren Sie Ihre Version](/powershell/azureps-cmdlets-docs/) mithilfe des PowerShell-Katalogs oder Webplattform-Installers, bevor Sie mit diesem Thema fortfahren.

```powershell
Version
-------
3.5.0
```

Verwenden Sie Folgendes, um die vorhandenen Tags für eine *Ressourcengruppe* anzuzeigen:

```powershell
(Get-AzureRmResourceGroup -Name examplegroup).Tags
```

Das Skript gibt das folgende Format zurück:

```powershell
Name                           Value
----                           -----
Dept                           IT
Environment                    Test
```

Verwenden Sie Folgendes, um die vorhandenen Tags für eine *Ressource mit einer angegebenen Ressourcen-ID* anzuzeigen:

```powershell
(Get-AzureRmResource -ResourceId {resource-id}).Tags
```

Oder verwenden Sie Folgendes, um die vorhandenen Tags für eine *Ressource mit einem angegebenen Namen und einer Ressourcengruppe* anzuzeigen:

```powershell
(Get-AzureRmResource -ResourceName examplevnet -ResourceGroupName examplegroup).Tags
```

Verwenden Sie Folgendes, um *Ressourcengruppen mit einem bestimmten Tag* abzurufen:

```powershell
(Find-AzureRmResourceGroup -Tag @{ Dept="Finance" }).Name 
```

Verwenden Sie Folgendes, um *Ressourcen mit einem bestimmten Tag* abzurufen:

```powershell
(Find-AzureRmResource -TagName Dept -TagValue Finance).Name
```

Wenn Sie Tags auf eine Ressource oder Ressourcengruppe anwenden, werden die bereits vorhandenen Tags für diese Ressource oder Ressourcengruppe überschrieben. Daher müssen Sie Ihre Vorgehensweise darauf abstimmen, ob für die Ressource oder Ressourcengruppe bereits Tags vorhanden sind. 

Verwenden Sie Folgendes, um einer *Ressourcengruppe ohne vorhandene Tags* Tags hinzuzufügen:

```powershell
Set-AzureRmResourceGroup -Name examplegroup -Tag @{ Dept="IT"; Environment="Test" }
```

Rufen Sie die vorhandenen Tags ab, fügen Sie das neue Tag hinzu, und wenden Sie die Tags wieder an, um einer *Ressourcengruppe mit vorhandenen Tags* Tags hinzuzufügen:

```powershell
$tags = (Get-AzureRmResourceGroup -Name examplegroup).Tags
$tags += @{Status="Approved"}
Set-AzureRmResourceGroup -Tag $tags -Name examplegroup
```

Verwenden Sie Folgendes, um einer *Ressource ohne vorhandene Tags* Tags hinzuzufügen:

```powershell
Set-AzureRmResource -Tag @{ Dept="IT"; Environment="Test" } -ResourceName examplevnet -ResourceGroupName examplegroup
```

Verwenden Sie Folgendes, um einer *Ressource mit vorhandenen Tags* Tags hinzuzufügen:

```powershell
$tags = (Get-AzureRmResource -ResourceName examplevnet -ResourceGroupName examplegroup).Tags
$tags += @{Status="Approved"}
Set-AzureRmResource -Tag $tags -ResourceName examplevnet -ResourceGroupName examplegroup
```

Verwenden Sie das folgende Skript, um alle Tags einer Ressourcengruppe auf die darin enthaltenen Ressourcen anzuwenden und *vorhandene Tags für die Ressourcen nicht beizubehalten*:

```powershell
$groups = Get-AzureRmResourceGroup
foreach ($g in $groups) 
{
    Find-AzureRmResource -ResourceGroupNameEquals $g.ResourceGroupName | ForEach-Object {Set-AzureRmResource -ResourceId $_.ResourceId -Tag $g.Tags -Force } 
}
```

Verwenden Sie das folgende Skript, um alle Tags einer Ressourcengruppe auf die darin enthaltenen Ressourcen anzuwenden und *vorhandene Tags für Ressourcen beizubehalten, die keine Duplikate sind*:

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

Durch Übergeben einer leeren Hashtabelle können Sie alle Tags löschen:

```powershell
Set-AzureRmResourceGroup -Tag @{} -Name examplegroup
```




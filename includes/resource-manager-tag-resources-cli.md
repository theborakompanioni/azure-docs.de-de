Verwenden Sie zum Hinzufügen eines Tags zu einer Ressourcengruppe **azure group set**. Übergeben Sie das Tag, wenn für die Ressourcengruppe keine Tags vorhanden sind.

```azurecli
azure group set -n tag-demo-group -t Dept=Finance
```

Tags werden als Ganzes aktualisiert. Übergeben Sie alle Tags, wenn Sie ein Tag einer Ressourcengruppe hinzufügen möchten, die über Tags verfügt. 

```azurecli
azure group set -n tag-demo-group -t Dept=Finance;Environment=Production;Project=Upgrade
```

Tags werden von Ressourcen in einer Ressourcengruppe nicht geerbt. Verwenden Sie **azure resource set**, um einer Ressource ein Tag hinzuzufügen. Übergeben Sie die API-Versionsnummer für den Ressourcentyp, dem Sie das Tag hinzufügen. Wenn Sie die API-Version abrufen möchten, verwenden Sie den folgenden Befehl mit dem Ressourcenanbieter für den festzulegenden Typ:

```azurecli
azure provider show -n Microsoft.Storage --json
```

Suchen Sie in den Ergebnissen nach dem gewünschten Ressourcentyp.

```azurecli
"resourceTypes": [
{
  "resourceType": "storageAccounts",
  ...
  "apiVersions": [
    "2016-01-01",
    "2015-06-15",
    "2015-05-01-preview"
  ]
}
...
```

Geben Sie diese API-Version, den Ressourcengruppennamen, Ressourcennamen, Ressourcentyp und Tagwert als Parameter an.

```azurecli
azure resource set -g tag-demo-group -n storagetagdemo -r Microsoft.Storage/storageAccounts -t Dept=Finance -o 2016-01-01
```

Tags sind direkt in Ressourcen und Ressourcengruppen vorhanden. Um die vorhandenen Tags anzuzeigen, rufen Sie eine Ressourcengruppe und ihre Ressourcen mit **azure group show** auf.

```azurecli
azure group show -n tag-demo-group --json
```

So werden Metadaten für die Ressourcengruppe zurückgegeben, einschließlich aller Tags, die darauf angewendet werden.

```azurecli
{
  "id": "/subscriptions/4705409c-9372-42f0-914c-64a504530837/resourceGroups/tag-demo-group",
  "name": "tag-demo-group",
  "properties": {
    "provisioningState": "Succeeded"
  },
  "location": "southcentralus",
  "tags": {
    "Dept": "Finance",
    "Environment": "Production",
    "Project": "Upgrade"
  },
  ...
}
```

Mit **azure resource show**zeigen Sie die Tags für eine bestimmte Ressource an.

```azurecli
azure resource show -g tag-demo-group -n storagetagdemo -r Microsoft.Storage/storageAccounts -o 2016-01-01 --json
```

Verwenden Sie Folgendes, um alle Ressourcen mit einem Tagwert abzurufen:

```azurecli
azure resource list -t Dept=Finance --json
```

Verwenden Sie Folgendes, um alle Ressourcengruppen mit einem Tagwert abzurufen:

```azurecli
azure group list -t Dept=Finance
```

Sie können die vorhandenen Tags in Ihrem Abonnement mit dem folgenden Befehl anzeigen:

```azurecli
azure tag list
```


<!--HONumber=Feb17_HO2-->



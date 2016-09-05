
<properties
	pageTitle="Verwalten von Ressourcen mithilfe der Azure-Befehlszeilenschnittstelle | Microsoft Azure"
	description="Verwalten Sie Azure-Ressourcen und -Gruppen mithilfe der Azure-Befehlszeilenschnittstelle (Command-Line Interface, CLI)."
	editor=""
	manager="timlt"
	documentationCenter=""
	authors="dlepow"
	services="azure-resource-manager"/>

<tags
	ms.service="azure-resource-manager"
	ms.workload="multiple"
	ms.tgt_pltfrm="vm-multiple"
	ms.devlang="na"
	ms.topic="article"
	ms.date="08/22/2016"
	ms.author="danlep"/>

# Verwalten von Azure-Ressourcen und -Ressourcengruppen mithilfe der Azure-Befehlszeilenschnittstelle


> [AZURE.SELECTOR]
- [Portal](azure-portal/resource-group-portal.md)
- [Azure-Befehlszeilenschnittstelle](xplat-cli-azure-resource-manager.md)
- [Azure PowerShell](powershell-azure-resource-manager.md)
- [.NET](https://azure.microsoft.com/documentation/samples/resource-manager-dotnet-resources-and-groups/)
- [Java](https://azure.microsoft.com/documentation/samples/resources-java-manage-resource-group/)
- [Knoten](https://azure.microsoft.com/documentation/samples/resource-manager-node-resources-and-groups/)
- [Python](https://azure.microsoft.com/documentation/samples/resource-manager-python-resources-and-groups/)
- [Ruby](https://azure.microsoft.com/documentation/samples/resource-manager-ruby-resources-and-groups/)


Die Azure-Befehlszeilenschnittstelle (Azure Command-Line Interface, Azure CLI) ist eines von mehreren Tools zum Bereitstellen und Verwalten von Ressourcen mit Resource Manager. In diesem Artikel werden allgemeine Methoden vorgestellt, mit denen Sie Azure-Ressourcen und -Ressourcengruppen mithilfe der Azure-Befehlszeilenschnittstelle im Resource Manager-Modus verwalten können. Information zum Bereitstellen von Ressourcen mithilfe der Befehlszeilenschnittstelle finden Sie unter [Bereitstellen von Ressourcen mit Azure Resource Manager-Vorlagen und Azure-CLI](resource-group-template-deploy-cli.md). Hintergrundinformationen zu Azure-Ressourcen und Resource Manager finden Sie in der [Übersicht über Azure Resource Manager](resource-group-overview.md).

>[AZURE.NOTE] Wenn Sie Azure-Ressourcen mit der Azure-Befehlszeilenschnittstelle verwalten möchten, müssen Sie die [Azure-Befehlszeilenschnittstelle installieren](xplat-cli-install.md) und sich mithilfe des `azure login`-Befehls [bei Azure anmelden](xplat-cli-connect.md). Vergewissern Sie sich, dass sich die Befehlszeilenschnittstelle im Resource Manager-Modus befindet, indem Sie `azure config mode arm` ausführen. Damit sind dann alle nötigen Vorbereitungen abgeschlossen.



## Abrufen von Ressourcengruppen und Ressourcen

### Ressourcengruppen

Führen Sie den folgenden Befehl aus, um eine Liste mit allen Ressourcengruppen in Ihrem Abonnement sowie deren Standorte abzurufen:

    azure group list
    

### Ressourcen
 Verwenden Sie den folgenden Befehl, um alle Ressourcen in einer Gruppe (beispielsweise mit dem Namen *testRG*) aufzulisten:

	azure resource list testRG

Verwenden Sie einen Befehl wie den folgenden, um eine einzelne Ressource in der Gruppe anzuzeigen (beispielsweise einen virtuellen Computer namens *MyUbuntuVM*):

	azure resource show testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15"
    
Beachten Sie den Parameter **Microsoft.Compute/virtualMachines**. Mit diesem Parameter wird die Art der Ressourcen angegeben, für die Sie Informationen anfordern möchten.
    
>[AZURE.NOTE]Bei Verwendung von Befehlen des Typs **azure resource** müssen Sie mit dem Parameter **-o** die API-Version der Ressource angeben (gilt nicht für den Befehl **list**). Sollte Ihnen die API-Version nicht bekannt sein, öffnen Sie die Vorlagendatei, und sehen Sie sich das Feld „apiVersion“ für die Ressource an. Weitere Informationen zu API-Versionen in Resource Manager finden Sie unter [Anbieter, Regionen, API-Versionen und Schemas für Resource Manager](resource-manager-supported-services.md).

Wenn Sie Details für eine Ressource anzeigen, ist es oft hilfreich, den Parameter `--json` zu verwenden. Dieser Parameter macht die Ausgabe besser lesbar, da es sich bei einigen Werten um geschachtelte Strukturen oder Sammlungen handelt. Im folgenden Beispiel sehen Sie, wie die Ergebnisse des **show**-Befehls als JSON-Dokument zurückgegeben werden.

	azure resource show testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15" --json

>[AZURE.NOTE] Bei Bedarf können Sie die JSON-Daten in einer Datei speichern. Verwenden Sie dazu das Zeichen „ &gt;“, um die Ausgabe in eine Datei zu umzuleiten. Beispiel:
>
> `azure resource show testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15" --json > myfile.json`

### Tags

Durch Hinzufügen von [Tags](resource-group-using-tags.md) zu Ressourcen und Ressourcengruppen können Sie Ihre Ressource besser strukturieren. Wenn Sie die bereits angewendeten Tags ermitteln möchten, rufen Sie einfach mit **azure group show** eine Ressourcengruppe und die dazugehörigen Ressourcen ab.

    azure group show -n tag-demo-group
    
Dieser Befehl gibt Metadaten für die Ressourcengruppe zurück. Hierzu zählen auch die bereits angewendeten Tags.
    
    info:    Executing command group show
    + Listing resource groups
    + Listing resources for the group
    data:    Id:                  /subscriptions/{guid}/resourceGroups/tag-demo-group
    data:    Name:                tag-demo-group
    data:    Location:            westus
    data:    Provisioning State:  Succeeded
    data:    Tags: Dept=Finance;Environment=Production
    data:    Resources:
    data:
    data:      Id      : /subscriptions/{guid}/resourceGroups/tag-demo-group/providers/Microsoft.Sql/servers/tfsqlserver
    data:      Name    : tfsqlserver
    data:      Type    : servers
    data:      Location: eastus2
    data:      Tags    : Dept=Finance;Environment=Production
    ...

Um nur die Tags für die Ressourcengruppe abzurufen, verwenden Sie ein JSON-Dienstprogramm, z.B. [jq](http://stedolan.github.io/jq/download/).

    azure group show -n tag-demo-group --json | jq ".tags"
    
Dieser Befehl gibt die Tags für die Ressourcengruppe zurück.
    
    {
      "Dept": "Finance",
      "Environment": "Production" 
    }

Die Tags für eine bestimmte Ressource können mithilfe von **azure resource show** angezeigt werden:

    azure resource show -g tag-demo-group -n tfsqlserver -r Microsoft.Sql/servers -o 2014-04-01-preview --json | jq ".tags"
    
Dieser Befehl gibt Folgendes zurück:
    
    {
      "Dept": "Finance",
      "Environment": "Production"
    }
    
Mit einem Befehl wie dem folgenden können Sie alle Ressourcen mit einem bestimmten Tag abrufen:

    azure resource list --json | jq ".[] | select(.tags.Dept == "Finance") | .name"
    
Dieser Befehl gibt die Namen von Ressourcen mit diesem Tag zurück:
    
    "tfsqlserver"
    "tfsqlserver/tfsqldata"

Tags werden als Ganzes aktualisiert. Wenn Sie also einer Ressource, die bereits mit einem Tag versehen wurde, ein Tag hinzufügen, müssen Sie die vorhandenen Tags abrufen, die Sie beibehalten möchten. Verwenden Sie **azure group set** zum Festlegen von Tagwerten für eine Ressourcengruppe, und geben Sie alle Tags für die Ressourcengruppe an.

    azure group set -n tag-demo-group -t Dept=Finance;Environment=Production;Project=Upgrade
    
Es wird eine Zusammenfassung der Ressourcengruppe mit den neuen Tags zurückgegeben.
    
    info:    Executing command group set
    ...
    data:    Name:                tag-demo-group
    data:    Location:            westus
    data:    Provisioning State:  Succeeded
    data:    Tags: Dept=Finance;Environment=Production;Project=Upgrade
    ...
    
Sie können die vorhandenen Tags in Ihrem Abonnement mit **azure tag list** auflisten und mit **azure tag create** ein Tag hinzufügen. Wenn Sie ein Tag aus der Taxonomie für Ihr Abonnement entfernen möchten, entfernen Sie es zunächst aus allen Ressourcen, mit denen es verwendet wird, und entfernen Sie es dann mithilfe von **azure tag delete**.

## Verwalten von Ressourcen


Wenn Sie einer Ressourcengruppe eine Ressource (beispielsweise ein Speicherkonto) hinzufügen möchten, führen Sie einen Befehl wie den folgenden aus:

    azure resource create testRG MyStorageAccount "Microsoft.Storage/storageAccounts" "westus" -o "2015-06-15" -p "{"accountType": "Standard_LRS"}"
    
Verwenden Sie neben dem Parameter **-o** (Angabe der API-Version der Ressource) auch den Parameter **-p**, um eine Zeichenfolge im JSON-Format mit ggf. erforderlichen oder zusätzlichen Eigenschaften zu übergeben.
    
    
Wenn Sie eine vorhandene Ressource (beispielsweise einen virtuellen Computer) löschen möchten, führen Sie einen Befehl wie den folgenden aus:

	azure resource delete testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15"

Verwenden Sie zum Verschieben vorhandener Ressourcen in eine andere Ressourcengruppe oder ein anderes Abonnement den Befehl **azure resource move**. Das folgende Beispiel zeigt, wie Sie ein Redis-Cache in eine neue Ressourcengruppe verschieben. Geben Sie im **-i**-Parameter eine durch Kommas getrennte Liste der zu verschiebenden Ressourcen-IDs an.


    azure resource move -i "/subscriptions/{guid}/resourceGroups/OldRG/providers/Microsoft.Cache/Redis/examplecache" -d "NewRG"

## Steuern des Zugriffs auf Ressourcen

Mit der Azure-Befehlszeilenschnittstelle können Sie Richtlinien zum Steuern des Zugriffs auf Azure-Ressourcen erstellen und verwalten. Hintergrundinformationen zu Richtliniendefinitionen und zum Zuweisen von Richtlinien zu Ressourcen finden Sie unter [Verwenden von Richtlinien für Ressourcenverwaltung und Zugriffssteuerung](resource-manager-policy.md).

Definieren Sie beispielsweise die folgende Richtlinie, um alle Anforderungen abzulehnen, bei denen der Standort nicht „USA, Westen“ oder „USA, Norden-Mitte“ lautet, und speichern Sie sie in der Richtliniendefinitionsdatei „policy.json“:

    {
    "if" : {
        "not" : {
        "field" : "location",
        "in" : ["westus" ,  "northcentralus"]
        }
    },
    "then" : {
        "effect" : "deny"
    }
    }

Führen Sie anschließend den Befehl **policy definition create** aus:

    azure policy definition create MyPolicy -p c:\temp\policy.json
    
Der Befehl generiert eine Ausgabe wie die folgende:

    + Creating policy definition MyPolicy
    data:    PolicyName:             MyPolicy
    data:    PolicyDefinitionId:     /subscriptions/########-####-####-####-############/providers/Microsoft.Authorization/policyDefinitions/MyPolicy

    data:    PolicyType:             Custom
    data:    DisplayName:            undefined
    data:    Description:            undefined
    data:    PolicyRule:             field=location, in=[westus, northcentralus], effect=deny

 Verwenden Sie die aus dem vorherigen Befehl zurückgegebene **PolicyDefinitionId**, um eine Richtlinie für den gewünschten Bereich zuzuweisen. Im folgenden Beispiel wird als Bereich das Abonnement verwendet, Sie können aber auch Ressourcengruppen oder einzelne Ressourcen verwenden:

    azure policy assignment create MyPolicyAssignment -p /subscriptions/########-####-####-####-############/providers/Microsoft.Authorization/policyDefinitions/MyPolicy -s /subscriptions/########-####-####-####-############/

Mithilfe der Befehle **policy definition show**, **policy definition set** und **policy definition delete** können Sie Richtliniendefinitionen abrufen, ändern oder entfernen.

Analog dazu können Sie mithilfe der Befehle **policy assignment show**, **policy assignment set** und **policy assignment delete** Richtlinienzuweisungen abrufen, ändern oder entfernen.


## Exportieren einer Ressourcengruppe als Vorlage

Sie können die Resource Manager-Vorlage für eine vorhandene Ressourcengruppe anzeigen. Das Exportieren der Vorlage hat zwei Vorteile:

1. Sie können problemlos zukünftige Bereitstellungen der Lösung automatisieren, da die gesamte Infrastruktur in der Vorlage definiert ist.

2. Sie können sich mit der Vorlagensyntax vertraut machen, indem Sie sich den JSON-Code für Ihre Lösung ansehen.

Mithilfe der Azure-Befehlszeilenschnittstelle können Sie entweder eine Vorlage exportieren, die dem aktuellen Zustand Ihrer Ressourcengruppe entspricht, oder die Vorlage herunterladen, die für eine bestimmte Bereitstellung verwendet wurde.

* **Exportieren der Vorlage für eine Ressourcengruppe:** Dies ist hilfreich, wenn Sie Änderungen an einer Ressourcengruppe vorgenommen haben und die JSON-Darstellung ihres aktuellen Zustands abrufen müssen. Die generierte Vorlage enthält aber nur eine minimale Anzahl von Parametern und keine Variablen. Die meisten Werte in der Vorlage sind hartcodiert. Vor dem Bereitstellen der generierten Vorlage kann es ratsam sein, weitere Werte in Parameter zu konvertieren, damit Sie die Bereitstellung für unterschiedliche Umgebungen anpassen können.

    Führen Sie den Befehl `azure group export` wie im folgenden Beispiel gezeigt aus, um die Vorlage für eine Ressourcengruppe in ein lokales Verzeichnis zu exportieren. (Geben Sie ein für Ihre Betriebssystemumgebung geeignetes lokales Verzeichnis an.)

        azure group export testRG ~/azure/templates/

* **Herunterladen der Vorlage für eine bestimmte Bereitstellung:** Dies ist hilfreich, wenn Sie die Vorlage anzeigen möchten, die für die Ressourcenbereitstellung verwendet wurde. Die Vorlage enthält alle Parameter und Variablen, die für die ursprüngliche Bereitstellung definiert wurden. Falls eine Person in Ihrer Organisation allerdings Änderungen an der Ressourcengruppe vorgenommen hat, die über die Definition in der Vorlage hinausgehen, stellt die Vorlage nicht den aktuellen Zustand der Ressourcengruppe dar.

    Führen Sie den Befehl `azure group deployment template download` aus, um die für eine bestimmte Bereitstellung verwendete Vorlage in ein lokales Verzeichnis herunterzuladen. Beispiel:

        azure group deployment template download TestRG testRGDeploy ~/azure/templates/downloads/
 
>[AZURE.NOTE] Der Vorlagenexport befindet sich noch in der Vorschauphase, und das Exportieren einer Vorlage wird derzeit nicht für alle Ressourcentypen unterstützt. Wenn Sie versuchen, eine Vorlage zu exportieren, wird ggf. ein Fehler mit dem Hinweis angezeigt, dass einige Ressourcen nicht exportiert wurden. Bei Bedarf können Sie diese Ressourcen nach dem Herunterladen manuell in der Vorlage definieren.



## Nächste Schritte

* Informationen zum Abrufen von Details zu Bereitstellungsvorgängen und zum Behandeln von Bereitstellungsfehlern mit der Azure-Befehlszeilenschnittstelle finden Sie unter [Anzeigen von Bereitstellungsvorgängen mit der Azure-Befehlszeilenschnittstelle](resource-manager-troubleshoot-deployments-cli.md).
* Informationen dazu, wie Sie eine Anwendung oder ein Skript mithilfe der Befehlszeilenschnittstelle für den Zugriff auf Ressourcen einrichten, finden Sie unter [Erstellen eines Dienstprinzipals für den Zugriff auf Ressourcen mithilfe der Azure-Befehlszeilenschnittstelle](resource-group-authenticate-service-principal-cli.md).

<!---HONumber=AcomDC_0824_2016-->
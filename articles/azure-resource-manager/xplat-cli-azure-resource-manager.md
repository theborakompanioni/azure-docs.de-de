---
title: Verwalten von Ressourcen mithilfe der Azure CLI | Microsoft Docs
description: Verwalten Sie Azure-Ressourcen und -Gruppen mithilfe der Azure-Befehlszeilenschnittstelle (Command-Line Interface, CLI).
editor: 
manager: timlt
documentationcenter: 
author: tfitzmac
services: azure-resource-manager
ms.assetid: bb0af466-4f65-4559-ac3a-43985fa096ff
ms.service: azure-resource-manager
ms.workload: multiple
ms.tgt_pltfrm: vm-multiple
ms.devlang: na
ms.topic: article
ms.date: 08/22/2016
ms.author: tomfitz
translationtype: Human Translation
ms.sourcegitcommit: 0782000e87bed0d881be5238c1b91f89a970682c
ms.openlocfilehash: 7fcf16ac835089a1c4127fd34264f2cf7cb098ac


---
# <a name="use-the-azure-cli-to-manage-azure-resources-and-resource-groups"></a>Verwalten von Azure-Ressourcen und -Ressourcengruppen mithilfe der Azure-Befehlszeilenschnittstelle
> [!div class="op_single_selector"]
> * [Portal](resource-group-portal.md) 
> * [Azure-Befehlszeilenschnittstelle](xplat-cli-azure-resource-manager.md)
> * [Azure PowerShell](powershell-azure-resource-manager.md)
> * [REST-API](resource-manager-rest-api.md)
> 
> 

Die Azure-Befehlszeilenschnittstelle (Azure Command-Line Interface, Azure CLI) ist eines von mehreren Tools zum Bereitstellen und Verwalten von Ressourcen mit Resource Manager. In diesem Artikel werden allgemeine Methoden vorgestellt, mit denen Sie Azure-Ressourcen und -Ressourcengruppen mithilfe der Azure-Befehlszeilenschnittstelle im Resource Manager-Modus verwalten können. Information zum Bereitstellen von Ressourcen mithilfe der Befehlszeilenschnittstelle finden Sie unter [Bereitstellen von Ressourcen mit Azure Resource Manager-Vorlagen und Azure-CLI](resource-group-template-deploy-cli.md). Hintergrundinformationen zu Azure-Ressourcen und Resource Manager finden Sie in der [Übersicht über Azure Resource Manager](resource-group-overview.md).

> [!NOTE]
> Wenn Sie Azure-Ressourcen mit der Azure-Befehlszeilenschnittstelle verwalten möchten, müssen Sie die [Azure-Befehlszeilenschnittstelle installieren](../xplat-cli-install.md) und sich mithilfe des `azure login`-Befehls [bei Azure anmelden](../xplat-cli-connect.md). Vergewissern Sie sich, dass sich die Befehlszeilenschnittstelle im Resource Manager-Modus befindet, indem Sie `azure config mode arm` ausführen. Damit sind dann alle nötigen Vorbereitungen abgeschlossen.
> 
> 

## <a name="get-resource-groups-and-resources"></a>Abrufen von Ressourcengruppen und Ressourcen
### <a name="resource-groups"></a>Ressourcengruppen
Führen Sie den folgenden Befehl aus, um eine Liste mit allen Ressourcengruppen in Ihrem Abonnement sowie deren Standorte abzurufen:

    azure group list


### <a name="resources"></a>Ressourcen
 Verwenden Sie den folgenden Befehl, um alle Ressourcen in einer Gruppe (beispielsweise mit dem Namen *testRG*) aufzulisten:

    azure resource list testRG

Verwenden Sie einen Befehl wie den folgenden, um eine einzelne Ressource in der Gruppe anzuzeigen (beispielsweise einen virtuellen Computer namens *MyUbuntuVM*:

    azure resource show testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15"

Beachten Sie den Parameter **Microsoft.Compute/virtualMachines**. Mit diesem Parameter wird die Art der Ressourcen angegeben, für die Sie Informationen anfordern möchten.

> [!NOTE]
> Bei Verwendung von Befehlen des Typs **azure resource** müssen Sie mit dem Parameter **-o** die API-Version der Ressource angeben (gilt nicht für den Befehl **list**). Sollte Ihnen die API-Version nicht bekannt sein, öffnen Sie die Vorlagendatei, und sehen Sie sich das Feld „apiVersion“ für die Ressource an. Weitere Informationen zu API-Versionen in Resource Manager finden Sie unter [Anbieter, Regionen, API-Versionen und Schemas für Resource Manager](resource-manager-supported-services.md).
> 
> 

Wenn Sie Details für eine Ressource anzeigen, ist es oft hilfreich, den Parameter `--json` zu verwenden. Dieser Parameter macht die Ausgabe besser lesbar, da es sich bei einigen Werten um geschachtelte Strukturen oder Sammlungen handelt. Im folgenden Beispiel sehen Sie, wie die Ergebnisse des **show**-Befehls als JSON-Dokument zurückgegeben werden.

    azure resource show testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15" --json

> [!NOTE]
> Bei Bedarf können Sie die JSON-Daten in einer Datei speichern. Verwenden Sie dazu das Zeichen „&gt;“, um die Ausgabe in eine Datei zu umzuleiten. Beispiel:
> 
> `azure resource show testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15" --json > myfile.json`
> 
> 

### <a name="tags"></a>Tags
[!INCLUDE [resource-manager-tag-resources-cli](../../includes/resource-manager-tag-resources-cli.md)]

## <a name="manage-resources"></a>Verwalten von Ressourcen
Wenn Sie einer Ressourcengruppe eine Ressource (beispielsweise ein Speicherkonto) hinzufügen möchten, führen Sie einen Befehl wie den folgenden aus:

    azure resource create testRG MyStorageAccount "Microsoft.Storage/storageAccounts" "westus" -o "2015-06-15" -p "{\"accountType\": \"Standard_LRS\"}"

Verwenden Sie neben dem Parameter **-o** (Angabe der API-Version der Ressource) auch den Parameter **-p**, um eine Zeichenfolge im JSON-Format mit ggf. erforderlichen oder zusätzlichen Eigenschaften zu übergeben.

Wenn Sie eine vorhandene Ressource (beispielsweise einen virtuellen Computer) löschen möchten, führen Sie einen Befehl wie den folgenden aus:

    azure resource delete testRG MyUbuntuVM Microsoft.Compute/virtualMachines -o "2015-06-15"

Verwenden Sie zum Verschieben vorhandener Ressourcen in eine andere Ressourcengruppe oder ein anderes Abonnement den Befehl **azure resource move** . Das folgende Beispiel zeigt, wie Sie ein Redis-Cache in eine neue Ressourcengruppe verschieben. Geben Sie im **-i** -Parameter eine durch Kommas getrennte Liste der zu verschiebenden Ressourcen-IDs an.

    azure resource move -i "/subscriptions/{guid}/resourceGroups/OldRG/providers/Microsoft.Cache/Redis/examplecache" -d "NewRG"

## <a name="control-access-to-resources"></a>Steuern des Zugriffs auf Ressourcen
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

    + Creating policy definition MyPolicy data:    PolicyName:             MyPolicy data:    PolicyDefinitionId:     /subscriptions/########-####-####-####-############/providers/Microsoft.Authorization/policyDefinitions/MyPolicy

    data:    PolicyType:             Custom data:    DisplayName:            undefined data:    Description:            undefined data:    PolicyRule:             field=location, in=[westus, northcentralus], effect=deny

 Verwenden Sie das aus dem vorherigen Befehl zurückgegebene **PolicyDefinitionId**-Element, um eine Richtlinie für den gewünschten Bereich zuzuweisen. Im folgenden Beispiel wird als Bereich das Abonnement verwendet, Sie können aber auch Ressourcengruppen oder einzelne Ressourcen verwenden:

    azure policy assignment create MyPolicyAssignment -p /subscriptions/########-####-####-####-############/providers/Microsoft.Authorization/policyDefinitions/MyPolicy -s /subscriptions/########-####-####-####-############/

Mithilfe der Befehle **policy definition show**, **policy definition set** und **policy definition delete** können Sie Richtliniendefinitionen abrufen, ändern oder entfernen.

Analog dazu können Sie mithilfe der Befehle **policy assignment show**, **policy assignment set** und **policy assignment delete** Richtlinienzuweisungen abrufen, ändern oder entfernen.

## <a name="export-a-resource-group-as-a-template"></a>Exportieren einer Ressourcengruppe als Vorlage
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

> [!NOTE]
> Der Vorlagenexport befindet sich noch in der Vorschauphase, und das Exportieren einer Vorlage wird derzeit nicht für alle Ressourcentypen unterstützt. Wenn Sie versuchen, eine Vorlage zu exportieren, wird ggf. ein Fehler mit dem Hinweis angezeigt, dass einige Ressourcen nicht exportiert wurden. Bei Bedarf können Sie diese Ressourcen nach dem Herunterladen manuell in der Vorlage definieren.
> 
> 

## <a name="next-steps"></a>Nächste Schritte
* Informationen zum Abrufen von Details zu Bereitstellungsvorgängen und zum Behandeln von Bereitstellungsfehlern mit der Azure-Befehlszeilenschnittstelle finden Sie unter [Anzeigen von Bereitstellungsvorgängen](resource-manager-deployment-operations.md).
* Informationen dazu, wie Sie eine Anwendung oder ein Skript mithilfe der Befehlszeilenschnittstelle für den Zugriff auf Ressourcen einrichten, finden Sie unter [Erstellen eines Dienstprinzipals für den Zugriff auf Ressourcen mithilfe der Azure-Befehlszeilenschnittstelle](resource-group-authenticate-service-principal-cli.md).
* Anleitungen dazu, wie Unternehmen Abonnements mit Resource Manager effektiv verwalten können, finden Sie unter [Azure-Unternehmensgerüst - Präskriptive Abonnementgovernance](resource-manager-subscription-governance.md).




<!--HONumber=Jan17_HO2-->



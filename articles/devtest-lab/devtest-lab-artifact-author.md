---
title: "Erstellen von benutzerdefinierten Artefakten für Ihre DevTest Labs-VM | Microsoft Docs"
description: "Erfahren Sie, wie Sie Ihre eigenen Artefakte für die Verwendung mit DevTest Labs erstellen."
services: devtest-lab,virtual-machines
documentationcenter: na
author: tomarcher
manager: douge
editor: 
ms.assetid: 32dcdc61-ec23-4a01-b731-78c029ea5316
ms.service: devtest-lab
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/16/2017
ms.author: tarcher
ms.translationtype: HT
ms.sourcegitcommit: 25e4506cc2331ee016b8b365c2e1677424cf4992
ms.openlocfilehash: 2412033daa1d97860dd9f380178622b1ddc590c0
ms.contentlocale: de-de
ms.lasthandoff: 08/24/2017

---
# <a name="create-custom-artifacts-for-your-devtest-labs-vm"></a>Erstellen von benutzerdefinierten Artefakten für Ihre DevTest Labs-VM
> [!VIDEO https://channel9.msdn.com/Blogs/Azure/how-to-author-custom-artifacts/player]
> 
> 

## <a name="overview"></a>Übersicht
**Artefakte** werden zum Bereitstellen und Konfigurieren Ihrer Anwendung nach der Bereitstellung eines virtuellen Computers verwendet. Ein Artefakt umfasst eine Artefaktdefinitionsdatei und andere Skriptdateien, die in einem Ordner in einem Git-Repository gespeichert sind. Artefaktdefinitionsdateien bestehen aus JSON und Ausdrücken, mit denen Sie angeben können, was Sie auf einem virtuellen Computer installieren möchten. Sie können beispielsweise den Namen eines Artefakts, den auszuführenden Befehl sowie Parameter, die beim Ausführen des Befehls verfügbar gemacht werden, definieren. Sie können auf andere Skriptdateien innerhalb der Artefaktdefinitionsdatei anhand ihres Namens verweisen.

## <a name="artifact-definition-file-format"></a>Format von Artefaktdefinitionsdateien
Das folgende Beispiel zeigt die Abschnitte, die die grundlegende Struktur einer Definitionsdatei bilden:

    {
      "$schema": "https://raw.githubusercontent.com/Azure/azure-devtestlab/master/schemas/2016-11-28/dtlArtifacts.json",
      "title": "",
      "description": "",
      "iconUri": "",
      "targetOsType": "",
      "parameters": {
        "<parameterName>": {
          "type": "",
          "displayName": "",
          "description": ""
        }
      },
      "runCommand": {
        "commandToExecute": ""
      }
    }

| Elementname | Erforderlich | Beschreibung |
| --- | --- | --- |
| $schema |Nein |Speicherort der JSON-Schemadatei, die beim Testen der Gültigkeit der Definitionsdatei hilft. |
| title |Ja |Der Name des im Lab angezeigten Artefakts. |
| Beschreibung |Ja |Die Beschreibung des im Lab angezeigten Artefakts. |
| iconUri |Nein |Der URI des im Lab angezeigten Symbols. |
| targetOsType |Ja |Das Betriebssystem des virtuellen Computers, auf dem das Artefakt installiert ist. Unterstützte Optionen sind: „Windows“ und „Linux“. |
| parameters |Nein |Werte, die bereitgestellt werden, wenn der Artefaktinstallationsbefehl auf einem Computer ausgeführt wird. Dies hilft beim Anpassen Ihres Artefakts. |
| runCommand |Ja |Artefaktinstallationsbefehl, der auf einem virtuellen Computer ausgeführt wird. |

### <a name="artifact-parameters"></a>Artefaktparameter
Im Parameterabschnitt der Vorlage geben Sie an, welche Werte ein Benutzer beim Installieren eines Artefakts eingeben kann. Auf diese Werte können Sie im Artefaktinstallationsbefehl verweisen.

Sie definieren Parameter mit der folgenden Struktur:

    "parameters": {
        "<parameterName>": {
          "type": "<type-of-parameter-value>",
          "displayName": "<display-name-of-parameter>",
          "description": "<description-of-parameter>"
        }
      }

| Elementname | Erforderlich | Beschreibung |
| --- | --- | --- |
| Typ |Ja |Der Typ des Parameterwerts. In der folgenden Liste finden Sie die zulässigen Typen: |
| displayName |Ja |Der Name des Parameters, der einem Benutzer im Labor angezeigt wird. | |
| Beschreibung |Ja |Die Beschreibung des Parameters, der im Labor angezeigt wird. |

Folgende Typen sind zulässig:

* „string“: eine beliebige gültige JSON-Zeichenfolge
* „int“: eine gültige JSON-Ganzzahl
* „bool“: ein gültiger boolescher JSON-Wert
* „array“: ein gültiges JSON-Array

## <a name="artifact-expressions-and-functions"></a>Ausdrücke und Funktionen für Artefakte
Sie können zum Erstellen des Artefaktinstallationsbefehls Ausdrücke und Funktionen verwenden.
Ausdrücke werden in Klammern eingeschlossen ([ und ]) und beim Installieren des Artefakts ausgewertet. Ausdrücke können an beliebiger Stelle in einem JSON-Zeichenfolgenwert auftreten und geben immer einen anderen JSON-Wert zurück. Wenn Sie ein Zeichenfolgenliteral verwenden müssen, das mit einer eckigen Klammer [ beginnt, müssen Sie zwei eckige Klammern verwenden [[.
In der Regel verwenden Sie Ausdrücke mit Funktionen, um einen Wert zu erstellen. Genau wie in JavaScript haben Funktionsaufrufe das Format „funktionsName(arg1,arg2,arg3)“.

Die folgende Liste zeigt häufig verwendete Funktionen:

* parameters(Parametername): gibt einen Parameterwert zurück, der beim Ausführen des Artefaktbefehls bereitgestellt wird.
* concat(arg1,arg2,arg3, …..) –     kombiniert mehrere Zeichenfolgenwerte. Diese Funktion kann eine beliebige Anzahl an Argumenten entgegennehmen. 

Das folgende Beispiel zeigt, wie Sie mit Ausdrücken und Funktionen einen Wert erstellen:

    runCommand": {
         "commandToExecute": "[concat('powershell.exe -ExecutionPolicy bypass \"& ./startChocolatey.ps1'
    , ' -RawPackagesList ', parameters('packages')
    , ' -Username ', parameters('installUsername')
    , ' -Password ', parameters('installPassword'))]"
    }

## <a name="create-a-custom-artifact"></a>Erstellen eines benutzerdefinierten Artefakts
Erstellen Sie Ihr benutzerdefiniertes Artefakt anhand der folgenden Schritte:

1. Installieren eines JSON-Editors: Sie benötigen zum Bearbeiten von Artefaktdefinitionsdateien einen JSON-Editor. Empfehlenswert ist [Visual Studio Code](https://code.visualstudio.com/), der für Windows, Linux und OS X verfügbar ist.
2. Abrufen einer Beispieldatei „artifactfile.json“: Sehen Sie sich die vom Azure DevTest Labs-Team erstellten Artefakte in unserem [GitHub-Repository](https://github.com/Azure/azure-devtestlab) an, in dem wir eine umfangreiche Bibliothek von Artefakten erstellt haben, die Ihnen beim Erstellen eigener Artefakte helfen. Laden Sie eine Artefaktdefinitionsdatei herunter, und nehmen Sie an dieser Änderungen vor, um eigene Artefakte zu erstellen.
3. Nutzen von IntelliSense: Zeigen Sie mithilfe von IntelliSense gültige Elemente an, die zum Erstellen einer Artefaktdefinitionsdatei verwendet werden können. Sie können auch die verschiedenen Optionen für die Werte eines Elements sehen. Beispielsweise zeigt IntelliSense beim Bearbeiten des **TargetOsType** -Elements die beiden Wahlmöglichkeiten Windows und Linux an.
4. Speichern Sie das Artefakt in einem [Git-Repository](devtest-lab-add-artifact-repo.md).
   
   1. Erstellen Sie ein separates Verzeichnis für jedes Artefakt, wobei der Verzeichnisname mit dem Artefaktnamen identisch ist.
   2. Speichern Sie die Artefaktdefinitionsdatei („artifactfile.json“) in dem erstellten Verzeichnis.
   3. Speichern Sie die Skripts, auf die im Artefaktinstallationsbefehl verwiesen wird.
      
      Ein Beispiel für den möglichen Inhalt eines Artefaktordners:
      
      ![Beispiel-Git-Repository für Artefakte](./media/devtest-lab-artifact-author/git-repo.png)
5. Fügen Sie das Artefaktrepository dem Lab hinzu. Weitere Informationen hierzu finden Sie im Artikel [Hinzufügen eines Git-Repositorys für Artefakte und Vorlagen](devtest-lab-add-artifact-repo.md).

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="related-articles"></a>Verwandte Artikel
* [Diagnostizieren von Artefaktfehlern in DevTest Labs](devtest-lab-troubleshoot-artifact-failure.md)
* [Join a VM to existing AD Domain using a resource manager template in Azure DevTest Labs](http://www.visualstudiogeeks.com/blog/DevOps/Join-a-VM-to-existing-AD-domain-using-ARM-template-AzureDevTestLabs) (Einbinden eines virtuellen Computers in eine vorhandenen AD-Domäne mithilfe einer Resource Manager-Vorlage in Azure DevTest Labs)

## <a name="next-steps"></a>Nächste Schritte
* Erfahren Sie, wie Sie einem [ein Git-Artefaktrepository zu einem Lab hinzufügen](devtest-lab-add-artifact-repo.md).



---
title: "Erstellen und Veröffentlichen eines Marketplace-Elements in Azure Stack | Microsoft-Dokumentation"
description: "Erstellen und veröffentlichen Sie ein Marketplace-Element in Azure Stack."
services: azure-stack
documentationcenter: 
author: ErikjeMS
manager: byronr
editor: 
ms.assetid: 77e5f60c-a86e-4d54-aa8d-288e9a889386
ms.service: azure-stack
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/21/2017
ms.author: erikje
ms.translationtype: HT
ms.sourcegitcommit: 83f19cfdff37ce4bb03eae4d8d69ba3cbcdc42f3
ms.openlocfilehash: f1a04567e86ebea722a7de6a86117a49d1205966
ms.contentlocale: de-de
ms.lasthandoff: 09/15/2017

---
# <a name="create-and-publish-a-marketplace-item"></a>Erstellen und Veröffentlichen eines Marketplace-Elements
## <a name="create-a-marketplace-item"></a>Erstellen von Marketplace-Elementen
1. Laden Sie das [Paketerstellungstool des Azure-Katalogs](http://www.aka.ms/azurestackmarketplaceitem) und das Azure Stack Marketplace-Beispielelement herunter.
2. Öffnen Sie das Marketplace-Beispielelement, und benennen Sie den Ordner **SimpleVMTemplate** um. (Verwenden Sie den gleichen Namen wie für das Marketplace-Element, etwa **Contoso.TodoList**.) Der Ordner enthält Folgendes:
   
       /Contoso.TodoList/
       /Contoso.TodoList/Manifest.json
       /Contoso.TodoList/UIDefinition.json
       /Contoso.TodoList/Icons/
       /Contoso.TodoList/Strings/
       /Contoso.TodoList/DeploymentTemplates/
3. [Erstellen Sie eine Azure Resource Manager-Vorlage](../azure-resource-manager/resource-group-authoring-templates.md), oder wählen Sie eine Vorlage von GitHub aus. Das Marketplace-Element verwendet diese Vorlage, um eine Ressource zu erstellen.
4. Testen Sie die Vorlage mit den Microsoft Azure Stack-APIs, um sicherzustellen, dass die Ressource erfolgreich bereitgestellt werden kann.
5. Wenn Ihre Vorlage auf einem VM-Image basiert, befolgen Sie die Anweisungen zum [Hinzufügen eines VM-Images zu Azure Stack](azure-stack-add-vm-image.md).
6. Speichern Sie die Azure Resource Manager-Vorlage im Ordner **/Contoso.TodoList/DeploymentTemplates/**.
7. Wählen Sie Symbole und Text für Ihr Marketplace-Element aus. Fügen Sie Symbole zum Ordner **Symbole** hinzu. Text wird der Datei **Ressourcen** im Ordner **Zeichenfolgen** hinzugefügt. Verwenden Sie für die Symbole die Namenskonvention für kleine, mittlere, große und breite Symbole. Eine ausführliche Beschreibung finden Sie unter [Referenz: Benutzeroberfläche für Marketplace-Elemente](#reference-marketplace-item-ui).
   
   > [!NOTE]
   > Für die ordnungsgemäße Erstellung des Marketplace-Elements sind alle vier Symbolgrößen (klein, mittel, groß, breit) erforderlich.
   > 
   > 
8. Geben Sie in der Datei **manifest.json** für **name** den Namen des Marketplace-Elements ein. Geben Sie außerdem für **publisher** Ihren Namen oder Ihr Unternehmen ein.
9. Fügen Sie unter **artifacts** für **name** und **path** die korrekten Informationen für die Azure Resource Manager-Vorlage ein, die Sie aufgenommen haben.
   
         "artifacts": [
            {
                "name": "Type your template name",
                "type": "Template",
                "path": "DeploymentTemplates\\Type your path",
                "isDefault": true
            }
10. Ersetzen Sie **My Marketplace Items** (Meine Marketplace-Elemente) durch eine Liste der Kategorien, in denen Ihr Marketplace-Element angezeigt werden soll.
    
             "categories":[
                 "My Marketplace Items"
              ],
11. Informationen zur weiteren Bearbeitung der Datei „manifest.json“ finden Sie unter [Referenz: Datei „manifest.json“ für Marketplace-Elemente](#reference-marketplace-item-manifestjson).
12. Öffnen Sie eine Eingabeaufforderung, und führen Sie den folgenden Befehl aus, um die Ordner in eine AZPKG-Datei zu packen:
    
        AzureGalleryPackager.exe package –m <path to manifest.json> -o <output location for the package>
    
    > [!NOTE]
    > Der vollständige Pfad zum Ausgabepaket muss vorhanden sein. Wenn der Ausgabepfad „C:\MarketPlaceItem\yourpackage.azpkg“ lautet, muss der Ordner „C:\MarketPlaceItem“ vorhanden sein.
    > 
    > 

## <a name="publish-a-marketplace-item"></a>Veröffentlichen von Marketplace-Elementen
1. Verwenden Sie PowerShell oder Azure Storage-Explorer, um Ihr Marketplace-Element (.azpkg) in Azure Blob Storage hochzuladen. Sie können es in Azure Stack-Speicher oder Azure Storage hochladen. (Es handelt sich um einen temporären Speicherort für das Paket.) Stellen Sie sicher, dass das Blob öffentlich zugänglich ist.
2. Stellen Sie auf dem virtuellen Clientcomputer in der Microsoft Azure Stack-Umgebung sicher, dass Ihre PowerShell-Sitzung mit Ihren Dienstadministrator-Anmeldeinformationen eingerichtet ist. Eine Anleitung zum Authentifizieren von PowerShell in Azure Stack finden Sie unter [Deploy templates in Azure Stack using PowerShell](azure-stack-deploy-template-powershell.md) (Bereitstellen von Vorlagen in Azure Stack mit PowerShell).
3. Verwenden Sie das PowerShell-Cmdlet **Add-AzureRMGalleryItem**, um das Marketplace-Element in Azure Stack zu veröffentlichen. Beispiel:
   
       Add-AzureRMGalleryItem -GalleryItemUri `
       https://sample.blob.core.windows.net/gallerypackages/Microsoft.SimpleTemplate.1.0.0.azpkg –Verbose
   
   | Parameter | Beschreibung |
   | --- | --- |
   | SubscriptionID |Azure-Abonnement-ID. Sie kann mithilfe von PowerShell abgerufen werden. Wenn Sie sie lieber im Portal abrufen möchten, navigieren Sie zum Anbieterabonnement, und kopieren Sie die Abonnement-ID. |
   | GalleryItemUri |Blob-URI für das Katalogpaket, das bereits in den Speicher hochgeladen wurde |
   | Apiversion |Festgelegt auf **2015-04-01** |
4. Rufen Sie das Portal auf. Sie können jetzt das Marketplace-Element im Portal – als Administrator und als Mandant – anzeigen.
   
   > [!NOTE]
   > Es kann ein paar Minuten dauern, bis das Paket angezeigt wird.
   > 
   > 
5. Das Marketplace-Element wurde nun im Azure Stack Marketplace gespeichert. Sie können es aus dem Blobspeicherort löschen.
6. Sie können ein Marketplace-Element mit dem Cmdlet **Remove-AzureRMGalleryItem** entfernen. Beispiel:
   
        Remove-AzureRMGalleryItem -Name Microsoft.SimpleTemplate.1.0.0  –Verbose
   
   > [!NOTE]
   > Auf der Marketplace-Benutzeroberfläche wird unter Umständen ein Fehler angezeigt, nachdem Sie ein Element entfernt haben. Um das Problem zu beheben, klicken Sie im Portal auf **Einstellungen**. Wählen Sie anschließend unter **Portalanpassung** die Option **Änderungen verwerfen** aus.
   > 
   > 

## <a name="reference-marketplace-item-manifestjson"></a>Referenz: Datei „manifest.json“ für Marketplace-Elemente
### <a name="identity-information"></a>Identitätsinformationen
| Name | Erforderlich | Typ | Einschränkungen | Beschreibung |
| --- | --- | --- | --- | --- |
| Name |X |String |[A-Za-z0-9]+ | |
| Herausgeber |X |String |[A-Za-z0-9]+ | |
| Version |X |String |[SemVer, v2](http://semver.org/) | |

### <a name="metadata"></a>Metadaten
| Name | Erforderlich | Typ | Einschränkungen | Beschreibung |
| --- | --- | --- | --- | --- |
| displayName |X |String |Empfehlung: 80 Zeichen |Im Portal wird der Elementname unter Umständen nicht richtig angezeigt, wenn er länger als 80 Zeichen ist. |
| PublisherDisplayName |X |String |Empfehlung: 30 Zeichen |Im Portal wird der Herausgebername unter Umständen nicht richtig angezeigt, wenn er länger als 30 Zeichen ist. |
| PublisherLegalName |X |String |Maximal 256 Zeichen | |
| Zusammenfassung |X |String |60–100 Zeichen | |
| LongSummary |X |String |140–256 Zeichen |Noch nicht für Azure Stack gültig |
| Beschreibung |X |[HTML](https://auxdocs.azurewebsites.net/en-us/documentation/articles/gallery-metadata#html-sanitization) |500 bis 5.000 Zeichen | |

### <a name="images"></a>Bilder
Marketplace verwendet die folgenden Symbole:

| Name | Breite | Höhe | Hinweise |
| --- | --- | --- | --- |
| Breite |255 px |115 px |Immer erforderlich |
| Groß |115 px |115 px |Immer erforderlich |
| Mittel |90 px |90 px |Immer erforderlich |
| Klein |40 px |40 px |Immer erforderlich |
| Screenshot |533 px |32 px |Optional |

### <a name="categories"></a>Categories
Jedes Marketplace-Element muss mit einer Kategorie gekennzeichnet werden, die die Position des Elements auf der Portalbenutzeroberfläche angibt. Sie können eine der vorhandenen Kategorien in Azure Stack („Compute“, „Daten und Speicher“ usw.) oder eine neue Kategorie auswählen.

### <a name="links"></a>Links
Jedes Marketplace-Element kann verschiedene Links zu zusätzlichen Inhalten enthalten. Die Links werden als eine Liste der Namen und URIs angegeben.

| Name | Erforderlich | Typ | Einschränkungen | Beschreibung |
| --- | --- | --- | --- | --- |
| displayName |X |String |Maximal 64 Zeichen | |
| Uri |X |URI | | |

### <a name="additional-properties"></a>Zusätzliche Eigenschaften
Zusätzlich zu den oben genannten Metadaten können Marketplace-Autoren benutzerdefinierte Schlüssel-Wert-Paare mit Daten in der folgenden Form angeben.

| Name | Erforderlich | Typ | Einschränkungen | Beschreibung |
| --- | --- | --- | --- | --- |
| displayName |X |String |Maximal 25 Zeichen | |
| Wert |X |String |Maximal 30 Zeichen | |

### <a name="html-sanitization"></a>HTML-Bereinigung
Für jedes Feld, das HTML zulässt, sind die folgenden Elemente und Attribute zulässig:

h1, h2, h3, h4, h5, p, ol, ul, li, a[target|href], br, strong, em, b, i

## <a name="reference-marketplace-item-ui"></a>Referenz: Benutzeroberfläche für Marketplace-Elemente
Im Azure Stack-Portal werden folgende Symbole und Texte für Marketplace-Elemente angezeigt:

### <a name="create-blade"></a>Blatt "Erstellen"
![Blatt "Erstellen"](media/azure-stack-marketplace-item-ui-reference/image1.png)

### <a name="marketplace-item-details-blade"></a>Blatt „Details“ für ein Marketplace-Element
![Blatt „Details“ für ein Marketplace-Element](media/azure-stack-marketplace-item-ui-reference/image3.png)



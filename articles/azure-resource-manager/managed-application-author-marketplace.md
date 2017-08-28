---
title: Verwaltete Azure-Anwendungen im Marketplace | Microsoft-Dokumentation
description: "In diesem Artikel werden verwaltete Azure-Anwendungen beschrieben, die im Marketplace zur Verfügung stehen."
services: azure-resource-manager
author: ravbhatnagar
manager: rjmax
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.date: 07/09/2017
ms.author: gauravbh; tomfitz
ms.translationtype: HT
ms.sourcegitcommit: 398efef3efd6b47c76967563251613381ee547e9
ms.openlocfilehash: 58ac7665abf7e75a43bb0b92bdf6f41005c3efe8
ms.contentlocale: de-de
ms.lasthandoff: 08/11/2017

---

# <a name="azure-managed-applications-in-the-marketplace"></a>Verwaltete Azure-Anwendungen im Marketplace

 MSPs, ISVs und Systemintegratoren (SIs) können ihre Lösungen mithilfe von verwalteten Azure-Anwendungen für alle Azure Marketplace-Kunden anbieten. Solche Lösungen reduzieren den Verwaltungs- und Wartungs-Mehraufwand für Kunden. Herausgeber können Infrastruktur und Software über den Marketplace verkaufen. Sie können für verwaltete Anwendungen Dienste und Support für den Betrieb hinzufügen. Weitere Informationen finden Sie in der [Übersicht über verwaltete Anwendungen](managed-application-overview.md).

In diesem Artikel wird erklärt, wie ein MSP, ISV oder SI eine Anwendung im Marketplace veröffentlichen und dem Kunden allgemein zur Verfügung stellen kann.

## <a name="prerequisites-for-publishing-a-managed-application"></a>Voraussetzungen für das Veröffentlichen einer verwalteten Anwendung

Voraussetzungen für eine Auflistung im Marketplace:

* Technisch

    *  Informationen über die grundlegende Struktur und Syntax der Azure Resource Manager-Vorlagen finden Sie unter [Azure Resource Manager-Vorlagen](resource-group-authoring-templates.md).
    *  Vollständige Vorlagenlösungen finden Sie unter [Azure Schnellstartvorlagen](https://azure.microsoft.com/en-us/documentation/templates/) oder [Schnellstartvorlage-Repository](https://github.com/azure/azure-quickstart-templates).
    *  Informationen zum Erstellen der Schnittstelle für Kunden, die ihre Anwendung über den Marketplace bereitstellen, finden Sie unter [Erstellen einer Benutzeroberflächen-Definitionsdatei](managed-application-createuidefinition-overview.md).

* Nicht technisch (Geschäftsanforderungen)

    *   Der Standort Ihres Unternehmens oder der Niederlassung muss sich in einem Land befinden, das vom Marketplace für den Verkauf unterstützt wird.
    *   Ihr Produkt muss auf eine Weise lizenziert sein, die mit den vom Marketplace unterstützten Abrechnungsmodellen kompatibel ist.
    *   Sie sind dafür zuständig, technischen Support für Kunden in einer wirtschaftlich vertretbaren Weise zur Verfügung zu stellen. Der Support kann kostenlos, kostenpflichtig oder mittels der Unterstützung durch die Community erfolgen.
    *   Sie sind für die Lizenzierung Ihrer Software und sämtlicher Abhängigkeiten von Drittanbietersoftware verantwortlich.
    *   Sie müssen Inhalte bereitstellen, die die erforderlichen Kriterien erfüllen, damit Ihr Angebot im Marketplace und im Azure-Portal gelistet wird.
    *   Sie müssen den Bedingungen der Azure Marketplace-Teilnahmerichtlinien und der Herausgebervereinbarung zustimmen.
    *   Sie müssen der Einhaltung der Nutzungsbedingungen, der Datenschutzerklärung von Microsoft und der Microsoft Azure Certified-Programmvereinbarung zustimmen.

## <a name="create-a-new-azure-application-offer"></a>Erstellen eines neuen Angebots für eine Azure-Anwendung

Wenn Sie die Voraussetzungen erfüllen, können Sie ein Angebot für eine verwaltete Anwendung erstellen. Werfen wir einen kurzen Blick auf grundlegende Informationen zu einem Angebot und einer SKU.

### <a name="offer"></a>Angebot

Das Angebot für eine verwaltete Anwendung entspricht einer Produktklasse, die ein Herausgeber anbietet. Wenn Sie über eine neue Art von Lösung/Anwendung verfügen, die Sie im Marketplace zur Verfügung stellen möchten, können Sie sie als ein neues Angebot einrichten. Ein Angebot ist eine Sammlung von SKUs. Jedes Angebot wird als eigene Entität im Azure Marketplace angezeigt.

### <a name="sku"></a>SKU

Eine SKU ist die kleinste kostenpflichtige Einheit eines Angebots. Mithilfe einer SKU innerhalb derselben Produktklasse (Angebot) können Sie zwischen Folgendem unterscheiden:

* Verschiedenen unterstützten Funktionen
* Verwalteten oder nicht verwalteten Angeboten
* Unterstützten Abrechnungsmodellen

Eine SKU wird im Marketplace unterhalb des übergeordneten Angebots angezeigt. Sie wird als eigene kostenpflichtige Entität im Azure-Portal angezeigt.

### <a name="set-up-an-offer"></a>Einrichten eines Angebots

1. Melden Sie sich beim [Cloudpartnerportal](https://cloudpartner.azure.com/) an.

2. Wählen Sie im Navigationsbereich auf der linken Seite **+ Neues Angebot** > **Azure-Anwendungen**.

    ![Neues Angebot](./media/managed-application-author-marketplace/newOffer.png)

3. Füllen Sie die Formulare aus, die in der **Editor**-Ansicht auf der linken Seite angezeigt werden. Erforderliche Felder sind mit einem roten Sternchen gekennzeichnet (*).

    ![Angebotseinstellungen](./media/managed-application-author-marketplace/newOffer_OfferSettings.png)

    Zum Erstellen einer verwalteten Anwendung werden vier Hauptformulare verwendet:

    a. Angebotseinstellungen

    b. SKUs

    c. Marketplace

    d. Support

Diese Formen werden in den folgenden Abschnitten ausführlicher beschrieben.

## <a name="offer-settings-form"></a>Formular für Angebotseinstellungen
Verwenden Sie dieses grundlegende Formular, um die Angebotseinstellungen anzugeben.

1. Füllen Sie das Formular **Angebotseinstellungen** aus. Die anderen Felder sind:

    a. **Angebots-ID**: Dieser eindeutige Bezeichner gibt das Angebot innerhalb eines Herausgeberprofils an. Diese ID ist in den Produkt-URLs, den Ressourcen-Manager-Vorlagen und in den Abrechnungsberichten sichtbar. Sie darf nur aus klein geschriebenen alphanumerischen Zeichen oder Bindestrichen (-) bestehen. Die ID darf nicht mit einem Gedankenstrich enden. Sie ist auf maximal 50 Zeichen beschränkt. Nachdem ein Angebot online geschaltet wurde, wird dieses Feld gesperrt.

    b. **Herausgeber-ID**: In dieser Dropdownliste können Sie das Herausgeberprofil auswählen, unter dem das Angebot veröffentlicht werden soll. Nachdem ein Angebot online geschaltet wurde, wird dieses Feld gesperrt.

    c. **Name**: Dieser Anzeigename für Ihr Angebot wird im Marketplace und im Portal angezeigt. Er darf aus höchstens 50 Zeichen bestehen. Verwenden Sie einen Markennamen mit hohem Wiedererkennungswert für Ihr Produkt. Verwenden Sie an dieser Stelle nicht den Namen Ihres Unternehmens, es sei denn, das Produkt wird unter diesem Namen beworben. Wenn Sie dieses Angebot auf Ihrer eigenen Website bewerben, vergewissern Sie sich, dass der Name exakt mit dem auf Ihrer Website übereinstimmt.

2. Klicken Sie auf **Speichern**, um Ihren Fortschritt zu speichern. 

## <a name="skus-form"></a>Formular für SKUs
Als Nächstes fügen Sie SKUs für Ihr Angebot hinzu.

1. Wählen Sie **SKUs** > **Neue SKU**. 

    ![Auswählen einer neuen SKU](./media/managed-application-author-marketplace/newOffer_skus.png)

2. Geben Sie eine **SKU-ID** ein. Eine SKU-ID ist ein eindeutiger Bezeichner für die SKU in einem Angebot. Diese ID ist in den Produkt-URLs, den Ressourcen-Manager-Vorlagen und in den Abrechnungsberichten sichtbar. Sie darf nur aus klein geschriebenen alphanumerischen Zeichen oder Bindestrichen (-) bestehen. Die ID darf nicht mit einem Gedankenstrich enden und ist auf maximal 50 Zeichen beschränkt. Nachdem ein Angebot online geschaltet wurde, wird dieses Feld gesperrt. Innerhalb eines Angebots können Sie mehrere SKUs einschließen. Sie benötigen eine SKU für jedes Image, das Sie veröffentlichen möchten.

3. Füllen Sie im folgenden Formular den Abschnitt **SKU-Details** aus:

    ![Angeben einer neuen SKU](./media/managed-application-author-marketplace/newOffer_newsku.png)

    Füllen Sie folgende Felder aus:
    
    a. **Titel**: Geben Sie einen Titel für diese SKU ein. Dieser Titel wird im Katalog für dieses Element angezeigt.

    b. **Zusammenfassung**: Geben Sie eine kurze Zusammenfassung für diese SKU ein. Dieser Text wird unterhalb des Titels angezeigt.

    c. **Beschreibung**: Geben Sie eine ausführliche Beschreibung der SKU ein.

    d. **SKU-Typ**: Die zulässigen Werte sind **Verwaltete Anwendung** und **Lösungsvorlagen**. Wählen Sie für diesen Fall **verwaltete Anwendung** aus.

4. Füllen Sie im folgenden Formular den Abschnitt **Paketdetails** aus:

    ![Paket](./media/managed-application-author-marketplace/newOffer_newsku_package.png)

    Füllen Sie folgende Felder aus:

    a. **Aktuelle Version**: Geben Sie eine Version für das Paket ein, das Sie hochladen. Sie sollte folgendes Format aufweisen: `{number}.{number}.{number}{number}`.

    b. **Paketdatei auswählen**: Dieses Paket enthält die folgenden Dateien, die in einer ZIP-Datei komprimiert werden:
    * **applianceMainTemplate.json**: Die Bereitstellungsvorlagendatei, die zum Bereitstellen der Lösung/Anwendung verwendet wird. Informationen zum Erstellen von Bereitstellungsvorlagendateien finden Sie unter [Erstellen Ihrer ersten Azure Resource Manager-Vorlage](resource-manager-create-first-template.md).
    * **appliancecreateUIDefinition.json**: Diese Datei wird vom Azure-Portal verwendet, um die Benutzeroberfläche für die Bereitstellung dieser Lösung/Anwendung zu generieren. Weitere Informationen finden Sie unter [Erste Schritte mit CreateUiDefinition](managed-application-createuidefinition-overview.md).
    * **mainTemplate.json**: Vorlagendatei, die nur die Microsoft.Solution/appliances-Ressource enthält. Die Datei „mainTemplate“ weist die folgenden Eigenschaften auf:

        *  **kind**: Für verwaltete Anwendungen im Marketplace wird **Marketplace** verwendet.
        *  **ManagedResourceGroupId**: In dieser Ressourcengruppe im Abonnement des Kunden werden alle Ressourcen, die in der Datei „applianceMainTemplate.json“ definiert sind, bereitgestellt.
        *  **PublisherPackageId**: Diese Zeichenfolge identifiziert eindeutig das Paket. Geben Sie den Wert im Format `{publisherId}.{OfferId}.{SKUID}.{PackageVersion}` an.

Rufen Sie im Veröffentlichungsportal die in der folgenden Abbildung dargestellte **Angebots-ID** und **Herausgeber-ID** ab:

![Angebots-ID](./media/managed-application-author-marketplace/UniqueString_pubid_offerid.png)
        
Rufen Sie die **SKU-ID** ab, wie in der folgenden Abbildung gezeigt:

![SKU-ID](./media/managed-application-author-marketplace/UniqueString_skuid.png)
        
Rufen Sie die **Version** des Pakets ab, wie in der folgenden Abbildung gezeigt:

![Paketversion](./media/managed-application-author-marketplace/UniqueString_packageversion.png)
    
  Der Wert von **PublisherPackageId** lautet basierend auf den vorhergehenden Beispielen `azureappliance-test.ravmanagedapptest.ravpreviewmanagedsku.1.0.0`.

  Beispiel mainTemplate.json:

  ```json
  {
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
      "storageAccountNamePrefix": {
        "type": "string",
        "metadata": {
          "description": "Specify the name of the storage account"
        }
      },
      "storageAccountType": {
        "type": "string"
      }
    },
    "variables": {
      "managedResourceGroup": "[concat(resourceGroup().id,uniquestring(resourceGroup().id))]"
    },
    "resources": [{
      "type": "Microsoft.Solutions/appliances",
      "apiVersion": "2016-09-01-preview",
      "name": "[concat(parameters('storageAccountNamePrefix'), '-', 'managed')]",
      "location": "[resourceGroup().location]",
      "kind": "marketplace",
      "properties": {
        "managedResourceGroupId": "[variables('managedResourceGroup')]",
        "PublisherPackageId":"azureappliancetest.ravmanagedapptest.ravpreviewmanagedsku.1.0.0",
        "parameters": {
          "storageAccountName": {
            "value": "[parameters('storageAccountNamePrefix')]"
          },
          "storageAccountType": {
            "value": "[parameters('storageAccountType')]"
          }
        }
      }
    }],
    "outputs": {

    }
  }
  ```

Dieses Paket sollte alle anderen geschachtelten Vorlagen oder Skripts enthalten, die erforderlich sind, um diese Anwendung erfolgreich bereitzustellen. Im Stammordner müssen die Dateien „mainTemplate.json“, „applianceMainTemplate.json“ und „applianceCreateUIDefinition.json“ vorhanden sein.

* **Autorisierungen**: Diese Eigenschaft definiert, wer und auf welcher Ebene diese Person Zugriff auf Ressourcen in Abonnements von Kunden erhält. Dadurch kann der Herausgeber die Anwendung im Auftrag des Kunden verwalten.
* **PrincipalId**: Bei dieser Eigenschaft handelt es sich um den Azure Active Directory(Azure AD)-Bezeichner eines Benutzers, einer Benutzergruppe oder einer Anwendung, mit dem bestimmte Berechtigungen für die Ressourcen im Kundenabonnement gewährt werden. Die Berechtigungen werden in der Rollendefinition beschrieben. 
* **Rollendefinition**: Bei dieser Eigenschaft handelt es sich um eine Liste von allen integrierten rollenbasierten Zugriffssteuerungsrollen (RBAC), die von Azure AD bereitgestellt werden. Sie können die Rolle auswählen, die am besten für die Verwaltung der Ressourcen im Auftrag des Kunden geeignet ist.

Sie können mehrere Autorisierungen hinzufügen. Es wird empfohlen, eine AD-Benutzergruppe zu erstellen und die jeweilige ID unter **PrincipalId** anzugeben. Auf diese Weise können Sie der Benutzergruppe weitere Benutzer hinzufügen, ohne die SKU aktualisieren zu müssen.

Weitere Informationen zur RBAC finden Sie unter [Erste Schritte mit der RBAC im Azure-Portal](../active-directory/role-based-access-control-what-is.md).

## <a name="marketplace-form"></a>Marketplace-Formular

Das Marketplace-Formular verlangt nach Feldern, die im [Azure Marketplace](https://azuremarketplace.microsoft.com) und im [Azure-Portal](https://portal.azure.com/) erscheinen.

### <a name="preview-subscription-ids"></a>Preview-Abonnement-IDs

Geben Sie eine Liste der Azure-Abonnement-IDs ein, die auf das Angebot zugreifen können, nachdem es veröffentlicht wurde. Mithilfe dieser zugelassenen Abonnements können Sie das Angebot in der Vorschau testen, bevor es online geschaltet wird. Sie können eine Whitelist von bis zu 100 Abonnements im Partnerportal kompilieren.

### <a name="suggested-categories"></a>Vorgeschlagene Kategorien

Wählen Sie bis zu fünf Kategorien aus der Liste aus, denen Ihr Angebot am besten zugeordnet werden kann. Diese Kategorien werden verwendet, um Ihr Angebot den Produktkategorien zuzuordnen, die im [Azure Marketplace](https://azuremarketplace.microsoft.com) und im [Azure-Portal](https://portal.azure.com/) verfügbar sind.

#### <a name="azure-marketplace"></a>Azure Marketplace

In der Zusammenfassung Ihrer verwalteten Anwendung werden die folgenden Felder angezeigt:

![Marketplace-Zusammenfassung](./media/managed-application-author-marketplace/publishvm10.png)

Auf der Registerkarte **Übersicht** für Ihre verwaltete Anwendung werden die folgenden Felder angezeigt:

![Marketplace-Übersicht](./media/managed-application-author-marketplace/publishvm11.png)

Auf der Registerkarte **Pläne + Preise** für Ihre verwaltete Anwendung werden die folgenden Felder angezeigt:

![Marketplace-Pläne](./media/managed-application-author-marketplace/publishvm15.png)

#### <a name="azure-portal"></a>Azure-Portal

In der Zusammenfassung Ihrer verwalteten Anwendung werden die folgenden Felder angezeigt:

![Zusammenfassung Portal](./media/managed-application-author-marketplace/publishvm12.png)

In der Übersicht für Ihre verwaltete Anwendung werden die folgenden Felder angezeigt:

![Portalübersicht](./media/managed-application-author-marketplace/publishvm13.png)

#### <a name="logo-guidelines"></a>Richtlinien für Logos

Befolgen Sie die folgenden Richtlinien für Logos, die Sie im Cloudpartnerportal hochladen:

*   Die Farbpalette des Azure-Designs ist einfach und geradlinig. Beschränken Sie die Anzahl der Primär- und Sekundärfarben auf Ihrem Logo.
*   Die Designfarben des Azure-Portals sind Weiß und Schwarz. Verwenden Sie diese Farben nicht als Hintergrundfarbe für Ihr Logo. Verwenden Sie eine Farbe, die Ihr Logo im Portal ideal zur Geltung bringt. Sie sollten einfache Primärfarben verwenden. *Wenn Sie einen transparenten Hintergrund verwenden, stellen Sie sicher, dass das Logo und der Text nicht weiß, schwarz oder blau sind.*
*   Verwenden Sie im Logo keinen Hintergrund mit Farbverlauf.
*   Platzieren Sie keinen Text auf dem Logo, auch nicht Ihren Firmen- oder Markennamen. Das Erscheinungsbild Ihres Logos sollte klar und direkt und frei von Farbverläufen sein.
*   Stellen Sie sicher, dass das Logo nicht verzerrt wird.

#### <a name="hero-logo"></a>Herologo

Das Herologo ist optional. Der Herausgeber muss kein Herologo hochladen. Nachdem das Herosymbol hochgeladen wurde, kann es nicht gelöscht werden. Zu diesem Zeitpunkt muss der Partner die Marketplace-Richtlinien für Herosymbole einhalten.

Befolgen Sie die folgenden Richtlinien für das Herologosymbol:

*   Anzeigename des Herausgebers, Plantitel und ausführliche Angebotszusammenfassung werden in Weiß angezeigt. Verwenden Sie aus diesem Grund keine helle Farbe für den Hintergrund des Herosymbols. Ein schwarzer, weißer und transparenter Hintergrund ist für Herosymbole nicht zulässig.
*   Nach der Auflistung des Angebots werden Anzeigename des Herausgebers, der Plantitel, die ausführliche Angebotszusammenfassung und die Schaltfläche **Erstellen** programmgesteuert in das Herologo eingebettet. Geben Sie daher beim Entwurf des Herologos keinen Text ein. Lassen Sie auf der rechten Seite einen leeren Bereich, da der Text programmgesteuert an dieser Stelle eingefügt wird. Der leere Bereich für den Text sollte auf der rechten Seite 415 x 100 Pixel betragen. Er wird um 370 Pixel von links versetzt.

    ![Beispiel für Herologo](./media/managed-application-author-marketplace/publishvm14.png)

## <a name="support-form"></a>Supportformular

Füllen Sie das **Support**-Formular mit Supportkontakten von Ihrem Unternehmen aus. Bei diesen Informationen kann es sich um Kontakte des Entwicklerteams und Kontakte des Kundensupports handeln.

## <a name="publish-an-offer"></a>Veröffentlichen eines Angebots

Nachdem Sie alle Abschnitte ausgefüllt haben, wählen Sie **Veröffentlichen**, um den Vorgang zum Bereitstellen Ihres Angebots für Kunden zu starten.

## <a name="next-steps"></a>Nächste Schritte

* Eine Einführung in verwaltete Anwendungen finden Sie in der [Übersicht über verwaltete Anwendungen](managed-application-overview.md).
* Informationen zur Nutzung einer verwalteten Anwendung aus dem Marketplace finden Sie unter [Nutzen verwalteter Azure-Anwendungen im Marketplace](managed-application-consume-marketplace.md).
* Informationen zum Veröffentlichen einer verwalteten Dienstkataloganwendung finden Sie unter [Erstellen und Veröffentlichen einer verwalteten Dienstkataloganwendung](managed-application-publishing.md).
* Informationen zum Nutzen einer verwalteten Dienstkataloganwendung, finden Sie unter [Nutzen einer verwalteten Dienstkataloganwendung](managed-application-consumption.md).


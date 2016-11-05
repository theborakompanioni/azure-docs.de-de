---
title: Erste Schritte mit privaten Vorlagen | Microsoft Docs
description: Es wird beschrieben, wie Sie private Vorlagen mit dem Azure-Portal, der Azure-Befehlszeilenschnittstelle (CLI) oder PowerShell hinzufügen, verwalten und freigeben.
services: marketplace-customer
documentationcenter: ''
author: VybavaRamadoss
manager: asimm
editor: ''
tags: marketplace, azure-resource-manager
keywords: ''

ms.service: marketplace
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/18/2016
ms.author: vybavar

---
# Erste Schritte mit privaten Vorlagen im Azure-Portal
Eine [Azure Resource Manager](../resource-group-authoring-templates.md)-Vorlage ist eine deklarative Vorlage, die zum Definieren Ihrer Bereitstellung verwendet wird. Sie können die für eine Lösung bereitzustellenden Ressourcen definieren und Parameter und Variablen angeben, die die Eingabe von Werten für verschiedene Umgebungen ermöglichen. Die Vorlage besteht aus JSON und Ausdrücken, mit denen Sie Werte für die Bereitstellung erstellen können.

Sie können die neue Funktion **Vorlagen** im [Azure-Portal](https://portal.azure.com) zusammen mit dem **Microsoft.Gallery**-Ressourcenanbieter als Erweiterung von [Azure Marketplace](https://azure.microsoft.com/marketplace/) verwenden, damit Benutzer private Vorlagen aus einer persönlichen Bibliothek erstellen, verwalten und bereitstellen können.

In diesem Dokument werden Sie durch das Hinzufügen, Verwalten und Freigeben einer privaten **Vorlage** mit dem Azure-Portal geführt.

## Anleitungen
Die folgenden Vorschläge helfen Ihnen, bei der Arbeit mit Lösungen die Vorteile von **Vorlagen** zu nutzen:

* Eine **Vorlage** ist eine kapselnde Ressource, die eine Resource Manager-Vorlage und zusätzliche Metadaten enthält. Sie verhält sich ähnlich wie ein Element im Marketplace. Der wesentliche Unterschied besteht darin, dass es im Gegensatz zu den öffentlichen Marketplace-Elementen ein privates Element ist.
* Die Bibliothek **Vorlagen** eignet sich gut für Benutzer, die die Bereitstellung anpassen müssen.
* **Vorlagen** sind auch gut für Benutzer geeignet, die in Azure ein einfaches Repository benötigen.
* Beginnen Sie mit einer vorhandenen Resource Manager-Vorlage. Suchen Sie bei [GitHub](https://github.com/Azure/azure-quickstart-templates) nach Vorlagen, oder [exportieren Sie Vorlagen](../resource-manager-export-template.md) aus einer vorhandenen Ressourcengruppe.
* **Vorlagen** sind an den Benutzer gebunden, von dem sie veröffentlicht werden. Der Herausgebername ist für alle Benutzer sichtbar, die über Lesezugriff darauf verfügen.
* **Vorlagen** sind Resource Manager-Ressourcen und können nach dem Veröffentlichen nicht mehr umbenannt werden.

## Hinzufügen einer Vorlagenressource
Es gibt zwei Methoden zum Erstellen einer Ressource vom Typ **Vorlage** im Azure-Portal.

### Methode 1: Erstellen einer neuen Vorlagenressource aus einer ausgeführten Ressourcengruppe
1. Navigieren Sie im Azure-Portal zu einer vorhandenen Ressourcengruppe. Wählen Sie unter **Einstellungen** die Option **Vorlage exportieren**.
2. Nachdem die Resource Manager-Vorlage exportiert wurde, können Sie die Schaltfläche **Vorlage speichern** verwenden, um sie im Repository **Vorlagen** zu speichern. Alle Details zum Exportieren der Vorlage finden Sie [hier](../resource-manager-export-template.md). <br /><br /> ![Ressourcengruppe exportieren](media/rg-export-portal1.PNG) <br />
3. Wählen Sie die Befehlsschaltfläche **Save to Template** (In Vorlage speichern). <br /><br />
4. Geben Sie Folgendes ein:
   
   * Name: Name des Vorlagenobjekts (HINWEIS: Dieser Name basiert auf Azure Resource Manager. Es gelten alle Beschränkungen der Namensgebung, und er kann nach der Erstellung nicht mehr geändert werden.)
   * Beschreibung: Kurze Zusammenfassung der Vorlage
     
     ![Vorlage speichern](media/save-template-portal1.PNG) <br />
5. Klicken Sie auf **Speichern**.
   
   > [!NOTE]
   > Auf dem Blatt „Vorlage exportieren“ werden Benachrichtigungen angezeigt, wenn die exportierte Resource Manager-Vorlage Fehler enthält. Sie können diese Resource Manager-Vorlage aber trotzdem unter „Vorlagen“ speichern. Sie sollten alle Probleme von Resource Manager-Vorlagen untersuchen und beheben, bevor Sie die exportierte Resource Manager-Vorlage erneut bereitstellen.
   > 
   > 

### B. Methode 2 : Hinzufügen einer neuen Vorlagenressource per „Durchsuchen“
Sie können eine **Vorlage** auch ganz neu hinzufügen, indem Sie unter **Durchsuchen > Vorlagen** die Befehlsschaltfläche „+Hinzufügen“ verwenden. Sie müssen Name, Beschreibung und den JSON-Code für die Resource Manager-Vorlage angeben.

![Vorlage hinzufügen](media/add-template-portal1.PNG) <br />

> [!NOTE]
> Microsoft.Gallery ist ein mandantenbasierter Azure-Ressourcenanbieter. Die Vorlagenressource ist an den Benutzer gebunden, der sie erstellt hat. Sie ist nicht an ein bestimmtes Abonnement gebunden. Sie müssen nur beim Bereitstellen einer Vorlage ein Abonnement auswählen.
> 
> 

## Anzeigen von Vorlagenressourcen
Sie können alle **Vorlagen**, die für Sie verfügbar sind, unter **Durchsuchen > Vorlagen** anzeigen. Darin sind **Vorlagen** enthalten, die Sie erstellt haben, sowie Vorlagen, die für Sie mit wechselnden Berechtigungsebenen freigegeben wurden. Weitere Informationen finden Sie unten im Abschnitt [Zugriffssteuerung](#access-control-for-a-tenant-resource-provider).

![Vorlage anzeigen](media/view-template-portal1.PNG) <br />

Sie können die Details einer **Vorlage** anzeigen, indem Sie in der Liste auf einen Eintrag klicken.

![Vorlage anzeigen](media/view-template-portal2c.png) <br />

## Bearbeiten einer Vorlagenressource
Sie können den Bearbeitungsfluss für eine **Vorlage** initiieren, indem Sie in der Liste „Durchsuchen“ mit der rechten Maustaste auf den Eintrag klicken oder die Befehlsschaltfläche „Bearbeiten“ wählen.

![Vorlage bearbeiten](media/edit-template-portal1a.PNG) <br />

Sie können die Beschreibung oder den Text der Resource Manager-Vorlage bearbeiten. Den Namen können Sie nicht bearbeiten, da es ein Resource Manager-Ressourcenname ist. Wenn Sie den JSON-Code der Resource Manager-Vorlage bearbeiten, stellen wir anhand einer Überprüfung sicher, dass es sich um gültigen JSON-Code handelt. Wählen Sie **OK** und dann **Speichern**, um die aktualisierte Vorlage zu speichern.

![Vorlage bearbeiten](media/edit-template-portal2a.PNG) <br />

Nachdem die **Vorlage** gespeichert wurde, wird eine Bestätigungsbenachrichtigung angezeigt.

![Vorlage bearbeiten](media/edit-template-portal3b.png) <br />

## Bereitstellen einer Vorlagenressource
Sie können alle **Vorlagen** bereitstellen, für die Sie über die Berechtigung **Lesen** verfügen. Während des Bereitstellungsvorgangs wird das standardmäßige Blatt für die Azure-Vorlagenbereitstellung gestartet. Geben Sie die Werte für die Resource Manager-Vorlagenparameter an, um mit der Bereitstellung fortzufahren.

![Vorlage bereitstellen](media/deploy-template-portal1b.png) <br />

## Freigeben einer Vorlagenressource
Sie können eine **Vorlagen**ressource für andere Benutzer freigeben. Das Verhalten bei der Freigabe ähnelt der [Rollenzuweisung für Ressourcen unter Azure](../active-directory/role-based-access-control-configure.md). Der Besitzer der **Vorlage** gewährt anderen Benutzern Berechtigungen zur Interaktion mit einer Vorlagenressource. Die Person oder Gruppe, für die Sie die **Vorlage** freigeben, kann die Resource Manager-Vorlage und die Katalogeigenschaften anzeigen.

### Zugriffssteuerung für die Microsoft.Gallery-Ressourcen
| Rolle | Berechtigungen |
| --- | --- |
| Besitzer |Ermöglicht die vollständige Kontrolle über die Vorlagenressource, einschließlich der Freigabe. |
| Leser |Ermöglicht das Lesen und Ausführen (Bereitstellen) der Vorlagenressource. |
| Mitwirkender |Ermöglicht das Bearbeiten und Löschen der Vorlagenressource. Der Benutzer kann die Vorlage nicht für andere Benutzer freigeben. |

Wählen Sie für den Eintrag die Option **Freigeben**, indem Sie mit der rechten Maustaste darauf klicken oder das Blatt „Ansicht“ für ein Element verwenden. Der Bereich für die Freigabe wird gestartet.

![Vorlage freigeben](media/share-template-portal1a.png) <br />

 Sie können jetzt eine Rolle und einen Benutzer oder eine Gruppe auswählen, um Zugriff auf eine bestimmte **Vorlage** zu gewähren. Die verfügbaren Rollen sind „Besitzer“, „Leser“ und „Mitwirkender“. Weitere Informationen finden Sie oben im Abschnitt [Zugriffssteuerung](#access-control-for-a-tenant-resource-provider).

![Vorlage freigeben](media/share-template-portal2b.png) <br />

![Vorlage freigeben](media/share-template-portal3b.png) <br />

Klicken Sie auf **Auswählen** und **OK**. Die Benutzer oder Gruppen, die Sie hinzugefügt haben, werden nun angezeigt.

![Vorlage freigeben](media/share-template-portal4b.png) <br />

> [!NOTE]
> Eine Vorlage kann nur für Benutzer und Gruppen freigegeben werden, die sich unter demselben Azure Active Directory-Mandanten befinden. Wenn Sie eine Vorlage für eine E-Mail-Adresse freigeben, die sich nicht unter Ihrem Mandanten befindet, wird eine Einladung gesendet. Darin wird der Benutzer aufgefordert, dem Mandanten als Gast beizutreten.
> 
> 

## Nächste Schritte
* Weitere Informationen zum Erstellen von Resource Manager-Vorlagen finden Sie unter [Erstellen von Vorlagen](../resource-group-authoring-templates.md).
* Grundlegende Informationen zu den Funktionen, die Sie in einer Resource Manager-Vorlage verwenden können, finden Sie unter [Vorlagenfunktionen](../resource-group-template-functions.md).
* Anleitungen zum Entwerfen von Vorlagen finden Sie unter [Bewährte Methoden für das Entwerfen von Azure-Ressourcen-Manager-Vorlagen](../best-practices-resource-manager-design-templates.md).

<!---HONumber=AcomDC_0629_2016-->
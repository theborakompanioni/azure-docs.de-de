---
title: "Azure-Portal für Ressourcenrichtlinien | Microsoft-Dokumentation"
description: "Beschreibt, wie das Azure-Portal zum Erstellen und Verwalten von Resource Manager-Richtlinien verwendet wird. Richtlinien können auf das Abonnement oder auf Ressourcengruppen angewendet werden."
services: azure-resource-manager
documentationcenter: na
author: tfitzmac
manager: timlt
editor: tysonn
ms.assetid: 
ms.service: azure-resource-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 03/30/2017
ms.author: tomfitz
translationtype: Human Translation
ms.sourcegitcommit: 197ebd6e37066cb4463d540284ec3f3b074d95e1
ms.openlocfilehash: 868b2cc1559053057d17b34c03e2e31347f399bf
ms.lasthandoff: 03/31/2017


---
# <a name="use-azure-portal-to-assign-and-manage-resource-policies"></a>Verwenden des Azure-Portals zum Zuweisen und Verwalten von Ressourcenrichtlinien
Im Azure-Portal können Sie Ressourcengruppen und Abonnements Ressourcenrichtlinien zuweisen. Die Benutzeroberfläche erleichtert die Auswahl der Richtlinie, die Sie zuweisen möchten, und die Festlegung der Parameterwerte für diese Richtlinie zum Anpassen der Richtlinieneinstellungen. 

Um eine Richtlinie über das Portal zuzuweisen, muss die Richtliniendefinition bereits in Ihrem Abonnement vorhanden sein. Das Abonnement umfasst mehrere integrierte Richtliniendefinitionen, die Sie Ressourcengruppen oder Abonnements zuweisen können. Sie sehen diese integrierten Richtlinien und jegliche benutzerdefinierten Richtlinien, die Sie definiert haben, wenn Sie das Verwaltungsportal verwenden, um Richtlinien zuzuweisen. Eine Einführung zu Richtlinien und dazu, wie Sie eine benutzerdefinierte Richtlinie definieren, finden Sie in der [Übersicht über Ressourcenrichtlinien](resource-manager-policy.md).

Richtlinien werden von allen untergeordneten Ressourcen geerbt. Wenn also eine Richtlinie auf eine Ressourcengruppe angewendet wird, gilt sie auch für alle Ressourcen in der Ressourcengruppe. In diesem Artikel bezieht sich der Begriff **Bereich** auf die Ressourcengruppe oder das Abonnement, der/dem die Richtlinie zugewiesen wird. 

Richtlinien werden beim Erstellen und Aktualisieren von Ressourcen (PUT- und PATCH-Vorgänge) ausgewertet.

## <a name="assign-a-policy"></a>Zuweisen einer Richtlinie

1. Um einer Ressourcengruppe oder einem Abonnement eine Richtlinie zuzuweisen, wählen Sie die Ressourcengruppe bzw. das Abonnement aus. Wählen Sie in den Einstellungen **Richtlinien**.

   ![Richtlinien auswählen](./media/resource-manager-policy-portal/select-policies.png)

2. Wählen Sie zum Erstellen einer Richtlinienzuweisung für diesen Bereich **Zuweisung hinzufügen**.

   ![Zuweisung hinzufügen](./media/resource-manager-policy-portal/add-assignment.png)

3. Wählen Sie die Richtlinie, die Sie zuweisen möchten. Auch wenn Sie Ihrem Abonnement keine Richtliniendefinitionen hinzugefügt haben, sehen Sie die integrierten Richtlinien, die für die Zuweisung verfügbar sind. Diese integrierten Richtlinien decken viele häufige Szenarien ab.

   ![Definition wählen](./media/resource-manager-policy-portal/select-definition.png)

4. Nach Auswahl einer Richtlinie werden eine Beschreibung der Richtlinie und alle Parameter für diese Richtlinie angezeigt. Die folgende Abbildung zeigt z.B. den für die Richtlinie erforderlichen Parameter **Zulässige Speicherorte** an, der die verfügbaren Speicherorte einschränkt.

   ![Parameter anzeigen](./media/resource-manager-policy-portal/show-parameters.png)

5. Wählen Sie über die Benutzeroberfläche die für die Richtlinienparameter festzulegenden Werte (z.B. die Speicherorte, die für die Bereitstellung verwendet werden können).

   ![Parameterwerte wählen](./media/resource-manager-policy-portal/select-parameters.png)

6. Geben Sie Werte für die anderen Parameter an. Der Bereich wird basierend auf dem Blatt, das Sie beim Beginn der Richtlinienzuweisung ausgewählt haben, automatisch zugewiesen. Wählen Sie nach Abschluss des Vorgangs **OK** .

   ![Parameter definieren](./media/resource-manager-policy-portal/define-parameters.png)

  Sie haben die Richtlinie dem angegebenen Bereich zugewiesen.

## <a name="view-policy-assignments"></a>Anzeigen der Richtlinienzuweisungen

Nach dem Zuweisen wird eine Richtlinie in der Liste der Richtlinien für diesen Bereich angezeigt. Die Registerkarte **Details** enthält eine Übersicht der Richtlinienzuweisung.

![Details anzeigen](./media/resource-manager-policy-portal/show-details.png)

Die Registerkarte **Zuweisungsregel** zeigt den JSON-Code für die Richtlinie.

![Zuweisungsregel anzeigen](./media/resource-manager-policy-portal/show-assignment-rule.png)

## <a name="change-an-existing-policy-assignment"></a>Ändern einer vorhandenen Richtlinienzuweisung

Wählen Sie zum Ändern einer Richtlinie **Zuweisung bearbeiten** oder **Löschen**.

![Zuweisung bearbeiten oder löschen](./media/resource-manager-policy-portal/edit-delete-policy.png)

## <a name="assign-custom-policies"></a>Zuweisen von benutzerdefinierten Richtlinien

Wenn Sie benutzerdefinierte Richtlinien in Ihrem Abonnement definiert haben, sind diese Richtlinien für die Zuweisung über das Portal verfügbar. Diesen Richtlinien ist **[Custom]** vorangestellt.

![Benutzerdefinierte Richtlinien](./media/resource-manager-policy-portal/show-custom-policy.png)

## <a name="next-steps"></a>Nächste Schritte
* Weitere Informationen zur JSON-Syntax zum Definieren von Richtlinien finden Sie unter [Übersicht über Ressourcenrichtlinien](resource-manager-policy.md).
* Anleitungen dazu, wie Unternehmen Abonnements mit Resource Manager effektiv verwalten können, finden Sie unter [Azure-Unternehmensgerüst - Präskriptive Abonnementgovernance](resource-manager-subscription-governance.md).
* Das Richtlinienschema wird unter [http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json](http://schema.management.azure.com/schemas/2015-10-01-preview/policyDefinition.json)veröffentlicht. 



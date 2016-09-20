<properties
	pageTitle="Benutzerdefinierte Rollen in Azure RBAC | Microsoft Azure"
	description="Erfahren Sie, wie Sie benutzerdefinierte Rollen mit der rollenbasierten Zugriffssteuerung in Azure für eine präzisere Identitätsverwaltung in Ihrem Azure-Abonnement definieren."
	services="active-directory"
	documentationCenter=""
	authors="kgremban"
	manager="kgremban"
	editor=""/>

<tags
	ms.service="active-directory"
	ms.devlang="na"
	ms.topic="article"
	ms.tgt_pltfrm="na"
	ms.workload="identity"
	ms.date="07/25/2016"
	ms.author="kgremban"/>


# Benutzerdefinierte Rollen in Azure RBAC


Erstellen Sie eine benutzerdefinierte Rolle in der rollenbasierten Zugriffssteuerung von Azure (Role-Based Access Control, RBAC), falls keine der integrierten Rollen Ihre speziellen Zugriffsanforderungen erfüllt. Benutzerdefinierte Rollen können mit [Azure PowerShell](role-based-access-control-manage-access-powershell.md), der [Azure-Befehlszeilenschnittstelle](role-based-access-control-manage-access-azure-cli.md) (Command-Line Interface, CLI) und der [REST-API](role-based-access-control-manage-access-rest.md) erstellt werden. Genau wie integrierte Rollen können auch benutzerdefinierte Rollen Benutzern, Gruppen und Anwendungen auf Abonnement-, Ressourcengruppen- und Ressourcenebene zugewiesen werden. Benutzerdefinierte Rollen werden in einem Azure AD-Mandanten gespeichert und können von allen Abonnements gemeinsam genutzt werden, die diesen Mandanten als Azure AD-Verzeichnis für das Abonnement nutzen.

Es folgt ein Beispiel für eine benutzerdefinierte Rolle zum Überwachen und Neustarten virtueller Computer:

```
{
  "Name": "Virtual Machine Operator",
  "Id": "cadb4a5a-4e7a-47be-84db-05cad13b6769",
  "IsCustom": true,
  "Description": "Can monitor and restart virtual machines.",
  "Actions": [
    "Microsoft.Storage/*/read",
    "Microsoft.Network/*/read",
    "Microsoft.Compute/*/read",
    "Microsoft.Compute/virtualMachines/start/action",
    "Microsoft.Compute/virtualMachines/restart/action",
    "Microsoft.Authorization/*/read",
    "Microsoft.Resources/subscriptions/resourceGroups/read",
    "Microsoft.Insights/alertRules/*",
    "Microsoft.Insights/diagnosticSettings/*",
    "Microsoft.Support/*"
  ],
  "NotActions": [

  ],
  "AssignableScopes": [
    "/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e",
    "/subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624",
    "/subscriptions/34370e90-ac4a-4bf9-821f-85eeedeae1a2"
  ]
}
```
## Actions
Mit der **Actions**-Eigenschaft einer benutzerdefinierten Rolle werden die Azure-Vorgänge angegeben, auf die mit der Rolle Zugriff gewährt wird. Es handelt sich um eine Sammlung von Vorgangszeichenfolgen, mit denen sicherungsfähige Vorgänge von Azure-Ressourcenanbietern identifiziert werden. Vorgangszeichenfolgen mit Platzhaltern (*) gewähren Zugriff auf alle Vorgänge, die mit der Vorgangszeichenfolge übereinstimmen. Beispiel:

-	`*/read` gewährt Zugriff auf Lesevorgänge für alle Ressourcentypen aller Azure-Ressourcenanbieter.
-	`Microsoft.Network/*/read` gewährt Zugriff auf Lesevorgänge für alle Ressourcentypen im Microsoft.Network-Ressourcenanbieter von Azure.
-	`Microsoft.Compute/virtualMachines/*` gewährt Zugriff auf alle Vorgänge virtueller Maschinen und die dazugehörigen untergeordneten Ressourcentypen.
-	`Microsoft.Web/sites/restart/Action` gewährt Zugriff zum Neustarten von Websites.

Verwenden Sie `Get-AzureRmProviderOperation` (in PowerShell) oder `azure provider operations show` (in Azure-CLI) zum Auflisten von Vorgängen von Azure-Ressourcenanbietern. Sie können diese Befehle auch verwenden, um zu überprüfen, ob eine Vorgangszeichenfolge gültig ist, und um Zeichenfolgen für Platzhaltervorgänge zu erweitern.

```
Get-AzureRMProviderOperation Microsoft.Compute/virtualMachines/*/action | FT Operation, OperationName

Get-AzureRMProviderOperation Microsoft.Network/*
```

![PowerShell-Screenshot: Get-AzureRMProviderOperation Microsoft.Compute/virtualMachines/*/action | FT Operation, OperationName](./media/role-based-access-control-configure/1-get-azurermprovideroperation-1.png)

```
azure provider operations show "Microsoft.Compute/virtualMachines/*/action" --js on | jq '.[] | .operation'

azure provider operations show "Microsoft.Network/*"
```

![Azure-Befehlszeilenschnittstelle – Screenshot: azure provider operations show „Microsoft.Compute/virtualMachines/*/action“](./media/role-based-access-control-configure/1-azure-provider-operations-show.png)

## NotActions
Verwenden Sie die **NotActions**-Eigenschaft, wenn der Satz von Vorgängen, die Sie zulassen möchten, durch Ausschließen eingeschränkter Vorgänge leichter zu definieren ist. Der von einer benutzerdefinierten Rolle gewährte Zugriff wird berechnet, indem die **NotActions**-Vorgänge von den **Actions**-Vorgängen abgezogen werden.

> [AZURE.NOTE] Wenn einem Benutzer eine Rolle zugewiesen wird, mit der ein Vorgang in **NotActions** ausgeschlossen wird, und dem Benutzer dann durch Zuweisen einer zweiten Rolle der Zugriff auf denselben Vorgang gewährt wird, kann der Benutzer den Vorgang durchführen. **NotActions** ist keine Verweigerungsregel. Es ist lediglich eine bequeme Möglichkeit, eine Gruppe zulässiger Vorgänge zu erstellen, wenn bestimmte Vorgänge ausgeschlossen werden müssen.

## AssignableScopes
Mit der **AssignableScopes**-Eigenschaft der benutzerdefinierten Rolle werden die Bereiche (Abonnements, Ressourcengruppen oder Ressourcen) angegeben, in denen die benutzerdefinierte Rolle für die Zuweisung verfügbar ist. Sie können die Verfügbarkeit der benutzerdefinierten Rolle für die Zuweisung auf die Abonnements oder Ressourcengruppen beschränken, für die dies erforderlich ist. Dadurch wird eine bessere Übersichtlichkeit der restlichen Abonnements oder Ressourcengruppen erzielt.

Beispiele für gültige zuweisbare Bereiche:

-	„/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e“ und „/subscriptions/e91d47c4-76f3-4271-a796-21b4ecfe3624“: Macht die Rolle für die Zuweisung in zwei Abonnements verfügbar.
-	„/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e“: Macht die Rolle für die Zuweisung in einem einzelnen Abonnement verfügbar.
-  „/subscriptions/c276fc76-9cd4-44c9-99a7-4fd71546436e/resourceGroups/Network“: Macht die Rolle für die alleinige Zuweisung in der Netzwerkressourcengruppe verfügbar.

> [AZURE.NOTE] Sie müssen mindestens ein Abonnement, eine Ressourcengruppe oder eine Ressourcen-ID verwenden.

## Zugriffssteuerung für benutzerdefinierte Rollen
Mit der **AssignableScopes**-Eigenschaft der benutzerdefinierten Rolle wird auch gesteuert, wer die Rolle anzeigen, ändern und löschen kann.

- Wer kann eine benutzerdefinierte Rolle erstellen? Besitzer (und Benutzerzugriffsadministratoren) von Abonnements, Ressourcengruppen und Ressourcen können benutzerdefinierte Rollen für die Verwendung in diesen Bereichen erstellen. Der Ersteller der Rolle muss in der Lage sein, den `Microsoft.Authorization/roleDefinition/write`-Vorgang für alle **AssignableScopes**-Elemente der Rolle auszuführen.

- Wer kann eine benutzerdefinierte Rolle ändern? Besitzer (und Benutzerzugriffsadministratoren) von Abonnements, Ressourcengruppen und Ressourcen können benutzerdefinierte Rollen in diesen Bereichen ändern. Benutzer müssen in der Lage sein, den `Microsoft.Authorization/roleDefinition/write`-Vorgang für alle **AssignableScopes**-Elemente einer benutzerdefinierten Rolle auszuführen.

- Wer kann benutzerdefinierte Rollen anzeigen? Alle integrierten Rollen in Azure RBAC ermöglichen das Anzeigen von Rollen, die für die Zuweisung verfügbar sind. Benutzer, die den Vorgang `Microsoft.Authorization/roleDefinition/read` für einen Bereich durchführen können, können die RBAC-Rollen anzeigen, die für die Zuweisung in diesem Bereich verfügbar sind.

## Siehe auch
- [Rollenbasierte Zugriffssteuerung](role-based-access-control-configure.md): Erste Schritte mit RBAC im Azure-Portal.
- Informationen zur Zugriffsverwaltung mit:
	- [PowerShell](role-based-access-control-manage-access-powershell.md)
	- [Azure-Befehlszeilenschnittstelle](role-based-access-control-manage-access-azure-cli.md)
	- [REST-API](role-based-access-control-manage-access-rest.md)
- [Integrierte Rollen:](role-based-access-built-in-roles.md) Hier erhalten Sie ausführliche Informationen zu den Standardrollen in RBAC.

<!---HONumber=AcomDC_0907_2016-->
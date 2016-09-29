<properties
	pageTitle="Ändern der Mandanten-ID des Schlüsseltresors nach einer Abonnementverschiebung | Microsoft Azure"
	description="Es wird beschrieben, wie Sie die Mandanten-ID für einen Schlüsseltresor ändern, nachdem ein Abonnement in einen anderen Mandanten verschoben wurde."
	services="key-vault"
	documentationCenter=""
	authors="amitbapat"
	manager="mbaldwin"
	tags="azure-resource-manager"/>

<tags
	ms.service="key-vault"
	ms.workload="identity"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="hero-article"
	ms.date="09/13/2016"
	ms.author="ambapat"/>

# Ändern der Mandanten-ID des Schlüsseltresors nach einer Abonnementverschiebung
### F: Mein Abonnement wurde von Mandant A zu Mandant B verschoben. Wie ändere ich die Mandanten-ID für meinen vorhandenen Schlüsseltresor und lege für Mandant B die richtigen ACLs für Prinzipale fest?

Wenn Sie in einem Abonnement einen neuen Schlüsseltresor erstellen, wird er automatisch an die standardmäßige Azure Active Directory-Mandanten-ID für das Abonnement gebunden. Außerdem werden auch alle Zugriffsrichtlinieneinträge an diese Mandanten-ID gebunden. Wenn Sie Ihr Azure-Abonnement aus Mandant A in Mandant B verschieben, können die Prinzipale (Benutzer und Anwendungen) in Mandant B nicht auf Ihre vorhandenen Schlüsseltresore zugreifen. Gehen Sie wie folgt vor, um dies zu beheben:

- Ändern Sie die Mandanten-ID, die allen vorhandenen Schlüsseltresoren des Abonnements zugeordnet ist, in den Mandanten B.
- Entfernen Sie alle vorhandenen Zugriffsrichtlinieneinträge.
- Fügen Sie neue Zugriffsrichtlinieneinträge hinzu, die Mandant B zugeordnet sind.

Wenn beispielsweise der Schlüsseltresor „myvault“ in einem Abonnement enthalten ist, das von Mandant A zu Mandant B verschoben wurde, können Sie die Mandanten-ID für diesen Schlüsseltresor ändern und alte Zugriffsrichtlinien entfernen.

<pre>
$vaultResourceId = (Get-AzureRmKeyVault -VaultName myvault).ResourceId
$vault = Get-AzureRmResource –ResourceId $vaultResourceId -ExpandProperties
$vault.Properties.TenantId = (Get-AzureRmContext).Tenant.TenantId
$vault.Properties.AccessPolicies = @()
Set-AzureRmResource -ResourceId $vaultResourceId -Properties $vault.Properties
</pre>

Da sich dieser Tresor vor der Verschiebung unter Mandant A befunden hat, lautet der ursprüngliche Wert von **$vault.Properties.TenantId** „Mandant A“, während er für **(Get-AzureRmContext).Tenant.TenantId** „Mandant B“ lautet.

Nachdem Sie Ihren Tresor nun der richtigen Mandanten-ID zugeordnet haben und alte Zugriffsrichtlinieneinträge entfernt wurden, können Sie neue Zugriffsrichtlinieneinträge mit [Set-AzureRmKeyVaultAccessPolicy](https://msdn.microsoft.com/library/mt603625.aspx) festlegen.

## Nächste Schritte

- Besuchen Sie die [Azure Key Vault-Foren](https://social.msdn.microsoft.com/forums/azure/home?forum=AzureKeyVault), wenn Sie Fragen zum Schlüsseltresor haben.

<!---HONumber=AcomDC_0921_2016-->
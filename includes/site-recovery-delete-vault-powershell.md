## <a name="delete-a-recovery-services-vault-powershell"></a>Löschen eines Recovery Services-Tresors (PowerShell)

1. Beschaffen des Recovery Services-Tresors

        $vault = Get-AzureRmRecoveryServicesVault -Name "ContosoVault"

2. Löschen des Tresors

        Remove-AzureRmRecoveryServicesVault -Vault $vault

>[!WARNING]
>
> Verwenden Sie den obigen Befehl mit äußerster Vorsicht, da alle Daten verloren gehen, wenn Sie einen Tresor versehentlich löschen. Dies ist ein endgültiger Vorgang, der nicht rückgängig gemacht werden kann.  



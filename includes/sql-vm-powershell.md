
## <a name="start-your-powershell-session"></a>Starten der PowerShell-Sitzung
Zuallererst muss die aktuelle Version der [Azure PowerShell](http://msdn.microsoft.com/library/mt619274.aspx) installiert sein und ausgeführt werden. Weitere Informationen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azureps-cmdlets-docs).

> [!NOTE]
> In den Beispielen in diesem Thema wird das [Azure Resource Manager-Bereitstellungsmodell](../articles/azure-resource-manager/resource-group-overview.md) verwendet. Daher kommen die [Azure Resource Manager-Cmdlets](http://msdn.microsoft.com/library/azure/mt125356.aspx) zum Einsatz. 
> 
> 

Führen Sie das Cmdlet [**Add-AzureRmAccount**](http://msdn.microsoft.com/library/mt619267.aspx) aus. Daraufhin wird eine Anmeldeseite angezeigt, auf der Sie Ihre Anmeldeinformationen eingeben müssen. Verwenden Sie die gleichen Anmeldeinformationen wie für die Anmeldung beim Azure-Portal.

    Add-AzureRmAccount

Wenn Sie über mehrere Abonnements verfügen, wählen Sie mit dem Cmdlet [**Set-AzureRmContext**](http://msdn.microsoft.com/library/mt619263.aspx) das Abonnement aus, das für die PowerShell-Sitzung verwendet werden soll. Führen Sie das Cmdlet [**Get-AzureRmContext**](http://msdn.microsoft.com/library/mt619265.aspx) aus, um das Abonnement anzuzeigen, das für die aktuelle PowerShell-Sitzung verwendet wird. Um alle Ihre Abonnements anzuzeigen, führen Sie [**Get-AzureRmSubscription**](http://msdn.microsoft.com/library/mt619284.aspx) aus.

    Set-AzureRmContext -SubscriptionId '4cac86b0-1e56-bbbb-aaaa-000000000000'


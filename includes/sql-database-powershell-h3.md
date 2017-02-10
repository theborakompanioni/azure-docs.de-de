
### <a name="start-your-powershell-session"></a>Starten der PowerShell-Sitzung
Zuallererst muss die aktuelle Version der [Azure PowerShell](https://msdn.microsoft.com/library/mt619274\(v=azure.300\).aspx) installiert sein und ausgeführt werden. Weitere Informationen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](/powershell/azureps-cmdlets-docs).

> [!NOTE]
> Viele neue Funktionen von SQL-Datenbank werden nur bei Verwendung des [Azure Resource Manager-Bereitstellungsmodells](../articles/azure-resource-manager/resource-group-overview.md) unterstützt. Daher werden in den Beispielen die [Azure SQL-Datenbank-PowerShell-Cmdlets](https://msdn.microsoft.com/library/azure/mt574084\(v=azure.300\).aspx) für Resource Manager verwendet. Die [Azure SQL-Datenbank-Cmdlets zur Dienstverwaltung](https://msdn.microsoft.com/library/azure/dn546723\(v=azure.300\).aspx) des klassischen Bereitstellungsmodells werden für Abwärtskompatibilität zwar unterstützt, aber dennoch wird die Verwendung der Resource Manager-Cmdlets empfohlen.
> 
> 

Führen Sie das Cmdlet [**Add-AzureRmAccount**](https://msdn.microsoft.com/library/azure/mt619267\(v=azure.300\).aspx) aus. Daraufhin wird eine Anmeldeseite angezeigt, auf der Sie Ihre Anmeldeinformationen eingeben müssen. Verwenden Sie die gleichen Anmeldeinformationen wie für die Anmeldung beim Azure-Portal.

    Add-AzureRmAccount

Wenn Sie über mehrere Abonnements verfügen, wählen Sie mit dem Cmdlet [**Set-AzureRmContext**](https://msdn.microsoft.com/library/azure/mt619263\(v=azure.300\).aspx) das Abonnement aus, das für die PowerShell-Sitzung verwendet werden soll. Führen Sie das Cmdlet [**Get-AzureRmContext**](https://msdn.microsoft.com/library/azure/mt619265\(v=azure.300\).aspx) aus, um das Abonnement anzuzeigen, das für die aktuelle PowerShell-Sitzung verwendet wird. Um alle Ihre Abonnements anzuzeigen, führen Sie [**Get-AzureRmSubscription**](https://msdn.microsoft.com/library/azure/mt619284\(v=azure.300\).aspx) aus.

    Set-AzureRmContext -SubscriptionId '4cac86b0-1e56-bbbb-aaaa-000000000000'


<!--HONumber=Dec16_HO2-->



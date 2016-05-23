
## Starten der PowerShell-Sitzung

Als erste Voraussetzung muss [Azure PowerShell](https://msdn.microsoft.com/library/mt619274.aspx) (1.0 oder später) installiert und ausgeführt werden. Weitere Informationen finden Sie unter [Installieren und Konfigurieren von Azure PowerShell](../articles/powershell-install-configure.md).


>[AZURE.NOTE] Viele neue Funktionen von SQL-Datenbank werden nur unter Verwendung des [Azure Resource Manager-Bereitstellungsmodells (ARM)](../articles/resource-group-overview.md) unterstützt. Daher werden in den Beispielen ARM-basierte [Azure SQL-Datenbank-PowerShell-Cmdlets](https://msdn.microsoft.com/library/azure/mt574084.aspx) verwendet. Die vorhandenen [(klassischen) Azure SQL-Datenbank-Cmdlets](https://msdn.microsoft.com/library/azure/dn546723.aspx) des klassischen Bereitstellungsmodells werden für die Abwärtskompatibilität zwar unterstützt, aber dennoch wird die Verwendung der ARM-basierten Cmdlets empfohlen.


Führen Sie das Cmdlet [**Add-AzureRmAccount**](https://msdn.microsoft.com/library/mt619267.aspx) aus. Daraufhin wird eine Anmeldeseite angezeigt, auf der Sie Ihre Anmeldeinformationen eingeben müssen. Verwenden Sie die gleichen Anmeldeinformationen wie für die Anmeldung beim Azure-Portal.

	Add-AzureRmAccount

Wenn Sie über mehrere Abonnements verfügen, wählen Sie mit dem Cmdlet [**Set-AzureRmContext**](https://msdn.microsoft.com/library/mt619263.aspx) das Abonnement aus, das für die PowerShell-Sitzung verwendet werden soll. Führen Sie das Cmdlet [**Get-AzureRmContext**](https://msdn.microsoft.com/library/mt619265.aspx) aus, um das Abonnement anzuzeigen, das für die aktuelle PowerShell-Sitzung verwendet wird. Um alle Ihre Abonnements anzuzeigen, führen [**Get-AzureRmSubscription**](https://msdn.microsoft.com/library/mt619284.aspx) aus.

	Set-AzureRmContext -SubscriptionId '4cac86b0-1e56-bbbb-aaaa-000000000000'


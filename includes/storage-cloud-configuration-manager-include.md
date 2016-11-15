Die [Microsoft Azure Configuration Manager-Bibliothek für .NET](https://www.nuget.org/packages/Microsoft.WindowsAzure.ConfigurationManager/) stellt eine Klasse für die Analyse einer Verbindungszeichenfolge aus einer Konfigurationsdatei bereit. Die [CloudConfigurationManager-Klasse](https://msdn.microsoft.com/library/azure/mt634650.aspx) analysiert Konfigurationseinstellungen unabhängig davon, ob die Clientanwendung auf dem Desktop, auf einem mobilen Gerät auf einem virtuellen Azure-Computer oder in einem Azure-Clouddienst ausgeführt wird.

Fügen Sie zum Verweisen auf das CloudConfigurationManager-Paket die folgende `using` -Anweisung zu Ihrer Klasse hinzu:

    using Microsoft.Azure;    //Namespace for CloudConfigurationManager

Im folgenden Beispiel wird gezeigt, wie Sie eine Verbindungszeichenfolge aus einer Konfigurationsdatei abrufen:

    // Parse the connection string and return a reference to the storage account.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(
        CloudConfigurationManager.GetSetting("StorageConnectionString"));

Die Verwendung von Azure Configuration Manager ist optional. Sie können auch eine API wie die [ConfigurationManager-Klasse](https://msdn.microsoft.com/library/system.configuration.configurationmanager.aspx)von .NET Framework verwenden.



<!--HONumber=Nov16_HO2-->



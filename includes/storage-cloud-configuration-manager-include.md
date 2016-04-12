### Analysieren der Verbindungszeichenfolge

Die Microsoft Azure Configuration Manager-Bibliothek, auf die Sie oben verwiesen haben, stellt eine Klasse für die Analyse einer Verbindungszeichenfolge aus einer Konfigurationsdatei bereit. Die [CloudConfigurationManager-Klasse](https://msdn.microsoft.com/library/azure/mt634650.aspx) analysiert Konfigurationseinstellungen unabhängig davon, ob die Clientanwendung auf dem Desktop, auf einem mobilen Gerät auf einem virtuellen Azure-Computer oder in einem Azure-Clouddienst ausgeführt wird.

Fügen Sie den folgenden Code in die Methode `Main()` ein:

    // Parse the connection string and return a reference to the storage account.
    CloudStorageAccount storageAccount = CloudStorageAccount.Parse(CloudConfigurationManager.GetSetting("StorageConnectionString"));

Die Verwendung von Azure Configuration Manager ist optional. Sie können auch eine API wie die [ConfigurationManager-Klasse](https://msdn.microsoft.com/library/system.configuration.configurationmanager.aspx) von .NET Framework verwenden.

<!---HONumber=AcomDC_0406_2016-->
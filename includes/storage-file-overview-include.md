## <a name="overview"></a>Übersicht
Azure File Storage ist ein Dienst, bei dem Dateifreigaben in der Cloud unter Verwendung des standardmäßigen Server Message Block-Protokolls bereitgestellt werden können (siehe [Microsoft SMB Protocol and CIFS Protocol Overview](https://msdn.microsoft.com/library/windows/desktop/aa365233.aspx)(Microsoft SMB-Protokoll und CIFS-Protokoll – Übersicht)). Sowohl SMB 2.1 als auch SMB 3.0 werden unterstützt. Mit Azure File Storage können Sie Legacyanwendungen, für die Dateifreigaben benötigt werden, schnell und ohne teures Umschreiben zu Azure migrieren. Anwendungen, die auf virtuellen Azure-Maschinen, in Clouddiensten oder auf lokalen Clients ausgeführt werden, können eine Dateifreigabe genauso in der Cloud bereitstellen, wie eine Desktopanwendung eine normale SMB-Freigabe bereitstellt. Die File Storage-Freigaben können dann von beliebig vielen Anwendungskomponenten gleichzeitig eingebunden und genutzt werden.

Da es sich bei File Storage-Freigaben um gewöhnliche SMB-Freigaben handelt, können Anwendungen in Azure über die E/A-APIs des Systems auf die Freigaben zugreifen. Entwickler können daher bei der Migration vorhandener Anwendungen auf ihren vorhandenen Code sowie auf bereits angeeignetes Know-how zurückgreifen. IT-Fachkräfte können PowerShell-Cmdlets verwenden, um Dateispeicher-Freigaben im Rahmen der Administration von Azure-Anwendungen zu erstellen, einzubinden und zu verwalten.

Sie können Azure-Dateifreigaben mit dem [Azure-Portal](https://portal.azure.com), den Azure Storage-PowerShell-Cmdlets, den Azure Storage-Clientbibliotheken oder der Azure Storage-REST-API erstellen. Da es sich bei diesen Dateifreigaben um SMB-Freigaben handelt, können Sie darauf außerdem über standardmäßige und vertraute Dateisystem-APIs zugreifen.



<!--HONumber=Jan17_HO3-->



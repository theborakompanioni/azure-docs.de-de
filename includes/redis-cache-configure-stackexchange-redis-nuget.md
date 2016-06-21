.NET-Anwendungen können den **StackExchange.Redis**-Cacheclient verwenden. Dieser kann mithilfe eines NuGet-Pakets in Visual Studio konfiguriert werden, um die Konfiguration von Cacheclientanwendungen zu vereinfachen.

>[AZURE.NOTE] Weitere Informationen finden Sie auf der [StackExchange.Redis](http://github.com/StackExchange/StackExchange.Redis)-Seite auf GitHub und in der [Dokumentation für den StackExchange.Redis-Cacheclient](http://github.com/StackExchange/StackExchange.Redis#documentation).

Klicken Sie im **Projektmappen-Explorer** mit der rechten Maustaste auf das Projekt und wählen Sie **NuGet-Pakete verwalten** aus, um eine Clientanwendung in Visual Studio mithilfe des StackExchange.Redis-NuGet-Pakets zu konfigurieren.

![NuGet-Pakete verwalten](media/redis-cache-configure-stackexchange-redis-nuget/redis-cache-manage-nuget-menu.png)

Geben Sie **StackExchange.Redis** oder **StackExchange.Redis.StrongName** in das Suchfeld ein, wählen Sie die gewünschte Version aus den Ergebnissen aus, und klicken Sie auf **Installieren**.

>[AZURE.NOTE] Falls Sie eine Version der **StackExchange.Redis**-Clientbibliothek mit starkem Namen bevorzugen, wählen Sie **StackExchange.Redis.StrongName** aus; andernfalls wählen Sie **StackExchange.Redis** aus.

![StackExchange.Redis-NuGet-Paket](media/redis-cache-configure-stackexchange-redis-nuget/redis-cache-stackexchange-redis.png)

Das NuGet-Paket wird heruntergeladen und fügt die benötigten Assemblyverweise zu Ihrer Clientanwendung hinzu, um mithilfe des StackExchange.Redis-Cacheclients auf den Azure Redis Cache zuzugreifen.

<!---HONumber=AcomDC_0615_2016-->
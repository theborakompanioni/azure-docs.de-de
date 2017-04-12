1. Melden Sie sich beim [Azure-Portal]an.
2. Klicken Sie auf **+NEU** > **Web und mobil** > **Mobile App**, und geben Sie einen Namen für Ihr Mobile App-Back-End an.
3. Wählen Sie für die **Ressourcengruppe**eine vorhandene Ressourcengruppe aus, oder erstellen Sie eine neue (verwenden Sie dabei den gleichen Namen wie für die App). 
   
    Sie können einen anderen vorhandenen App Service-Plan auswählen oder einen neuen Plan erstellen. Weitere Informationen zu App Services-Plänen sowie zum Erstellen eines neuen Plans in einem anderen Tarif und am gewünschten Standort finden Sie unter [Azure App Service-Pläne – Detaillierte Übersicht](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md).
4. Für den **App Service-Plan**ist der Standardplan (im [Standard-Tarif](https://azure.microsoft.com/pricing/details/app-service/)) ausgewählt. Sie können auch einen anderen Plan auswählen oder [einen neuen Plan erstellen](../articles/app-service/azure-web-sites-web-hosting-plans-in-depth-overview.md#create-an-app-service-plan). Die Einstellungen für den App Service-Plan bestimmen [den Standort, die Funktionen, die Kosten und die Computeressourcen](https://azure.microsoft.com/pricing/details/app-service/) Ihrer App. 
   
    Nachdem Sie den Plan festgelegt haben, klicken Sie auf **Erstellen**. Dadurch wird das Back-End für mobile Apps erstellt. 
5. Klicken Sie auf dem Blatt **Einstellungen** für das neue Back-End für mobile Apps auf **Schnellstart** > Ihre Clientanwendungsplattform > **Verbindung mit Datenbank herstellen**. 
   
    ![](./media/app-service-mobile-dotnet-backend-create-new-service/dotnet-backend-create-data-connection.png)
6. Klicken Sie auf dem Blatt **Datenverbindung hinzufügen** auf **SQL-Datenbank** > **Neue Datenbank erstellen**, geben Sie den **Namen** für die Datenbank ein, wählen Sie einen Tarif aus, und klicken Sie anschließend auf **Server**.  Sie können diese neue Datenbank wiederverwenden. Wenn Sie bereits eine Datenbank am gleichen Standort haben, können Sie stattdessen **eine vorhandene Datenbank verwenden**. Die Verwendung einer Datenbank an einem anderen Standort wird aufgrund der Kosten für die Bandbreite und einer höheren Latenz nicht empfohlen.
   
    ![](./media/app-service-mobile-dotnet-backend-create-new-service/dotnet-backend-create-db.png)
7. Geben Sie auf dem Blatt **Neuer Server** im Feld **Servername** einen eindeutigen Servernamen ein, geben Sie einen Anmeldenamen und ein Kennwort an, aktivieren Sie das Kontrollkästchen **Azure-Diensten Zugriff auf den Server erlauben**, und klicken Sie anschließend auf **OK**. Dadurch wird die neue Datenbank erstellt.
8. Klicken Sie auf dem Blatt **Datenverbindung hinzufügen** auf **Verbindungszeichenfolge**, geben Sie den Anmeldenamen und das Kennwort für Ihre Datenbank ein, und klicken Sie anschließend auf **OK**. Warten Sie ein paar Minuten, bis die Datenbank bereitgestellt wurde, bevor Sie fortfahren.

<!-- URLs. -->
[Azure-Portal]: https://portal.azure.com/

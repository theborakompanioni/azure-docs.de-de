1.	Melden Sie sich auf dem [Azure-Portal](https://portal.azure.com/) an.
2.	Klicken Sie auf der Navigationsleiste auf **Neu**, klicken Sie auf **Daten und Speicher**, und klicken Sie anschließend auf **DocumentDB (NoSQL)**.

	![Screenshot des Azure-Portals mit Hervorhebung von „Weitere Dienste“ und „DocumentDB (NoSQL)“](./media/documentdb-create-dbaccount/create-nosql-db-databases-json-tutorial-1.png)

3. Geben Sie auf dem Blatt **Neues Konto** die gewünschte Konfiguration für das DocumentDB-Konto an.

	![Screenshot des Blatts "Neue DocumentDB"](./media/documentdb-create-dbaccount/create-nosql-db-databases-json-tutorial-2.png)


	- Geben Sie im Feld **ID** einen Namen zur Identifizierung des DocumentDB-Kontos ein. Wenn die **ID** überprüft wurde, wird im Feld **ID** ein grünes Häkchen angezeigt. Der Wert für **ID** wird zum Hostnamen innerhalb des URI. Die **ID** darf nur Kleinbuchstaben, Ziffern und den Bindestrich "-" enthalten und muss zwischen 3 und 50 Zeichen lang sein. Beachten Sie, dass *documents.azure.com* an den gewählten Namen des Endpunkts angehängt wird, um den Endpunkt des DocumentDB-Kontos zu erstellen.

	- Wählen Sie unter **Abonnement** das Azure-Abonnement aus, das Sie mit dem DocumentDB-Konto verwenden möchten. Wenn Ihr Konto nur über ein Abonnement verfügt, wird standardmäßig dieses Konto ausgewählt.

	- Unter **Ressourcengruppe** können Sie eine Ressourcengruppe für Ihr DocumentDB-Konto auswählen oder erstellen. Standardmäßig wird eine neue Ressourcengruppe erstellt. Weitere Informationen finden Sie unter [Verwenden des Azure-Portals zum Verwalten Ihrer Azure-Ressourcen](../articles/azure-portal/resource-group-portal.md).

	- Unter **Standort** können Sie einen geografischen Standort angeben, an dem Ihr DocumentDB-Konto gehostet wird.
	
    - Aktivieren Sie das Kontrollkästchen **An das Dashboard anheften**, um komfortabel auf Ihr Konto und die später erstellten Ressourcen zugreifen zu können.

4.	Klicken Sie nach der Konfiguration der Optionen für das neue DocumentDB-Konto auf **Erstellen**. Der Status der Bereitstellung kann im Startmenü überwacht werden. ![Screenshot der Kachel "Erstellen" im Startmenü – Online-Datenbankersteller](./media/documentdb-create-dbaccount/create-nosql-db-databases-json-tutorial-3.png)

	Sie können den Status auch über den Hub "Benachrichtigungen" überwachen.

	![Datenbanken schnell erstellen - Screenshot des Benachrichtigungs-Hubs, der zeigt, dass das DocumentDB-Konto erstellt wird](./media/documentdb-create-dbaccount/create-nosql-db-databases-json-tutorial-4.png)

	![Screenshot des Benachrichtigungs-Hubs, der zeigt, dass das DocumentDB-Konto erfolgreich erstellt und in einer Ressourcengruppe bereitgestellt wurde – Benachrichtigung für Online-Datenbankersteller](./media/documentdb-create-dbaccount/create-nosql-db-databases-json-tutorial-5.png)

5.	Nach dem Erstellen steht das DocumentDB-Konto mit den Standardeinstellungen zur Verfügung. Beachten Sie, dass die Standardkonsistenz des DocumentDB-Kontos auf **Sitzung** eingestellt ist. Sie können die Standardkonsistenz anpassen, indem Sie im Ressourcenmenü auf **Standardkonsistenz** klicken. Weitere Informationen zu den Konsistenzebenen von DocumentDB finden Sie unter [Konsistenzebenen in DocumentDB](../articles/azure-portal/resource-group-portal.md).

    ![Screenshot des Blatts "Ressourcengruppe" – Anwendungsentwicklung beginnen](./media/documentdb-create-dbaccount/create-nosql-db-databases-json-tutorial-6.png)

    ![Screenshot des Blatts „Konsistenzebene“ – Sitzungskonsistenz](./media/documentdb-create-dbaccount/create-nosql-db-databases-json-tutorial-7.png)

[How to: Create a DocumentDB account]: #Howto
[Next steps]: #NextSteps
[documentdb-manage]: ../articles/documentdb/documentdb-manage.md

<!---HONumber=AcomDC_0831_2016-->
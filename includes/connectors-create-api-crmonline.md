#### Voraussetzungen
- Ein Azure-Konto. Sie können ein [kostenloses Konto](https://azure.microsoft.com/free) erstellen.
- Ein [Dynamics CRM Online](https://www.microsoft.com/de-DE/dynamics/crm-free-trial-overview.aspx)-Konto

Bevor Sie Ihr Dynamics-Konto in einer Logik-App verwenden, autorisieren Sie die Logik-App für die Verbindungsherstellung mit Ihrem CRM Online-Konto. Sie können dies ganz einfach in Ihrer Logik-App im Azure-Portal durchführen.

Führen Sie folgende Schritte aus, um Ihre Logik-App für die Verbindungsherstellung mit Ihrem CRM Online-Konto zu autorisieren:

1. Erstellen Sie eine Logik-App. Wählen Sie im Logic Apps-Designer in der Dropdownliste **Von Microsoft verwaltete APIs anzeigen** aus, und geben Sie dann „Dynamics“ in das Suchfeld ein. Wählen Sie einen Trigger oder eine Aktion aus: ![](./media/connectors-create-api-crmonline/dynamics-triggers.png)
2. Wenn Sie bisher noch keine Verbindung mit Dynamics hergestellt haben, werden Sie aufgefordert, sich mit Ihren Dynamics-Anmeldeinformationen anzumelden: ![](./media/connectors-create-api-crmonline/dynamics-signin.png)
3. Wählen Sie **Anmelden** aus, und geben Sie Ihren Benutzernamen und Ihr Kennwort ein. Wählen Sie **Anmelden**.

	Mithilfe dieser Anmeldeinformationen wird Ihre Logik-App dazu autorisiert, eine Verbindung mit Ihrem Dynamics-Konto herzustellen und auf die darin gespeicherten Daten zuzugreifen.
4. Beachten Sie, dass die Verbindung erstellt wurde. Fahren Sie nun mit den weiteren Schritten in Ihrer Logik-App fort: ![](./media/connectors-create-api-crmonline/dynamics-properties.png)

<!---HONumber=AcomDC_0817_2016-->
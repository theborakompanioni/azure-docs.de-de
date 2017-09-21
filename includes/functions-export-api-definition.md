## <a name="export-an-api-definition"></a>Exportieren einer API-Definition
Sie verfügen über eine OpenAPI-Definition für Ihre Funktion (aus [Erstellen einer OpenAPI-Definition für eine Funktion](../articles/azure-functions/functions-openapi-definition.md)). Im nächsten Schritt wird die API-Definition exportiert, damit sie von PowerApps und Microsoft Flow in einer benutzerdefinierten API verwendet werden kann.

> [!IMPORTANT]
> Hierzu müssen Sie bei Azure mit den gleichen Anmeldeinformationen angemeldet sein, die Sie auch für Ihre PowerApps- und Microsoft Flow-Mandanten verwenden. Dadurch kann Azure die benutzerdefinierte API erstellen und für PowerApps und Microsoft Flow verfügbar machen.

1. Klicken Sie im [Azure-Portal](https://portal.azure.com) auf den Namen Ihrer Funktionen-App (z.B. **function-demo-energy**) und dann auf **Plattformfeatures** > **API-Definition**.

    ![API-Definition](media/functions-export-api-definition/api-definition.png)

1. Klicken Sie auf **Nach PowerApps + Microsoft Flow exportieren**.

    ![API-Definitionsquelle](media/functions-export-api-definition/export-api-1.png)

1. Verwenden Sie im rechten Bereich die in der Tabelle angegebenen Einstellungen.

    |Einstellung|Beschreibung|
    |--------|------------|
    |**Exportmodus**|Wählen Sie **Express** aus, um die benutzerdefinierte API automatisch zu generieren. Bei Verwendung von **Manuell** wird die API-Definition zwar exportiert, muss dann aber manuell in PowerApps und Microsoft Flow importiert werden. Weitere Informationen finden Sie unter [Export in PowerApps und Microsoft Flow](https://docs.microsoft.com/azure/app-service/app-service-export-api-to-powerapps-and-flow?toc=%2fazure%2fazure-functions%2ftoc.json).|
    |**Umgebung**|Wählen Sie die Umgebung aus, in der die benutzerdefinierte API gespeichert werden soll. Weitere Informationen finden Sie unter [Umgebungen – Übersicht (PowerApps)](https://powerapps.microsoft.com/tutorials/environments-overview/) oder [Umgebungen – Übersicht (Microsoft Flow)](https://us.flow.microsoft.com/documentation/environments-overview-admin/).|
    |**Name der benutzerdefinierten API**|Geben Sie einen Namen ein (beispielsweise `Turbine Repair`).|
    |**API-Schlüsselname**|Geben Sie den Namen ein, der App- und Flow-Erstellern auf der Benutzeroberfläche der benutzerdefinierten API angezeigt werden soll. Wie Sie sehen, enthält das Beispiel hilfreiche Informationen.|
 
    ![Export in PowerApps und Microsoft Flow](media/functions-export-api-definition/export-api-2.png)

1. Klicken Sie auf **OK**. Die benutzerdefinierte API wird erstellt und der angegebenen Umgebung hinzugefügt.
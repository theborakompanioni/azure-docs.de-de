<properties
   pageTitle="Verwenden von Azure Functions zum Ausführen eines geplanten Bereinigungstasks| Microsoft Azure"
   description="Verwenden Sie Azure Functions, um eine C#-Funktion zu erstellen, die basierend auf einem Ereignistimer ausgeführt wird."
   services="functions"
   documentationCenter="na"
   authors="ggailey777"
   manager="erikre"
   editor=""
   tags=""
   />

<tags
   ms.service="functions"
   ms.devlang="multiple"
   ms.topic="article"
   ms.tgt_pltfrm="multiple"
   ms.workload="na"
   ms.date="09/26/2016"
   ms.author="glenga"/>
   
# Verwenden von Azure Functions zum Ausführen eines geplanten Bereinigungstasks

In diesem Thema erfahren Sie, wie Sie mit Azure Functions eine neue Funktion in C# erstellen, die basierend auf einem Ereignistimer ausgeführt wird, um Zeilen in einer Datenbanktabelle zu bereinigen. Die neue Funktion wird basierend auf einer vordefinierten Vorlage im Azure Functions-Portal erstellt. Zur Unterstützung dieses Szenarios müssen Sie auch eine Datenbank-Verbindungszeichenfolge als App Service-Einstellung in der Funktionen-App festlegen.

## Voraussetzungen 

Bevor Sie eine Funktion erstellen können, müssen Sie über ein aktives Azure-Konto verfügen. Wenn Sie noch kein Azure-Konto haben, [erstellen Sie ein kostenloses Konto](https://azure.microsoft.com/free/).

In diesem Thema wird ein Transact-SQL-Befehl veranschaulicht, der einen Massenbereinigungsvorgang in der Tabelle *TodoItems* in einer SQL-Datenbank ausführt. Diese Tabelle „TodoItems“ wird auch erstellt, wenn Sie das Tutorial [Azure App Service Mobile Apps: Schnellstart](../app-service-mobile/app-service-mobile-ios-get-started.md) durchlaufen. Sie können auch eine Beispieldatenbank verwenden. Wenn Sie eine andere Tabelle verwenden, müssen Sie den Befehl ändern.

Sie können die Verbindungszeichenfolge, die von einem Mobile App-Back-End verwendet wird, im Portal unter **Alle Einstellungen** > **Anwendungseinstellungen** > **Verbindungszeichenfolgen** > **Werte der Verbindungszeichenfolge anzeigen** > **MS\_TableConnectionString** abrufen. Sie können die Verbindungszeichenfolge auch direkt aus einer SQL-Datenbank im Portal unter **Alle Einstellungen** > **Eigenschaften** > **Datenbank-Verbindungszeichenfolgen anzeigen** > **ADO.NET (SQL-Authentifizierung)** abrufen.

In diesem Szenario wird ein Massenvorgang auf die Datenbank angewendet. Damit Ihre Funktion einzelne CRUD-Vorgänge in einer Mobile Apps-Tabelle verarbeiten kann, sollten Sie stattdessen die Mobile Table-Bindung verwenden.

## Festlegen einer SQL-Datenbank-Verbindungszeichenfolge in der Funktionen-App

Eine Funktions-App hostet die Ausführung Ihrer Funktionen in Azure. Es ist eine bewährte Methode, Verbindungszeichenfolgen und andere geheime Schlüssel in den Einstellungen Ihrer Funktionen-App zu speichern. Dies verhindert eine versehentliche Offenlegung, wenn Ihr Funktionscode irgendwo in einem Repository abgelegt wird.

1. Wechseln Sie zum [Azure Functions-Portal](https://functions.azure.com/signin), und melden Sie sich mit Ihrem Azure-Konto an.

2. Wenn Sie bereits eine Funktions-App besitzen, wählen Sie diese in **Ihre Funktions-Apps** aus, und klicken Sie dann auf **Öffnen**. Um eine neue Funktionen-App zu erstellen, geben Sie einen eindeutigen **Namen** für Ihre neue Funktionen-App ein, oder übernehmen Sie den generierten Namen, wählen Sie die bevorzugte **Region** aus, und klicken Sie anschließend auf **Erstellen und starten**.

3. Klicken Sie in Ihrer Funktionen-App auf **Funktionen-App-Einstellungen** > **Zu App Service-Einstellungen wechseln**.

	![Funktionen-App, Blatt „Einstellungen“](./media/functions-create-an-event-processing-function/functions-app-service-settings.png)

4. Klicken Sie in Ihrer Funktionen-App auf **Alle Einstellungen**, und scrollen Sie nach unten zu **Anwendungseinstellungen**. Geben Sie dann unter **Verbindungseinstellungen** für **Name** `sqldb_connection` ein. Fügen Sie die Verbindungszeichenfolge in **Wert** ein. Klicken Sie auf **Speichern**, und schließen Sie dann das Blatt „Funktionen-App“, um zum Azure Functions-Portal zurückzukehren.

    ![App Service-Einstellung „Verbindungszeichenfolge“](./media/functions-create-an-event-processing-function/functions-app-service-settings-connection-strings.png)

Nun können Sie den C#-Funktionscode hinzufügen, der eine Verbindung mit Ihrer SQL-Datenbank herstellt.

## Erstellen einer per Timer ausgelösten Funktion aus der Vorlage

1. Klicken Sie in Ihrer Funktionen-App auf **+ Neue Funktion** > **TimerTrigger – C#** > **Erstellen**. Eine Funktion mit einem Standardnamen wird erstellt, die nach dem Standardzeitplan einmal pro Minute ausgeführt wird.

	![Neue per Timer ausgelöste Funktion erstellen](./media/functions-create-an-event-processing-function/functions-create-new-timer-trigger.png)

2. Fügen Sie im Bereich **Code** auf der Registerkarte **Entwickeln** die folgenden Assemblyverweise am Anfang des vorhandenen Funktionscodes hinzu:

		#r "System.Configuration"
		#r "System.Data"

3. Fügen Sie der Funktion die folgenden `using`-Anweisungen hinzu:

		using System.Configuration;
		using System.Data.SqlClient;
		using System.Threading.Tasks; 

4. Ersetzen Sie die vorhandene **Run**-Funktion durch den folgenden Code:

		public static async Task Run(TimerInfo myTimer, TraceWriter log)
		{
		    var str = ConfigurationManager.ConnectionStrings["sqldb_connection"].ConnectionString;
		    using (SqlConnection conn = new SqlConnection(str))
		    {
		        conn.Open();
		        var text = "DELETE from dbo.TodoItems WHERE Complete='True'";
		        using (SqlCommand cmd = new SqlCommand(text, conn))
		        {
					// Execute the command and log the # rows deleted.
		            var rows = await cmd.ExecuteNonQueryAsync();
		            log.Info($"{rows} rows were deleted");
		        }
		    }
		}

5. Klicken Sie auf **Speichern**, überwachen Sie das Fenster **Protokolle** auf die nächste Funktionsausführung, und beachten Sie die Anzahl der Zeilen, die aus der Tabelle „TodoItems“ gelöscht wurden.

6. (Optional) Markieren Sie mithilfe der [Mobile Apps-Schnellstart-App](../app-service-mobile/app-service-mobile-ios-get-started.md) zusätzliche Elemente als „Abgeschlossen“. Kehren Sie dann zum Fenster **Protokolle** zurück, und beobachten Sie, wie dieselbe Anzahl von Zeilen von der Funktion während der nächsten Ausführung gelöscht wird.

##Nächste Schritte

Weitere Informationen zu Azure Functions finden Sie in diesen Themen.

+ [Entwicklerreferenz zu Azure Functions](functions-reference.md) Referenz zum Programmieren von Funktionen sowie zum Festlegen von Triggern und Bindungen.
+ [Testing Azure Functions (Testen von Azure Functions)](functions-test-a-function.md) Beschreibt verschiedene Tools und Techniken zum Testen Ihrer Funktionen
+ [How to scale Azure Functions (Skalieren von Azure Functions)](functions-scale.md) Beschreibt die für Azure Functions verfügbaren Servicepläne einschließlich des dynamischen Serviceplans, und wie Sie den richtigen Plan finden

[AZURE.INCLUDE [Hinweis zu den ersten Schritten](../../includes/functions-get-help.md)]

<!---HONumber=AcomDC_0928_2016-->
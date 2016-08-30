## Erstellen eines Ereignis-Hubs

1. Melden Sie sich beim [klassischen Azure-Portal][] an, und klicken Sie im unteren Teil des Bildschirms auf **NEU**.

2. Klicken Sie auf **App Services**, **Service Bus**,** Event Hub** und auf **Schnellerfassung**.

	![][1]

3. Geben Sie einen Namen für den Event Hub ein. Wählen Sie die gewünschte Region aus, und klicken Sie dann auf **Neuen Event Hub erstellen**.

	![][2]

4. Wenn Sie nicht explizit einen vorhandenen Namespace in einer bestimmten Region ausgewählt haben, wird im Portal ein Namespace für Sie erstellt (normalerweise ***Event Hub-Name*-ns**). Klicken Sie auf diesen Namespace (in diesem Beispiel **eventhub-ns**).

	![][3]

5. Klicken Sie unten auf der Seite auf **Verbindungsinformationen**. Klicken Sie auf die Kopierschaltfläche (siehe folgende Abbildung), um die Verbindungszeichenfolge **RootManageSharedAccessKey** in die Zwischenablage zu kopieren. Speichern Sie diese Verbindungszeichenfolge für die spätere Verwendung in diesem Tutorial.

	![][4]

Ihr Event Hub wird jetzt erstellt, und Sie verfügen über die zum Senden und Empfangen von Ereignissen erforderlichen Verbindungszeichenfolgen.

[1]: ./media/event-hubs-create-event-hub/create-event-hub1.png
[2]: ./media/event-hubs-create-event-hub/create-event-hub2.png
[3]: ./media/event-hubs-create-event-hub/create-event-hub3.png
[4]: ./media/event-hubs-create-event-hub/create-conn-str1.png

[klassischen Azure-Portal]: https://manage.windowsazure.com/

<!---HONumber=AcomDC_0824_2016-->
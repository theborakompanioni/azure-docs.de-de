## Empfangen von Nachrichten auf dem simulierten Gerät

In diesem Abschnitt ändern Sie die simulierte Geräteanwendung, die Sie in [Erste Schritte mit IoT Hub] erstellt haben, um Cloud-zu-Gerät-Nachrichten von IoT Hub zu empfangen.

1. Fügen Sie in Visual Studio im **SimulatedDevice**-Projekt der **Program**-Klasse folgende Methode hinzu.

        private static async void ReceiveC2dAsync()
        {
            Console.WriteLine("\nReceiving cloud to device messages from service");
            while (true)
            {
                Message receivedMessage = await deviceClient.ReceiveAsync();
                if (receivedMessage == null) continue;

                Console.ForegroundColor = ConsoleColor.Yellow;
                Console.WriteLine("Received message: {0}", Encoding.ASCII.GetString(receivedMessage.GetBytes()));
                Console.ResetColor();

                await deviceClient.CompleteAsync(receivedMessage);
            }
        }

    Die `ReceiveAsync`-Methode gibt die empfangene Nachricht asynchron zurück, sobald sie vom Gerät empfangen wird. Nach Ablauf eines festlegbaren Timeoutzeitraums (hier wird der Standardwert eine Minute verwendet) gibt sie *NULL* zurück. Falls dies geschieht, soll der Code weiter auf neue Nachrichten warten, was durch die Zeile `if (receivedMessage == null) continue` gewährleistet ist.

    Der Aufruf von `CompleteAsync()` teilt IoT Hub mit, dass die Nachricht erfolgreich verarbeitet wurde. Die Nachricht kann sicher aus der Gerätewarteschlange entfernt werden. Wenn ein Fehler aufgetreten ist, der den Abschluss der Verarbeitung der Nachricht durch die Geräte-App verhindert hat, übermittelt IoT Hub sie erneut. Dann ist es wichtig, dass die Nachrichtenverarbeitungslogik in der Geräte-App *idempotent* ist, sodass der mehrmalige Empfang der gleichen Nachricht das gleiche Ergebnis erzeugt. Eine Anwendung kann eine Nachricht auch vorübergehend verwerfen, sodass IoT Hub die Nachricht für die zukünftige Verwendung in der Warteschlange beibehält. Oder die Anwendung kann eine Nachricht ablehnen, wodurch die Nachricht dauerhaft aus der Warteschlange entfernt wird. Weitere Informationen zum Lebenszyklus von C2D-Nachrichten finden Sie im [Entwicklungsleitfaden für IoT Hub][IoT Hub Developer Guide - C2D].

> [AZURE.NOTE] Wird HTTP/1 anstelle von AMQP als Transport verwendet wird, erfolgt von der `ReceiveAsync`-Methode sofort eine Rückgabe. Das unterstützte Muster für C2D-Nachrichten mit HTTP/1 entspricht Geräten, die nicht permanent verbunden sind und weniger häufig Nachrichten abrufen (d.h. seltener als alle 25 Minuten). Die Ausgabe von mehr HTTP/1-Empfangsvorgängen führt dazu, dass die Anforderungen von IoT Hub gedrosselt werden. Weitere Informationen zu den Unterschieden zwischen der AMQP- und HTTP/1-Unterstützung sowie zur IoT Hub-Drosselung finden Sie im [Entwicklungsleitfaden für IoT Hub][IoT Hub Developer Guide - C2D].

2. Fügen Sie in der **Main**-Methode unmittelbar vor der Zeile `Console.ReadLine()` folgende Methode hinzu:

        ReceiveC2dAsync();

> [AZURE.NOTE] Der Einfachheit halber wird in diesem Lernprogramm keine Wiederholungsrichtlinie implementiert. Im Produktionscode sollten Sie Wiederholungsrichtlinien implementieren (z.B. einen exponentiellen Backoff), wie im MSDN-Artikel zum Behandeln [Vorübergehender Fehler beschrieben](https://msdn.microsoft.com/library/hh680901(v=pandp.50).aspx).

<!-- Links -->
[IoT Hub Developer Guide - C2D]: ../articles/iot-hub/iot-hub-devguide.md#c2d

<!-- Images -->

<!---HONumber=AcomDC_0608_2016-->
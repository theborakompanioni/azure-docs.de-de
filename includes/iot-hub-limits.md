In der folgenden Tabelle sind die Grenzwerte aufgeführt, die den verschiedenen Dienstebenen (S1, S2, S3, F1) zugeordnet sind. Informationen zu den Kosten jeder *Einheit* finden Sie unter [IoT Hub – Preise](https://azure.microsoft.com/pricing/details/iot-hub/).

| Ressource | S1 Standard | S2 Standard | S3 Standard | F1 Free |
| -------- | ----------- | ----------- | ----------- | ------- |
| Nachrichten/Tag | 400\.000 | 6\.000.000 | 300\.000.000 | 8\.000 |
| Maximale Anzahl der Einheiten | 200 | 200 | 200 | 1 |

> [AZURE.NOTE] Wenden Sie sich an den Microsoft-Support, wenn Sie voraussichtlich mehr als 200 Einheiten mit einem Hub im Tarif S1, S2 oder S3 verwenden.

Die folgende Tabelle enthält die für IoT Hub-Ressourcen geltenden Grenzwerte:

| Ressource | Begrenzung |
| -------- | ----- |
| Maximale Anzahl kostenpflichtiger IoT Hubs pro Azure-Abonnement | 10 |
| Maximale Anzahl kostenloser IoT Hubs pro Azure-Abonnement | 1 |
| Maximale Anzahl von Geräte-Identitäten,<br/> die bei einem einzelnen Aufruf zurückgegeben wird | 1000 |
| Maximale Beibehaltungsdauer von IoT Hub-Nachrichten für D2C-Nachrichten | 7 Tage |
| Maximale Größe einer Nachricht von einem Gerät an die Cloud | 256 KB |
| Maximale Größe eines Batches, das vom Gerät an die Cloud gesendet wird | 256 KB |
| Maximale Anzahl von Nachrichten im Batch, das vom Gerät an die Cloud gesendet wird | 500 |
| Maximale Größe einer Nachricht von der Cloud an das Gerät | 64 KB |
| Maximale Gültigkeitsdauer von Nachrichten von der Cloud an das Gerät | 2 Tage |
| Maximale Übermittlungsanzahl von Nachrichten von der<br/> Cloud an das Gerät | 100 |
| Maximale Übermittlungsanzahl von Feedbacknachrichten<br/> als Reaktion auf eine Nachricht von der Cloud an das Gerät | 100 |
| Maximale Gültigkeitsdauer von Feedbacknachrichten<br/> als Reaktion auf eine Nachricht von der Cloud an das Gerät | 2 Tage |

> [AZURE.NOTE] Wenn Sie mehr als zehn kostenpflichtige IoT Hubs in einem Azure-Abonnement benötigen, wenden Sie sich an den Microsoft Support.

Der IoT Hub-Dienst drosselt Anforderungen, wenn die folgenden Kontingente überschritten werden:

| Drosselung | Wert pro Hub |
| -------- | ------------- |
| Vorgänge in der Identitätsregistrierung <br/> (Erstellen, Abrufen, Aktualisieren, Löschen), <br/> einzelne Import-/Exportvorgänge oder Massenimport/-export | 5000/Minute/Einheit (für S3) <br/> 100/Minute/Einheit (für S1 und S2) |
| Geräteverbindungen | 6\.000/Sekunde/Einheit (für S3), 120/Sekunde/Einheit (für S2), 12/Sekunde/Einheit (für S1). <br/>Mindestens 100/Sekunde |
| Senden von Nachrichten von Geräten an die Cloud | 6\.000/Sekunde/Einheit (für S3), 120/Sekunde/Einheit (für S2), 12/Sekunde/Einheit (für S1). <br/>Mindestens 100/Sekunde |
| C2D-Sendevorgänge | 5\.000/Minute/Einheit (für S3), 100/Minute/Einheit (für S1 und S2). |
| C2D-Empfangsvorgänge | 50\.000/Minute/Einheit (für S3), 1.000/Minute/Einheit (für S1 und S2). |
| Dateiuploadvorgänge | 5\.000 Dateiuploadbenachrichtigungen/Minute/Einheit (S3), 100 Dateiuploadbenachrichtigungen/Minute/Einheit (für S1 und S2). <br/> 10.000 SAS-URIs können gleichzeitig für ein Speicherkonto geöffnet sein.<br/> 10 SAS-URIs/Gerät können gleichzeitig geöffnet sein. |

<!---HONumber=AcomDC_0824_2016-->
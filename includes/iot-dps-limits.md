Die folgende Tabelle enthält die Grenzwerte, die für Ressourcen des IoT Hub Device Provisioning-Diensts gelten:

| Ressource | Begrenzung |
| --- | --- |
| Maximale Device Provisioning-Dienste pro Azure-Abonnement | 10 |
| Maximale Anzahl von Registrierungen (Enrollments) | 10.000 |
| Maximale Anzahl von Registrierungen (Registrations) | 10.000 |
| Maximale Anzahl von Registrierungsgruppen | 100 |
| Maximale Anzahl von Zertifizierungsstellen | 10 |

> [!NOTE]
> Diese Grenzwerte gelten für die öffentliche Vorschauversion. Sobald der Dienst allgemein verfügbar ist, können Sie sich an den [Microsoft-Support](https://azure.microsoft.com/support/options/) wenden, um die Anzahl von Instanzen in Ihrem Abonnement erhöhen zu lassen.

Der Device Provisioning-Dienst drosselt Anforderungen, wenn die folgenden Kontingente überschritten werden:

| Drosselung | Wert pro Dienst |
| --- | --- |
| Vorgänge | 100 pro Minute |
| Geräteregistrierungen | 100 pro Minute |

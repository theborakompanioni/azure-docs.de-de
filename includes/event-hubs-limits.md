In der folgenden Tabelle sind für Azure Event Hubs spezifische Kontingente und Grenzwerte aufgelistet. Weitere Informationen zu Event Hubs finden Sie unter [Event Hubs – Preise](https://azure.microsoft.com/pricing/details/event-hubs/). Informationen zu Preisen und anderen Kontingenten für Service Bus finden Sie in der Übersicht der [Service Bus-Preise](https://azure.microsoft.com/pricing/details/service-bus/).

| Begrenzung | Umfang | Typ | Verhalten beim Überschreiten | Wert |
|--------------------------------------------------|-------------|--------|------------------------------------------------------------------------------------------------------------------------|----------|
| Anzahl von Event Hubs pro Namespace | Namespace | Statisch | Nachfolgende Anforderungen für die Erstellung eines neuen Namespaces werden zurückgewiesen. | 10 |
| Anzahl von Partitionen pro Event Hub | Entität | Statisch | - | 32 |
| Anzahl von Consumergruppen pro Event Hub | Entität | Statisch | - | 20 |
| Anzahl von AMQP-Verbindungen pro Namespace | Namespace | Statisch | Nachfolgende Anforderungen für zusätzliche Verbindungen werden abgelehnt, und vom aufrufenden Code wird eine Ausnahme empfangen. | 5.000 |
| Maximale Ereignisgröße | Systemweit | Statisch | - | 256 KB |
| Anzahl nicht epochenbezogner Empfänger pro Consumergruppe | Entität | Statisch | - | 5 |
| Maximale Aufbewahrungsdauer von Ereignisdaten | Entität | Statisch | - | 1-7 Tage |
| Maximale Durchsatzeinheiten | Namespace | Statisch | Bei Überschreiten des Grenzwerts für Durchsatzeinheiten werden Ihre Daten gedrosselt, und eine **ServerBusyException**-Ausnahme wird ausgelöst. Sie können im Tarif „Standard“ eine höhere Anzahl von Durchsatzeinheiten anfordern, indem Sie ein Supportticket erstellen. Zusätzliche Durchsatzeinheiten sind für einen festgelegten Kaufpreis in 20er-Blöcken verfügbar. | 20 |

<!---HONumber=AcomDC_0615_2016-->

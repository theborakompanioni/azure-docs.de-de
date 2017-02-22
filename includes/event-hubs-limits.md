In der folgenden Tabelle sind für Azure Event Hubs spezifische Kontingente und Grenzwerte aufgelistet. Informationen zu den Preisen von Event Hubs finden Sie unter [Event Hubs – Preise](https://azure.microsoft.com/pricing/details/event-hubs/).

| Begrenzung | Umfang | Typ | Verhalten beim Überschreiten | Wert |
| --- | --- | --- | --- | --- |
| Anzahl von Event Hubs pro Namespace |Namespace |Statisch |Nachfolgende Anforderungen für die Erstellung eines neuen Namespaces werden zurückgewiesen. |10 |
| Anzahl von Partitionen pro Event Hub |Entität |Statisch |- |32 |
| Anzahl von Consumergruppen pro Event Hub |Entität |Statisch |- |20 |
| Anzahl von AMQP-Verbindungen pro Namespace |Namespace |Statisch |Nachfolgende Anforderungen für zusätzliche Verbindungen werden abgelehnt, und vom aufrufenden Code wird eine Ausnahme empfangen. |5.000 |
| Maximale Größe des Event Hubs-Ereignisses|Systemweit |Statisch |- |256 KB |
| Maximale Größe eines Event Hub-Namens |Entität |Statisch |- |50 Zeichen |
| Anzahl nicht epochenbezogener Empfänger pro Consumergruppe |Entität |Statisch |- |5 |
| Maximale Aufbewahrungsdauer von Ereignisdaten |Entität |Statisch |- |1–7 Tage |
| Maximale Durchsatzeinheiten |Namespace |Statisch |Bei einer Überschreitung des Grenzwerts für Durchsatzeinheiten werden Ihre Daten gedrosselt, und eine **ServerBusyException**-Ausnahme wird ausgelöst. Sie können im Tarif „Standard“ eine höhere Anzahl von Durchsatzeinheiten anfordern, indem Sie ein Supportticket erstellen. Zusätzliche Durchsatzeinheiten sind für einen festgelegten Kaufpreis in 20er-Blöcken verfügbar. |20 |



<!--HONumber=Feb17_HO3-->



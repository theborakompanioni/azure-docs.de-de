Die Rechnungsstellung für Azure-Speicher basiert auf Ihrem Speicherkonto. Speicherkosten basieren auf den folgenden Faktoren: Region/Standort, Kontotyp, Speicherkapazität, Replikationsschema, Speichertransaktionen und Datenausgang.

* „Region“ bezieht sich auf die geografische Region, in der sich Ihr Konto befindet.
* „Kontotyp“ bezieht sich darauf, ob Sie ein allgemeines Speicherkonto oder ein BLOB-Speicherkonto verwenden. Bei einem BLOB-Speicherkonto bestimmt der Zugriffstarif darüber hinaus das Abrechnungsmodell für das Konto.
* Speicherkapazität bezieht sich darauf, wie viel von Ihrer Speicherkontozuweisung zum Speichern von Daten verwendet wird.
* Die Replikation bestimmt, wie viele Kopien Ihrer Daten jeweils an welchen Standorten unterhalten werden.
* Transaktionen beziehen sich auf alle Lese- und Schreibvorgänge im Azure-Speicher.
* Datenausgang bezieht sich auf Daten, die aus einer Azure-Region übertragen werden. Wenn eine Anwendung, die nicht in der gleichen Region ausgeführt wird, auf die Daten in Ihrem Speicherkonto zugreift, fallen Gebühren für den Datenausgang an. (Für Azure-Dienste können Sie Maßnahmen durchführen, um Daten und Dienste in den gleichen Rechenzentren zu gruppieren und so Datenausgangsgebühren zu reduzieren oder zu eliminieren.)

Die Seite [Preise für Azure Storage](https://azure.microsoft.com/pricing/details/storage/) bietet detaillierte Preisinformationen basierend auf Kontotyp, Speicherkapazität, Replikation und Transaktionen. In der [Datenübertragungs-Preisübersicht](https://azure.microsoft.com/pricing/details/data-transfers/) finden Sie detaillierte Preisinformationen für den Datenausgang. Sie können den [Azure Storage-Preisrechner](https://azure.microsoft.com/pricing/calculator/?scenario=data-management) verwenden, um Ihre Kosten zu bestimmen.


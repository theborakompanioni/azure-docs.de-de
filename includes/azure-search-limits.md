| Ressource | Kostenlos | Basic (Vorschau) <sup>6</sup> | S1 | S2 <sup>7</sup> |
| --- | --- | --- | --- | --- |
| Maximale Anzahl der Suchdienste<sup>8</sup> |1 pro Abonnement |12 pro Abonnement |12 pro Abonnement |1 pro Abonnement |
| Maximale Speichergröße <sup>1</sup> |50 MB oder 10.000 Dokumente |2 GB pro Dienst |25 GB pro Partition oder 300 GB Dokumente pro Dienst |100 GB pro Partition oder 1,2 TB pro Dienst |
| Maximale Anzahl gehosteter Dokumente |10\.000 insgesamt |1 Million pro Dienst |15 Millionen pro Partition (bis zu 180 Millionen Dokumente pro Dienst) |60 Millionen pro Partition (bis zu 720 Millionen Dokumente pro Dienst) |
| Maximale Anzahl von Indizes |3 |5 |50 |200 |
| Maximale Anzahl von Indexern |3 |5 |50 |200 |
| Maximale Anzahl von Indexerdatenquellen |3 |5 |50 |200 |
| Index: maximale Anzahl von Feldern pro Index |1000 |100 <sup>5</sup> |1000 |1000 |
| Index: maximale Anzahl von Bewertungsprofilen pro Index |16 |16 |16 |16 |
| Index: maximale Anzahl von Funktionen pro Profil |8 |8 |8 |8 |
| Indexer: maximale Indizierungslast pro Aufruf |10\.000 Dokumente |Nur durch maximale Dokumentanzahl beschränkt |Nur durch maximale Dokumentanzahl beschränkt |Nur durch maximale Dokumentanzahl beschränkt |
| Indexer: maximale Ausführungszeit |3 Minuten |24 Stunden |24 Stunden |24 Stunden |
| Blobindexer: maximale Blobgröße, MB |16 |16 |128 |256 |
| Blobindexer: maximale Anzahl der Zeichen des aus einem Blob extrahierten Inhalts |32\.000 |64\.000 |4 Millionen |4 Millionen |
| Abfragen pro Sekunde (QPS) <sup>2</sup> |– |~3 pro Replikat |~15 pro Replikat |~60 pro Replikat |
| Horizontales Hochskalieren: maximale Anzahl von Sucheinheiten (SU) <sup>3</sup> |– |Bis zu 3 Einheiten (3 Replikate und 1 Partition) |36 Einheiten |36 Einheiten |
| Preise <sup>4</sup> |– |75 USD pro SU pro Monat |250 USD pro SU pro Monat |1\.000 USD pro SU pro Monat |

<sup>1</sup> Die Speichergröße ist entweder eine feste Menge oder die Anzahl der Dokumente pro Dienst, je nachdem, was zuerst erreicht wird.

<sup>2</sup> QPS ist ein ungefährer Wert, der auf Heuristik basiert, mit simulierten und tatsächlichen Kundenworkloads zum Ableiten der geschätzten Werte. Der genaue QPS-Durchsatz variiert abhängig von Ihren Daten und der Art der Abfrage.

<sup>3</sup> Sucheinheiten sind die abrechenbare Einheit für ein Replikat oder eine Partition. Sie brauchen beides für Speicher-, Indizierungs- und Abfragevorgänge. Unter [Kapazitätsplanung](../articles/search/search-capacity-planning.md) finden Sie gültige Kombinationen von Replikaten und Partitionen, mit denen Sie im Basic- bzw. Standard-Tarif innerhalb der maximalen Grenzwerte von 3 oder 36 Einheiten bleiben.

<sup>4</sup> Der Preis gilt für den US-Markt und veranschaulicht die relativen Kosten für die einzelnen Tarife. Für andere Märkte gelten andere Preise. Preise in anderen Währungen finden Sie unter [Preisübersicht](https://azure.microsoft.com/pricing/details/search/). Der Preis gilt pro Sucheinheit (SU). Auf der S1-Ebene kostet eine Konfiguration mit drei Sucheinheiten (z. B. 3 Replikate und 1 Partition) im Durchschnitt $750 pro Monat. Wenn Sie innerhalb des Monats auf weniger SUs herunterskalieren, wird die Rechnung anteilig erstellt, sodass nur die tatsächliche Nutzung abgerechnet wird.

<sup>5</sup> Der Basic-Tarif ist der einzige Tarif mit einem unteren Grenzwert von 100 Feldern pro Index.

<sup>6</sup> Der [Basic-Tarif](http://aka.ms/azuresearchbasic) ist während der Vorschauphase zu einem Einführungspreis mit 50 Prozent Nachlass auf den vollen Preis erhältlich.

<sup>7</sup> S2 benötigt ein Dienstticket. Es kann nicht im Portal bereitgestellt werden. Wenden Sie sich an den Support, oder senden Sie eine E-Mail an azuresearch_contact@microsoft.com, um Unterstützung zu erhalten.

<sup>8</sup> Sie können die maximale Anzahl von Diensten pro Abonnement für alle Tarife erhöhen (Ausnahme: Free). Wenden Sie sich an den Support, oder senden Sie eine E-Mail an azuresearch_contact@microsoft.com, um Unterstützung zu erhalten.

<!---HONumber=AcomDC_0608_2016-->
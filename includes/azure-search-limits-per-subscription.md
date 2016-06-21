Sie können mehrere Dienste innerhalb eines Abonnements erstellen, die jeweils in einem bestimmten Tarif bereitgestellt werden. Die einzige Beschränkung ist die zulässige Anzahl von Diensten für jeden Tarif innerhalb eines einzelnen Azure-Abonnements. Angaben zur maximalen Anzahl von Diensten pro Tarif sind unten aufgeführt. Wie erwähnt, könnten Sie bis zu 12 Dienste im Basic-Tarif und weitere 12 Dienste im S1-Tarif innerhalb desselben Abonnements erstellen.

Weitere Tarife sind einmal pro Abonnement verfügbar. Sie können sich an den Azure-Support wenden, wenn Sie S2, S3 oder S3 HD mehrfach pro Abonnement benötigen.

Ressource|Kostenlos|Basic|S1|S2|S3 <sup>1</sup> <br/>(Vorschau) |S3 HD <sup>1</sup> <br/>(Vorschau) 
---|---|---|---|----|---|----
Maximale Anzahl von Diensten |1 |12 |12 |1 |1 |1 
Maximale Skalierung in SU <sup>2</sup>|N/V <sup>3</sup>|3 SU <sup>4</sup> |36 SU|36 SU|36 SU|12 SU <sup>5</sup>

<sup>1</sup> Tarife in der **Vorschauphase** werden zu einem Einführungspreis von 50 Prozent Nachlass auf den vollen Preis abgerechnet. Vor der allgemeinen Verfügbarkeit (GA) werden Tarife als Vorschaufunktion vorgestellt. Es gibt keine Vereinbarung zum Servicelevel (SLA) während der Vorschau. Weitere Informationen zu Tarifen finden Sie unter [Choose a SKU or tier for Azure Search](../articles/search/search-sku-tier.md) (Auswählen einer SKU oder eines Tarifs für Azure Search).

<sup>2</sup> **Sucheinheiten (SU)** sind gebührenpflichtige Einheiten pro Dienst, die entweder als **Replikat** oder **Partition** zugeordnet werden. Sie benötigen beide Ressourcentypen für Speicher-, Indizierungs- und Abfragevorgänge. Gültige Kombinationen, die innerhalb der maximalen Grenzwerte bleiben, finden Sie unter [Kapazitätsplanung in Azure Search](../articles/search/search-capacity-planning.md).

<sup>3</sup> **Free** basiert auf freigegebenen Ressourcen, die von mehreren Abonnenten verwendet werden. In diesem Tarif gibt es keine dedizierten Ressourcen für einen einzelnen Abonnenten. Aus diesem Grund wird Skalierbarkeit nicht unterstützt.

<sup>4</sup> **Basic** verfügt über eine feste Partition. SUs werden verwendet, um Replikate für die Skalierung der Abfrageworkloads zuzuordnen.

<sup>5</sup> **S3 HD** basiert auf der gleichen Hardware wie S3, jedoch in einer Konfiguration, die für eine große Anzahl von kleineren Indizes optimiert ist. Der Tarif weist eine sehr große Partition statt 12 kleinere Partitionen auf, und er verfügt über maximal 12 Replikate, ähnlich wie S3.

<!---HONumber=AcomDC_0608_2016-->
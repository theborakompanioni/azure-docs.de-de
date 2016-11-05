Sie können mehrere Dienste in einem Abonnement erstellen, die jeweils für einen bestimmten Tarif bereitgestellt werden. Als einzige Einschränkung gilt hierbei die Anzahl von zulässigen Diensten des jeweiligen Tarifs. Beispielsweise können Sie in einem Abonnement bis zu 12 Dienste für den Basic-Tarif und weitere 12 Dienste für den S1-Tarif erstellen. 

Die Obergrenze für die maximale Anzahl von Diensten kann auf Anfrage angehoben werden. Wenden Sie sich an den Support von Azure, wenn Sie in einem Abonnement weitere Dienste benötigen.

| Ressource | Kostenlos | Basic | S1 | S2 | S3 <br/>(Vorschau) <sup>1</sup> | S3 HD <br/>(Vorschau) <sup>1</sup> |
| --- | --- | --- | --- | --- | --- | --- |
| Maximale Anzahl von Diensten |1 |12 |12 |6 |6 |6 |
| Maximale Skalierung in SU <sup>2</sup> |N/V <sup>3</sup> |3 SU <sup>4</sup> |36 SU |36 SU |36 SU |12 SU <sup>5</sup> |

<sup>1</sup> Vorschautarife werden zur Einführung zum halben Preis berechnet. Der volle Preis gilt dann, wenn für den Tarif die allgemeine Verfügbarkeit (GA) erreicht wird. Es gibt keine Vereinbarung zum Servicelevel (SLA) während der Vorschau. Weitere Informationen zu Tarifen finden Sie unter [Auswählen einer SKU oder eines Tarifs für Azure Search](../articles/search/search-sku-tier.md).

<sup>2</sup> Sucheinheiten (SU) sind gebührenpflichtige Einheiten pro Dienst, die entweder als *Replikat* oder *Partition* zugeordnet werden. Sie benötigen beide Ressourcen für Speicher-, Indizierungs- und Abfragevorgänge. Informationen zu gültigen Kombinationen, die innerhalb der maximalen Grenzwerte bleiben, finden Sie unter [Kapazitätsplanung in Azure Search](../articles/search/search-capacity-planning.md). 

<sup>3</sup> Free basiert auf gemeinsamen Ressourcen, die von mehreren Abonnenten verwendet werden. In diesem Tarif gibt es keine dedizierten Ressourcen für einen einzelnen Abonnenten. Aus diesem Grund ist die maximale Skalierung als nicht zutreffend markiert.

<sup>4</sup> Basic verfügt über eine feste Partition. Bei diesem Tarif werden zusätzliche SUs verwendet, um weitere Replikate für umfangreichere Abfrageworkloads zuzuordnen.

<sup>5</sup> S3 HD basiert auf der gleichen Hardware wie S3, aber in einer Konfiguration, die für eine große Anzahl von kleineren Indizes optimiert ist. Ähnlich wie bei Basic wird eine sehr große Partition mit zusätzlichen Sucheinheiten genutzt, die für Replikate verwendet werden können.

<!--HONumber=Oct16_HO2-->



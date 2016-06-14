Ressource|Kostenlos|Basic (Vorschau)|S1|S2
---|---|---|---|----
Maximale Dienste in jedem Tarif pro Abonnement <sup>1</sup>|1 |12 |12 |1
Maximale Skalierung pro Tarif <sup>2</sup>|–|3 SU (bis zu 3 Replikate und 1 Partition)|36 SU|36 SU  

<sup>1</sup> Jeder Dienst wird in einem bestimmten Tarif bereitgestellt. Dabei gelten Grenzwerte für die Anzahl der Dienste, die in jedem Tarif in einem einzelnen Azure-Abonnement bereitgestellt werden können. Während der Vorschauphase sind Tarife zu einem Einführungspreis mit 50 Prozent Nachlass auf den vollen Preis erhältlich.

<sup>2</sup> Grenzwerte für das horizontale Hochskalieren werden in Sucheinheiten (Search Units, SU) pro Tarif definiert. Sucheinheiten sind die abrechenbare Einheit für ein **Replikat** oder eine **Partition**. Sie brauchen beides für Speicher-, Indizierungs- und Abfragevorgänge. Unter [Kapazitätsplanung](../articles/search/search-capacity-planning.md) finden Sie gültige Kombinationen von Replikaten und Partitionen, mit denen Sie im **Basic**- bzw. **Standard**-Tarif unterhalb der maximalen Grenzwerte von 3 oder 36 Einheiten bleiben. Da der Tarif **Free** auf gemeinsamen Ressourcen basiert, die von mehreren Abonnenten genutzt werden, ist das horizontale Hochskalieren hier nicht möglich.

<!---HONumber=AcomDC_0601_2016-->
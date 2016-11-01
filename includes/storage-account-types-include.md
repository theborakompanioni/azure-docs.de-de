Zwei Typen von Speicherkonten stehen zur Verfügung:

### Allgemeine Speicherkonten

Ein allgemeines Speicherkonto bietet Ihnen Zugriff auf Azure Storage-Dienste wie Tabellen, Warteschlangen, Dateien, Blobs und Datenträger von virtuellen Azure-Computern in einem einzigen Konto. Diese Art von Speicherkonto umfasst zwei Leistungsstufen:

- Storage Standard ermöglicht Ihnen das Speichern von Tabellen, Warteschlangen, Dateien, Blobs und Datenträgern von virtuellen Azure-Computern.
- Storage Premium unterstützt derzeit nur Datenträger von virtuellen Azure-Computern. Eine ausführliche Übersicht über Azure Premium Storage finden Sie unter [Premium-Speicher: Hochleistungsspeicher für Arbeitslasten auf virtuellen Azure-Computern](../articles/storage/storage-premium-storage.md).

### BLOB-Speicherkonten

BLOB-Speicherkonten sind spezielle Speicherkonten und dienen dazu, unstrukturierte Daten als Blobs (Objekte) in Azure Storage zu speichern. BLOB-Speicherkonten sind mit Ihren bereits vorhandenen allgemeinen Speicherkonten vergleichbar und besitzen die gleichen Haltbarkeits-, Verfügbarkeits-, Skalierbarkeits- und Leistungseigenschaften, die Sie schon heute verwenden – einschließlich vollständiger API-Konsistenz für Blockblobs und Anfügeblobs. Bei Anwendungen, die nur Block- oder Anfügeblobspeicher benötigen, empfiehlt sich die Verwendung von BLOB-Speicherkonten.

> [AZURE.NOTE] BLOB-Speicherkonten unterstützen nur Block- und Anfügeblobs, keine Seitenblobs.

BLOB-Speicherkonten verfügen über das Attribut **Zugriffsstufe**, das bei der Kontoerstellung festgelegt und später bei Bedarf geändert werden kann. Es gibt zwei Arten von Zugriffsstufen, die auf Grundlage Ihres Datenzugriffsmusters festgelegt werden können:

- Die Zugriffsstufe **Hot**, wenn auf die Objekte im Speicherkonto häufig zugegriffen wird. Dabei fallen bei der Datenspeicherung geringere Zugriffskosten an.
- Die Zugriffsstufe **Cool**, wenn auf die Objekte im Speicherkonto weniger häufig zugegriffen wird. Dabei fallen bei der Datenspeicherung geringere Speicherkosten an.

Bei einer Änderung des Nutzungsmusters der Daten können Sie jederzeit zwischen den beiden Zugriffstarifen wechseln. Die Änderung des Zugriffstarifs kann mit zusätzlichen Kosten verbunden sein. Weitere Informationen dazu finden Sie unter [Preise und Abrechnung für BLOB-Speicherkonten](../articles/storage/storage-blob-storage-tiers.md#pricing-and-billing).

Weitere Informationen zu BLOB-Speicherkonten finden Sie unter [Azure-Blobspeicher: Tarife für selten genutzte Daten (Cool Data) und häufig genutzte Daten (Hot Data)](../articles/storage/storage-blob-storage-tiers.md).

Um ein Speicherkonto erstellen zu können, müssen Sie über ein Azure-Abonnement verfügen. Dies ist ein Plan, der Ihnen Zugriff auf eine Vielzahl von Azure-Diensten verschafft. Sie können die Verwendung von Azure mit einem [kostenlosen Konto](https://azure.microsoft.com/pricing/free-trial/) beginnen. Wenn Sie sich entscheiden, einen Abonnementplan zu kaufen, können Sie zwischen einer Vielzahl von [Kaufoptionen](https://azure.microsoft.com/pricing/purchase-options/) auswählen. [MSDN-Abonnenten](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/) erhalten ein monatliches Gratis-Guthaben für Azure-Dienste, inklusive Azure-Speicher. Informationen zu Volumenpreisen finden Sie unter [Preise für Azure Storage](https://azure.microsoft.com/pricing/details/storage/).

Informationen zum Erstellen eines Standardspeicherkontos finden Sie unter [Erstellen Sie ein Speicherkonto](../articles/storage/storage-create-storage-account.md#create-a-storage-account). Sie können bis zu 100 eindeutig benannte Speicherkonten unter einem einzigen Abonnement erstellen. Weitere Informationen zu den Begrenzungen von Speicherkonten finden Sie unter [Skalierbarkeits- und Leistungsziele für Azure Storage](../articles/storage/storage-scalability-targets.md).

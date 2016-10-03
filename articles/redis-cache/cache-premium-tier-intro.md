<properties 
	pageTitle="Einführung in den Premium-Tarif von Azure Redis Cache | Microsoft Azure" 
	description="Hier erfahren Sie, wie Sie für Ihre Azure Redis Cache-Instanzen im Tarif ";Premium"; Redis-Persistenz Redis-Clustering und VNET-Unterstützung erstellen und verwalten." 
	services="redis-cache" 
	documentationCenter="" 
	authors="steved0x" 
	manager="douge" 
	editor=""/>

<tags 
	ms.service="cache" 
	ms.workload="tbd" 
	ms.tgt_pltfrm="cache-redis" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="09/15/2016" 
	ms.author="sdanie"/>

# Einführung in den Premium-Tarif von Azure Redis Cache
Azure Redis Cache ist ein verteilter, verwalteter Cache, mit der Sie hoch skalierbare und reaktionsschnelle Anwendungen erstellen können, indem Sie äußerst schnellen Zugriff auf Daten bieten.

Der neue Tarif „Premium“ ist speziell auf Unternehmen zugeschnitten. Er enthält alle Features des Tarifs „Standard“ und bietet darüber hinaus weitere Features, z.B. eine bessere Leistung, größere Workloads, Notfallwiederherstellung, Import/Export und verbesserte Sicherheit. Nachfolgend finden Sie weitere Informationen zu den zusätzlichen Features des Cachetarifs "Premium".

## Bessere Leistung als in den Tarifen "Standard" und "Basic"
**Bessere Leistung in den Tarifen "Standard" und "Basic".** Caches im Tarif "Premium" werden auf Hardware mit schnelleren Prozessoren bereitgestellt, die im Vergleich zu den Tarifen "Basic" oder "Standard" eine bessere Leistung bieten. Caches im Tarif "Premium" erreichen einen höheren Durchsatz und geringere Wartezeiten.

**Der Durchsatz für Cache derselben Größe ist im Tarif "Premium" im Vergleich zum Tarif "Standard" höher.** Ein P4-Cache (Premium) mit 53 GB erreicht beispielsweise einen Durchsatz von 250.000 Anforderungen pro Sekunde, während ein C6-Cache (Standard) einen Durchsatz von 150.000 erreicht.

Ausführliche Informationen zu Größe, Durchsatz und Bandbreite von Premium-Caches finden Sie in den [häufig gestellten Fragen zu Azure Redis Cache](cache-faq.md#what-redis-cache-offering-and-size-should-i-use)

## Redis-Datenpersistenz
Der Tarif "Premium" ermöglicht die Persistenz der Cachedaten in einem Azure Storage-Konto. In einem "Basic"-/"Standard"-Cache werden alle Daten nur im Arbeitsspeicher gespeichert. Bei Problemen mit der zugrunde liegenden Infrastruktur kann es zu Datenverlusten kommen. Es wird empfohlen, das Redis-Feature für Datenpersistenz im Tarif "Premium" zu verwenden, um den Schutz vor Datenverlusten zu erhöhen. Azure Redis Cache bietet RDB- und AOF-Optionen (demnächst verfügbar) bei der [Redis-Persistenz](http://redis.io/topics/persistence).

Informationen zum Konfigurieren von Persistenz finden Sie unter [Konfigurieren von Persistenz für Azure Redis Cache vom Typ "Premium"](cache-how-to-premium-persistence.md).

##Redis-Cluster
Wenn Sie Caches mit einer Größe über 53 GB erstellen oder Daten über mehrere Redis-Knoten horizontal partitionieren möchten, können Sie das im Tarif "Premium" verfügbare Redis-Clustering verwenden. Jeder Knoten besteht aus einem von Azure für hohe Verfügbarkeit verwalteten Paar aus primärem Cache und Replikatcache.

**Redis-Clustering bietet Ihnen maximale Skalierung und Durchsatz.** Der Durchsatz steigt linear, je mehr Shards (Knoten) Sie im Cluster verwenden. Wenn Sie z. B. einen P4-Cluster mit 10 Shards erstellen, beträgt der verfügbare Durchsatz 250 KB * 10 = 2,5 Millionen Anforderungen pro Sekunde. Ausführliche Informationen zu Größe, Durchsatz und Bandbreite von Premium-Caches finden Sie in den [häufig gestellten Fragen zu Azure Redis Cache](cache-faq.md#what-redis-cache-offering-and-size-should-i-use).

Informationen zum Einstieg in die Arbeit mit Clustern finden Sie unter [Konfigurieren von Clustern für Azure Redis Cache vom Typ "Premium"](cache-how-to-premium-clustering.md).

##Erhöhte Sicherheit und Isolierung

Auf im Tarif "Basic" oder "Standard" erstellte Caches kann über das öffentliche Internet zugegriffen werden. Der Zugriff auf den Cache ist basierend auf dem Zugriffsschlüssel eingeschränkt. Im Tarif "Premium" können Sie darüber hinaus sicherstellen, dass nur Clients in einem bestimmten Netzwerk auf den Cache zugreifen können. Sie können Redis-Cache in einem [Azure Virtual Network (VNet)](https://azure.microsoft.com/services/virtual-network/) bereitstellen. Sie können alle Features von VNet, z. B. Subnetze, Richtlinien für die Zugriffssteuerung und andere Features, verwenden, um den Zugriff auf Redis weiter einzuschränken.

Weitere Informationen finden Sie unter [Konfigurieren der Unterstützung virtueller Netzwerke für Azure Redis Cache vom Typ "Premium"](cache-how-to-premium-vnet.md).

## Import/Export

Import/Export ist ein Vorgang der Azure Redis Cache-Datenverwaltung, bei dem Sie Daten in Azure Redis Cache importieren oder aus Azure Redis Cache exportieren können. Hierzu importieren bzw. exportieren Sie eine Redis Cache-Datenbankmomentaufnahme (RDB) aus einem Premium-Cache in ein Seitenblob in einem Azure-Speicherkonto. So können Sie zwischen verschiedenen Azure Redis Cache-Instanzen migrieren oder den Cache vor der Nutzung mit Daten auffüllen.

Die Importfunktion kann verwendet werden, um Redis-kompatible RDB-Dateien von beliebigen Redis-Servern zu importieren, die in einer Cloud oder Umgebung ausgeführt werden, z.B. Redis unter Linux oder Windows oder bei einem Cloudanbieter wie Amazon Web Services und anderen. Das Importieren von Daten ist eine einfache Möglichkeit zum Erstellen eines Cache mit vorab aufgefüllten Daten. Während des Importvorgangs lädt Azure Redis Cache die RDB-Dateien aus dem Azure-Speicher in den Arbeitsspeicher und fügt die Schlüssel anschließend in den Cache ein.

Mit der Exportfunktion können Sie die in Azure Redis Cache gespeicherten Daten als Redis-kompatible RDB-Dateien exportieren. Sie können diese Funktion nutzen, um Daten von einer Azure Redis Cache-Instanz zur anderen oder auf einen anderen Redis-Server zu verschieben. Während des Exportvorgangs wird auf dem virtuellen Computer, der die Azure Redis Cache-Serverinstanz hostet, eine temporäre Datei erstellt, die dann in das angegebene Speicherkonto hochgeladen wird. Nachdem der Exportvorgang mit dem Status „Erfolg“ oder „Fehler“ abgeschlossen wurde, wird die temporäre Datei gelöscht.

Weitere Informationen finden Sie unter [How to import data into and export data from Azure Redis Cache](cache-how-to-import-export-data.md) (Gewusst wie: Importieren und Exportieren von Daten in Azure Redis Cache).

## Reboot

Im Premium-Tarif können Sie einen oder mehrere Knoten Ihres Caches bei Bedarf neu starten. Dies ermöglicht das Testen Ihrer Anwendung auf Stabilität im Fall eines Ausfalls. Sie können die folgenden Knoten neu starten.

-	Masterknoten des Caches
-	Slaveknoten des Caches
-	Sowohl den Master- als auch den Slaveknoten des Caches
-	Wenn Sie einen Premium-Cache mit Clustering verwenden, können Sie den Master-, Slave- oder beide Knoten für einzelne Shards im Cache neu starten.

Weitere Informationen finden Sie unter [Neustart](cache-administration.md#reboot) und [Häufig gestellte Fragen zum Neustart](cache-administration.md#reboot-faq).

## Planen von Updates

Mit dem Feature zum Planen von Updates können Sie ein Wartungsfenster für Ihren Cache bestimmen. Wenn das Wartungsfenster angegeben ist, erfolgen alle Redis-Serverupdates in diesem Zeitfenster. Um ein Wartungsfenster anzugeben, wählen Sie die gewünschten Tage aus. Geben Sie die Startzeit des Wartungsfensters für jeden Tag an. Beachten Sie, dass die Zeit im Wartungsfenster als UTC angegeben ist.

Weitere Informationen finden Sie unter [Planen von Updates](cache-administration.md#schedule-updates) und [Häufig gestellte Fragen zum Planen von Updates](cache-administration.md#schedule-updates-faq).

>[AZURE.NOTE] Nur Updates des Redis-Servers erfolgen während des geplanten Wartungsfensters. Das Wartungsfenster gilt nicht für Azure-Updates oder Updates des Betriebssystems virtueller Computer.

## So wechseln Sie zum Premium-Tarif

Um zum Premium-Tarif zu wechseln, wählen Sie einfach auf dem Blatt **Tarif ändern** einen der Premium-Tarife aus. Sie können für Ihren Cache auch mithilfe von PowerShell und CLI zum Premium-Tarif wechseln. Eine detaillierte Anleitung finden Sie unter [Skalieren von Azure Redis Cache](cache-how-to-scale.md) und [Automatisieren eines Skalierungsvorgangs](cache-how-to-scale.md#how-to-automate-a-scaling-operation).

## Nächste Schritte

Erstellen Sie einen Cache, und untersuchen Sie die neuen Features des "Premium"-Tarifs.

-	[Konfigurieren von Persistenz für Azure Redis Cache vom Typ "Premium"](cache-how-to-premium-persistence.md)
-	[Konfigurieren der Unterstützung virtueller Netzwerke für Azure Redis Cache vom Typ "Premium"](cache-how-to-premium-vnet.md)
-	[Konfigurieren von Clustern für Azure Redis Cache vom Typ "Premium"](cache-how-to-premium-clustering.md)
-	[How to import data into and export data from Azure Redis Cache (Gewusst wie: Importieren und Exportieren von Daten in Azure Redis Cache)](cache-how-to-import-export-data.md)
-	[Verwalten von Azure Redis Cache](cache-administration.md)
  

<!---HONumber=AcomDC_0921_2016-->
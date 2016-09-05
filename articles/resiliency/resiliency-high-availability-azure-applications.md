<properties
   pageTitle="Hohe Verfügbarkeit für Azure-Anwendungen | Microsoft Azure"
   description="Technische Übersichten und ausführliche Informationen zum Entwerfen und Erstellen von Anwendungen für hohe Verfügbarkeit in Microsoft Azure."
   services=""
   documentationCenter="na"
   authors="adamglick"
   manager="saladki"
   editor=""/>

<tags
   ms.service="resiliency"
   ms.devlang="na"
   ms.topic="article"
   ms.tgt_pltfrm="na"
   ms.workload="na"
   ms.date="08/18/2016"
   ms.author="aglick"/>

#Hohe Verfügbarkeit für in Microsoft Azure erstellte Anwendungen

Eine hoch verfügbare Anwendung gleicht Schwankungen bei Verfügbarkeit und Last sowie vorübergehende Ausfälle in den abhängigen Diensten und Hardwarekomponenten aus. Die Anwendung funktioniert hinsichtlich der Benutzerzufriedenheit und Systemreaktion weiterhin auf einem akzeptablen Niveau, wie dies durch Geschäftsanforderungen oder Vereinbarungen zum Servicelevel (SLAs) für die Anwendung definiert ist.

##Funktionen von Azure für hohe Verfügbarkeit

Azure weist viele integrierte Plattformfunktionen auf, die hoch verfügbare Anwendungen unterstützen. In diesem Abschnitt werden einige dieser wichtigen Funktionen beschrieben. Eine umfassendere Analyse der Plattform finden Sie unter [Technischer Leitfaden zur Resilienz in Azure](./resiliency-technical-guidance.md).

###Fabric Controller

Mit dem Azure Fabric Controller wird der Zustand der Azure-Computeinstanzen bereitgestellt und überwacht. Der Fabric Controller überprüft den Status der Hardware und Software für die Host- und Gastcomputerinstanzen. Wenn ein Fehler erkannt wird, werden SLAs durch automatisches Verschieben der VM-Instanzen erzwungen. Das Konzept von Fehler- und Upgradedomänen unterstützt Compute-SLAs zusätzlich.

Wenn mehrere Clouddienst-Rolleninstanzen bereitgestellt werden, stellt Azure diese in verschiedenen Fehlerdomänen bereit. Eine Fehlerdomänengrenze ist im Grunde ein anderes Hardwarerack in der gleichen Region. Fehlerdomänen reduzieren die Wahrscheinlichkeit, dass ein ermittelter Hardwarefehler die Dienstbereitstellung einer Anwendung unterbricht. Sie können die Anzahl von Fehlerdomänen, die Ihren Worker- oder Webrollen zugeordnet werden, nicht verwalten. Der Fabric Controller verwendet dedizierte Ressourcen, die von den in Azure gehosteten Anwendungen getrennt sind. Er weist eine Verfügbarkeit von 100% auf, da er das Kernstück des Azure-Systems darstellt. Er überwacht und verwaltet Rolleninstanzen über Fehlerdomänen hinweg.

Im folgenden Diagramm sind gemeinsam genutzte Azure-Ressourcen dargestellt, die vom Fabric Controller über verschiedene Fehlerdomänen hinweg bereitgestellt und verwaltet werden.

![Vereinfachte Darstellung der Fehlerdomänenisolation](./media/resiliency-high-availability-azure-applications/fault-domain-isolation.png)

Upgradedomänen ähneln Fehlerdomänen in der Funktionsweise, unterstützen aber Upgrades statt Fehler. Eine Upgradedomäne ist eine logische Einheit der Instanzentrennung, die festlegt, welche Instanzen in einem bestimmten Dienst zu einem bestimmten Zeitpunkt aktualisiert werden. Für Ihre gehostete Dienstbereitstellung sind standardmäßig fünf Upgradedomänen definiert. Sie können diesen Wert jedoch in der Dienstdefinitionsdatei ändern. Angenommen, Sie nutzen acht Instanzen Ihrer Webrolle. Es gibt zwei Instanzen in drei Upgradedomänen sowie zwei Instanzen in einer Upgradedomäne. Azure definiert die Updatesequenz, aber die Basis ist die Anzahl von Upgradedomänen. Weitere Informationen zu Upgradedomänen finden Sie unter [Aktualisieren eines Clouddiensts](../cloud-services/cloud-services-update-azure-service.md).

###Funktionen in anderen Diensten

Zusätzlich zu den Plattformfunktionen, die eine hohe Computeverfügbarkeit unterstützen, bettet Azure Hochverfügbarkeitsfunktionen in die anderen Dienste ein. Beispielsweise werden drei Replikate aller Blob-, Tabellen- und Warteschlangendaten von Azure Storage gespeichert. Zudem gibt es die Option der Georeplikation, um Sicherungen von Blobs und Tabellen in einer sekundären Region zu speichern. Das Azure Content Delivery Network ermöglicht die Zwischenspeicherung von Blobs auf der ganzen Welt, um sowohl Redundanz und Skalierbarkeit sicherzustellen. In Azure SQL-Datenbank werden auch mehrere Replikate gespeichert.

Zusätzlich zu den Artikeln der Reihe [Technischer Leitfaden zur Resilienz in Azure](https://aka.ms/bctechguide) finden Sie weitere Informationen unter [Best Practices for the Design of Large-Scale Services on Azure Cloud Services](https://azure.microsoft.com/blog/best-practices-for-designing-large-scale-services-on-windows-azure/) (Bewährte Methoden für den Entwurf umfangreicher Dienste in Azure Cloud Services). Diese Artikel bieten eine eingehendere Erläuterung der Verfügbarkeitsfunktionen der Azure-Plattform.

Obwohl Azure mehrere Funktionen zur Unterstützung einer hohen Verfügbarkeit bereitstellt, ist es wichtig, die Grenzen dieser Funktionen zu kennen:

- Azure garantiert, dass Ihre Rollen verfügbar sind und ausgeführt werden, hat jedoch keine Kenntnis darüber, ob Ihre Anwendung ausgeführt wird oder überlastet ist.
- Für Azure SQL-Datenbank werden Daten synchron innerhalb der Region repliziert. Sie können die aktive Georeplikation auswählen, bei der bis zu vier zusätzliche Datenbankkopien in derselben Region (oder verschiedenen Regionen) zulässig sind. Diese Datenbankreplikate sind keine Point-in-Time-Sicherungen. SQL-Datenbanken verfügen über Funktionen für Point-in-Time-Sicherungen. Weitere Informationen zu Funktionen für Point-in-Time-Sicherungen von SQL-Datenbanken finden Sie unter [Azure SQL Database Point in Time Restore](https://azure.microsoft.com/blog/azure-sql-database-point-in-time-restore/) (Azure SQL-Datenbank – Point-in-Time-Wiederherstellung).
- Für Azure Storage werden Tabellen- und Blobdaten standardmäßig in eine andere Region repliziert. Sie können auf die Replikate jedoch erst zugreifen, nachdem Microsoft ein Failover zum alternativen Standort durchgeführt hat. Ein Regionsfailover findet nur im Fall einer längeren regionsweiten Dienstunterbrechung statt, und es gibt keine SLA für die Dauer eines geografischen Failovers. Beachten Sie auch, dass sich jegliche Datenbeschädigungen schnell auf die Replikate ausbreiten.

Daher müssen Sie die Plattformverfügbarkeitsfunktionen durch anwendungsspezifische Verfügbarkeitsfunktionen ergänzen. Zu den anwendungsspezifischen Verfügbarkeitsfunktionen gehören Blobmomentaufnahmen, um Point-in-Time-Sicherungen von Blobdaten zu erstellen.

###Verfügbarkeitsgruppen für virtuelle Azure-Computer

Im Großteil dieses Artikels geht es um Clouddienste, die ein PaaS-Modell (Platform as a Service) verwenden. Es gibt aber auch bestimmte Verfügbarkeitsfunktionen für virtuelle Azure-Computer, die ein IaaS-Modell (Infrastructure as a Service) verwenden. Um eine hohe Verfügbarkeit für virtuelle Computer zu erzielen, müssen Sie Verfügbarkeitsgruppen verwenden. Eine Verfügbarkeitsgruppe stellt eine ähnliche Funktion für Fehler- und Upgradedomänen bereit. Innerhalb einer Verfügbarkeitsgruppe positioniert Azure die virtuellen Computer so, dass ermittelte Hardwarefehler und Wartungsaktivitäten nicht dazu führen, dass alle Computer in dieser Gruppe ausfallen. Verfügbarkeitsgruppen sind erforderlich, um die Azure-SLA für die Verfügbarkeit von virtuellen Computern zu erreichen.

Das folgende Diagramm zeigt eine Darstellung von zwei Verfügbarkeitsgruppen, die virtuelle Webcomputer bzw. virtuelle SQL Server-Computer gruppieren.

![Verfügbarkeitsgruppen für virtuelle Azure-Computer](./media/resiliency-high-availability-azure-applications/availability-set-for-azure-virtual-machines.png)

>[AZURE.NOTE] Im vorherigen Diagramm ist SQL Server installiert und wird auf virtuellen Computern ausgeführt. Dies unterscheidet sich von Azure SQL-Datenbank, wo eine Datenbank als verwalteter Dienst bereitgestellt wird.

##Anwendungsstrategien für hohe Verfügbarkeit

Die meisten Anwendungsstrategien für hohe Verfügbarkeit umfassen entweder Redundanz oder die Entfernung von festen Abhängigkeiten zwischen Anwendungskomponenten. Der Anwendungsentwurf sollte Fehlertoleranz während zeitweiliger Ausfälle von Azure oder Drittanbieterdiensten unterstützen. In den folgenden Abschnitten werden Anwendungsmuster zum Erhöhen der Verfügbarkeit Ihrer Clouddienste beschrieben.

###Asynchrone Kommunikation und permanente Warteschlangen

Betrachten wir die asynchrone Kommunikation zwischen lose gekoppelten Diensten zum Erhöhen der Verfügbarkeit in Azure-Anwendungen. In diesem Muster werden Nachrichten zur späteren Verarbeitung entweder an Speicherwarteschlangen oder an Azure Service Bus-Warteschlangen geschrieben. Wenn Sie die Nachricht an die Warteschlange schreiben, wird die Steuerung sofort an den Absender der Nachricht zurückgegeben. Die Nachrichtenverarbeitung, die üblicherweise als Workerrolle implementiert ist, erfolgt auf einer anderen Anwendungsebene. Wenn die Workerrolle ausfällt, werden die Nachrichten in der Warteschlange gesammelt, bis der Verarbeitungsdienst wiederhergestellt wird. Solange die Warteschlange verfügbar ist, gibt es keine direkte Abhängigkeit zwischen dem Absender im Front-End und dem Nachrichtenprozessor. Dadurch sind keine synchronen Dienstaufrufe erforderlich, die in verteilten Anwendungen einen Durchsatzengpass darstellen können.

Eine Variante dieses Verfahrens verwendet Azure Storage (Blobs, Tabellen, Warteschlangen) oder Service Bus-Warteschlangen als Failoverspeicherort für fehlerhafte Datenbankaufrufe. Ein Beispiel: Ein synchroner Aufruf an einen anderen Dienst innerhalb einer Anwendung (z.B. Azure SQL-Datenbank) führt wiederholt zu einem Fehler. Sie können diese Daten möglicherweise in einem permanenten Speicher serialisieren. Zu einem späteren Zeitpunkt, wenn der Dienst oder die Datenbank wieder online ist, kann die Anwendung die Anforderung aus dem Speicher erneut senden. Der Unterschied in diesem Modell besteht darin, dass der temporäre Speicherort kein fester Bestandteil des Anwendungsworkflows ist. Er wird nur in Fehlerszenarien verwendet.

In beiden Szenarien verhindern die asynchrone Kommunikation und der temporäre Speicher, dass ein ausgefallener Back-End-Dienst zu einem Ausfall der gesamten Anwendung führt. Warteschlangen dienen als logisches Zwischenelement. Weitere Informationen zur Auswahl des richtigen Warteschlangendiensts finden Sie unter [Azure-Warteschlangen und Azure Service Bus-Warteschlangen – Vergleich und Gegenüberstellung](../service-bus/service-bus-azure-and-service-bus-queues-compared-contrasted.md).

###Fehlererkennung und Wiederholungslogik

Ein wichtiger Punkt beim Entwerfen von hoch verfügbaren Anwendungen ist die Verwendung einer Wiederholungslogik im Code, um einen vorübergehend ausgefallenen Dienst richtig behandeln zu können. Der vom Microsoft Patterns and Practices-Team entwickelte [Transient Fault Handling Application Block](https://msdn.microsoft.com/library/hh680934.aspx) (Anwendungsblock zur Behandlung vorübergehender Fehler) unterstützt Anwendungsentwickler bei diesem Prozess. Der Begriff „vorübergehend“ bezeichnet einen Zustand, der nur für einen relativ kurzen Zeitraum anhält. Im Rahmen dieses Artikels ist die Behandlung von vorübergehenden Fehlern Teil der Entwicklung einer hoch verfügbaren Anwendung. Bei vorübergehenden Zuständen kann es sich beispielsweise um zeitweilige Netzwerkfehler und unterbrochene Datenbankverbindungen handeln.

Der Anwendungsblock zur Behandlung vorübergehender Fehler ist eine vereinfachte Möglichkeit, mit der Sie Fehler in Ihrem Code ordnungsgemäß behandeln können. Sie können hiermit die Verfügbarkeit Ihrer Anwendungen verbessern, indem Sie eine stabile Logik zur Behandlung vorübergehender Fehler hinzufügen. In den meisten Fällen verarbeitet die Wiederholungslogik die kurze Unterbrechung und verbindet Sender und Empfänger nach einem oder mehreren fehlgeschlagenen Versuchen erneut. Anwendungsbenutzer bemerken einen erfolgreichen Wiederholungsversuch üblicherweise nicht.

Entwicklern stehen drei Optionen zur Verwaltung der Wiederholungslogik zur Verfügung: inkrementell, mit festen Intervallen und exponentiell. Bei der Option „inkrementell“ wächst die Zeitspanne zwischen den Wiederholungsversuchen linear an (z.B. 1, 2, 3 und 4 Sekunden). Bei der Option „festes Intervall“ ist die Zeitspanne zwischen den Wiederholungsversuchen immer gleich (z.B. 2 Sekunden). Bei der zufälligeren Option „exponentiell“ sind die Zeitspannen zwischen den Wiederholungsversuchen länger. Das Muster der Wiederholungsversuche ist exponentiell (z.B. 2, 4, 8 und 16 Sekunden).

Die allgemeine Strategie für Ihren Code lautet wie folgt:

1. Definieren Sie die Wiederholungsstrategie und -richtlinie.
1. Versuchen Sie, den Vorgang auszuführen, der möglicherweise einen vorübergehenden Fehler verursacht.
1. Wenn ein vorübergehender Fehler auftritt, rufen Sie die Wiederholungsrichtlinie auf.
1. Wenn alle Wiederholungsversuche zu Fehlern führen, fangen Sie eine abschließende Ausnahme ab.

Testen Sie die Wiederholungslogik mit simulierten Fehlern, um sicherzustellen, dass Wiederholungen bei aufeinanderfolgenden Vorgängen keine unvorhergesehenen langen Verzögerungen verursachen. Führen Sie diesen Vorgang aus, bevor Sie sich entscheiden, die Aufgabe insgesamt als fehlerhaft zu kennzeichnen.

###Verweisdatenmuster für hohe Verfügbarkeit

Verweisdaten sind die schreibgeschützten Daten einer Anwendung. Diese Daten stellen den Geschäftskontext bereit, innerhalb dessen die Anwendung während eines Geschäftsvorgangs Transaktionsdaten generiert. Transaktionsdaten sind eine Point-in-Time-Funktion der Verweisdaten. Daher richtet sich die Integrität dieser Daten nach der Momentaufnahme der Verweisdaten zum Zeitpunkt der Transaktion. Diese Definition ist zwar etwas allgemein gehalten, reicht aber für den Zweck dieses Artikels aus.

Verweisdaten im Kontext einer Anwendung sind für die Funktionsweise der Anwendung notwendig. Die jeweiligen Anwendungen erstellen und verwalten Verweisdaten – Systeme zur Masterdatenverwaltung (MDM) führen diese Funktion häufig aus. Diese Systeme sind für den Lebenszyklus der Verweisdaten zuständig. Beispiele für Verweisdaten: Produktkataloge, Mitarbeitermaster, Teilemaster und Gerätemaster. Verweisdaten können auch von außerhalb der Organisation stammen, z.B. Postleitzahlen oder Steuersätze. Strategien zur Erhöhung der Verfügbarkeit von Verweisdaten sind üblicherweise weniger kompliziert als Strategien für Transaktionsdaten. Verweisdaten haben den Vorteil, größtenteils unveränderlich zu sein.

Sie können Web- und Workerrollen in Azure erstellen, die Verweisdaten autonom zur Laufzeit verwenden, indem Sie die Verweisdaten zusammen mit der Anwendung bereitstellen. Wenn die Größe des lokalen Speichers eine solche Bereitstellung zulässt, ist dies die ideale Lösung. Eingebettete Datenbanken (SQL, NoSQL) oder in einem lokalen Dateisystem bereitgestellte XML-Dateien unterstützen die Eigenständigkeit von Azure-Computeskalierungseinheiten. Sie sollten jedoch einen Mechanismus einrichten, um die Daten in jeder Rolle ohne erneute Bereitstellung zu aktualisieren. Platzieren Sie zu diesem Zweck alle Updates der Verweisdaten in einem Cloudspeicherendpunkt (z.B. einem Azure-Blobspeicher oder einer SQL-Datenbank). Fügen Sie jeder Rolle Code hinzu, der die Datenupdates beim Starten der Rolle in die Computeknoten herunterlädt. Alternativ dazu können Sie auch Code hinzufügen, der einem Administrator erlaubt, einen erzwungenen Download in die Rolleninstanzen durchzuführen.

Um die Verfügbarkeit zu erhöhen, sollten Sie Rollen auch einen Satz Verweisdaten enthalten, falls der Speicher ausfällt. Auf diese Weise können die Rollen mit einem Basissatz an Verweisdaten starten, bis die Speicherressource für die Updates verfügbar wird.

![Hohe Anwendungsverfügbarkeit durch autonome Computeknoten](./media/resiliency-high-availability-azure-applications/application-high-availability-through-autonomous-compute-nodes.png)

Eine Überlegung für dieses Muster ist die Bereitstellungs- und Startgeschwindigkeit für Ihre Rollen. Wenn Sie große Mengen an Verweisdaten beim Start bereitstellen oder herunterladen, kann dies die für das Starten neuer Bereitstellungen oder Rolleninstanzen erforderliche Zeitspanne erhöhen. Dieser Nachteil kann jedoch akzeptabel sein, wenn Sie dafür die Autonomie erhalten, dass die Verweisdaten für jede Rolle sofort verfügbar sind und Sie sich nicht auf externe Speicherdienste verlassen müssen.

###Transaktionsdatenmuster für hohe Verfügbarkeit

Transaktionsdaten sind die Daten, die von einer Anwendung in einem Geschäftskontext generiert werden. Transaktionsdaten sind eine Kombination aus den Geschäftsprozessen, die von der Anwendung implementiert werden, und den Verweisdaten, die diese Prozesse unterstützen. Beispiele für Transaktionsdaten sind Bestellungen, erweiterte Versandmitteilungen, Rechnungen und CRM-Verkaufschancen. Die auf diese Weise generierten Transaktionsdaten werden zu Buchführungszwecken oder zur weiteren Verarbeitung an externe Systeme übermittelt.

Bedenken Sie, dass Verweisdaten sich innerhalb der für diese Daten zuständigen Systeme verändern können. Aus diesem Grund müssen Transaktionsdaten den Point-in-Time-Verweisdatenkontext speichern, sodass sie nur minimale externe Abhängigkeiten für die semantische Konsistenz aufweisen. Beispiel: Ein Produkt wird einige Monate nach Ausführung einer Bestellung aus dem Katalog entfernt. Es empfiehlt sich, so viel Verweisdatenkontext wie möglich in die Transaktion einzubetten. Dadurch wird die mit der Transaktion verknüpfte Semantik beibehalten, auch wenn sich die Verweisdaten nach der Erfassung der Transaktion ändern.

Wie bereits erwähnt, bieten sich Architekturen, die eine lose Kopplung und asynchrone Kommunikation verwenden, für eine höhere Verfügbarkeit an. Dies gilt auch für Transaktionsdaten, die Implementierung ist jedoch komplexer. In traditionellen Transaktionssystemen garantiert üblicherweise die Datenbank die Transaktion. Wenn Sie Zwischenschichten einführen, muss der Anwendungscode die Daten in verschiedenen Schichten ordnungsgemäß verarbeiten, um Konsistenz und Dauerhaftigkeit sicherzustellen.

Die folgende Sequenz beschreibt einen Workflow, der die Erfassung der Transaktionsdaten von ihrer Verarbeitung trennt:

1. Webcomputeknoten: Vorlegen der Verweisdaten.
1. Externer Speicher: Speichern der Transaktionszwischendaten.
1. Webcomputeknoten: Abschließen der Endbenutzertransaktion.
1. Webcomputeknoten: Senden der abgeschlossenen Transaktionsdaten zusammen mit dem Verweisdatenkontext an einen temporären permanenten Speicher, für den eine vorhersagbare Reaktion garantiert wird.
1. Webcomputeknoten: Signalisieren des Abschlusses der Transaktion an den Benutzer.
1. Hintergrund-Computeknoten: Extrahieren der Transaktionsdaten, Nachbearbeiten der Daten bei Bedarf und Senden der Daten an den endgültigen Speicherort im aktuellen System.

Das folgende Diagramm zeigt eine mögliche Implementierung dieses Entwurfs in einem in Azure gehosteten Clouddienst.

![Hohe Verfügbarkeit durch lose Kopplung](./media/resiliency-disaster-recovery-high-availability-azure-applications/application-high-availability-through-loose-coupling.png)

Die gestrichelten Pfeile im vorherigen Diagramm weisen auf eine asynchrone Verarbeitung hin. Die Front-End-Webrolle hat keine Kenntnis über diese asynchrone Verarbeitung. Dadurch wird die Transaktion an ihrem endgültigen Ziel mit Verweis auf das aktuelle System gespeichert. Aufgrund der durch dieses asynchrone Modell eingeführten Latenz stehen die Transaktionsdaten nicht sofort für Abfragen zur Verfügung. Daher muss jede Transaktionsdateneinheit in einem Cache oder einer Benutzersitzung gespeichert werden, um die sofortigen Anforderungen der Benutzeroberfläche zu erfüllen.

Die Webrolle ist vom Rest der Infrastruktur unabhängig. Ihr Verfügbarkeitsprofil ist eine Kombination der Webrolle und der Azure-Warteschlange, nicht der gesamten Infrastruktur. Dieser Ansatz sorgt nicht nur für hohe Verfügbarkeit, sondern ermöglicht auch eine horizontale Skalierung der Webrolle, unabhängig vom Back-End-Speicher. Dieses Modell für hohe Verfügbarkeit kann die Wirtschaftlichkeit von Vorgängen beeinflussen. Zusätzliche Komponenten wie Azure-Warteschlangen und -Workerrollen können sich auf die monatlichen Nutzungskosten auswirken.

Beachten Sie, dass das vorherige Diagramm nur eine Implementierung dieses entkoppelten Ansatzes für Transaktionsdaten zeigt. Es gibt viele weitere mögliche Implementierungen. Die folgende Liste enthält einige Alternativen:

 * Eine Workerrolle kann zwischen Webrolle und Speicherwarteschlange platziert werden.
 * Eine Service Bus-Warteschlange kann anstelle einer Azure Storage-Warteschlange verwendet werden.
 * Beim endgültigen Ziel kann es sich um Azure Storage oder einen anderen Datenbankanbieter handeln.
 * Azure Cache kann auf der Webschicht verwendet werden, um die unmittelbaren Anforderungen an eine Zwischenspeicherung nach der Transaktion zu erfüllen.

###Skalierbarkeitsmuster

Es ist wichtig zu beachten, dass sich die Skalierbarkeit des Clouddiensts direkt auf die Verfügbarkeit auswirkt. Wenn Ihr Dienst aufgrund einer erhöhten Last nicht mehr reagiert, haben die Benutzer den Eindruck, dass die Anwendung ausgefallen ist. Befolgen Sie die bewährten Methoden für die Skalierbarkeit, basierend auf der erwarteten Anwendungslast und Ihren zukünftigen Erwartungen. Eine sehr hohe Verfügbarkeit erfordert eine Vielzahl von Überlegungen, z.B. die Verwendung einzelner oder mehrerer Speicherkonten, die gemeinsame Nutzung über mehrere Datenbanken hinweg sowie Strategien für die Zwischenspeicherung. Detaillierte Einblicke in diese Muster finden Sie unter [Best Practices for the Design of Large-Scale Services on Azure Cloud Services](https://azure.microsoft.com/blog/best-practices-for-designing-large-scale-services-on-windows-azure/) (Bewährte Methoden für den Entwurf umfangreicher Dienste in Azure Cloud Services).

##Nächste Schritte

Dieser Artikel ist Teil einer Reihe von Artikeln mit Fokus auf [Notfallwiederherstellung und hohe Verfügbarkeit für in Microsoft Azure erstellte Anwendungen](./resiliency-disaster-recovery-high-availability-azure-applications.md). Der nächste Artikel dieser Reihe ist [Notfallwiederherstellung für in Microsoft Azure erstellte Anwendungen](./resiliency-disaster-recovery-azure-applications.md).

<!---HONumber=AcomDC_0824_2016-->
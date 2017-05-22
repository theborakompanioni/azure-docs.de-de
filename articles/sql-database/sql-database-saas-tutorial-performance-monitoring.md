---
title: "Überwachen der Leistung einer SaaS-App für SQL-Datenbank | Microsoft-Dokumentation"
description: "Überwachen und Verwalten der Leistung für die Wingtip Tickets-Beispiel-App (WTP) für Azure SQL-Datenbank"
keywords: Tutorial zur SQL-Datenbank
services: sql-database
documentationcenter: 
author: stevestein
manager: jhubbard
editor: 
ms.assetid: 
ms.service: sql-database
ms.custom: tutorial
ms.workload: data-management
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: hero-article
ms.date: 05/10/2017
ms.author: billgib; sstein
ms.translationtype: Human Translation
ms.sourcegitcommit: fc4172b27b93a49c613eb915252895e845b96892
ms.openlocfilehash: af9511978718af10c97bee6af3a2835c9d2c1ff4
ms.contentlocale: de-de
ms.lasthandoff: 05/12/2017


---
# <a name="monitor-performance-of-the-wtp-sample-saas-application"></a>Überwachen der Leistung der SaaS-Beispielanwendung WTP

In diesem Tutorial werden die integrierten Überwachungs- und Benachrichtigungsfeatures von SQL-Datenbank und Pools für elastische Datenbanken veranschaulicht, und anschließend werden verschiedene Szenarien für die Verwaltung wichtiger Leistungsaspekte in SaaS-Anwendungen beschrieben.

Die App Wingtip Tickets verwendet ein Datenmodell mit einem einzelnen Mandanten, wobei jeder Veranstaltungsort (Mandant) über eine eigene Datenbank verfügt. Wie viele SaaS-Anwendungen ist das erwartete Workloadmuster des Mandanten unvorhersehbar und sporadisch. Mit anderen Worten, Ticketverkäufe können jederzeit erfolgen. Um von diesem typischen Datenbanknutzungsmuster zu profitieren, werden Mandantendatenbanken in Pools für elastische Datenbanken bereitgestellt. Pools für elastische Datenbanken optimieren die Kosten einer Lösung, indem Ressourcen über viele Datenbanken hinweg gemeinsam genutzt werden. Bei einem solchen Muster ist es wichtig, die Datenbank- und Poolressourcenverwendung zu überwachen, um sicherzustellen, dass die Lasten gleichmäßig auf die Pools verteilt werden. Sie müssen zudem sicherstellen, dass die einzelnen Datenbanken über ausreichende Ressourcen verfügen, und dass die Pools ihre [eDTU](sql-database-what-is-a-dtu.md)-Grenzwerte nicht erreichen. In diesem Tutorial werden Methoden zum Überwachen und Verwalten von Datenbanken und Pools behandelt, und es wird erklärt, wie Sie Korrekturmaßnahmen als Reaktion auf Workloadschwankungen ergreifen.

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]

> * Simulieren der Verwendung der Mandantendatenbanken durch Ausführen eines bereitgestellten Lastengenerators
> * Überwachen der Mandantendatenbanken während ihrer Reaktion auf einen Lastenanstieg
> * Zentrales Hochskalieren von Pools für elastische Datenbanken als Reaktion auf die erhöhte Datenbankauslastung
> * Bereitstellen eines zweiten Pools für elastische Datenbanken, um einen Lastenausgleich der Datenbankaktivität vorzunehmen


Stellen Sie zum Durchführen dieses Tutorials sicher, dass die folgenden Voraussetzungen erfüllt sind:

* Die WTP-App wurde bereitgestellt. Unter [Deploy and explore the WTP SaaS application (Bereitstellen und Erkunden der SaaS-Anwendung von WTP)](sql-database-saas-tutorial.md) finden Sie Informationen dazu, wie Sie die App in weniger als fünf Minuten bereitstellen.
* Azure PowerShell wurde installiert. Weitere Informationen hierzu finden Sie unter [Erste Schritte mit Azure PowerShell](https://docs.microsoft.com/powershell/azure/get-started-azureps).

## <a name="introduction-to-saas-performance-management-patterns"></a>Einführung in SaaS-Leistungsverwaltungsmuster

Die Verwaltung der Datenbankleistung umfasst das Kompilieren und Analysieren von Leistungsdaten sowie das Reagieren auf diese Daten durch Anpassen von Parametern, um eine akzeptable Reaktionszeit für Ihre Anwendung zu erhalten. Werden mehrere Mandanten gehostet, sind Pools für elastische Datenbanken eine kostengünstige Möglichkeit, Ressourcen für eine Gruppe von Datenbanken mit unvorhersehbaren Workloads bereitzustellen und zu verwalten. Bei bestimmten Workloadmustern können davon profitieren, nur zwei S3-Datenbanken in einem Pool zu verwalten. Bei einem Pool werden nicht nur die Kosten für Ressourcen geteilt, es kann auch die Notwendigkeit entfallen, einzelne Datenbanken ständig zu überwachen und nachzuverfolgen.

![Medien](./media/sql-database-saas-tutorial-performance-monitoring/app-diagram.png)

Pools und die Datenbanken in den Pools müssen jedoch weiterhin überwacht werden, um sicherzustellen, dass die Leistung innerhalb zulässiger Bereiche bleibt. Optimieren Sie die Konfiguration der Pools so, dass die Anforderungen des aggregierten Workloads erfüllt sind, und stellen Sie sicher, dass die Pool-eDTUs für den Gesamtworkload angemessen sind. Passen Sie die eDTU-Mindest- und Höchstwerte pro Datenbank an die entsprechenden Werte für Ihre spezifischen Anwendungsanforderungen an.

### <a name="performance-management-strategies"></a>Leistungsverwaltungsstrategien

* Um zu vermeiden, die Leistung manuell überwachen zu müssen, ist es am effizientesten, **Benachrichtigungen einzurichten, die ausgelöst werden, wenn Datenbanken oder Pools vom Normalbereich abweichen**.
* Um auf kurzfristige Schwankungen in der aggregierten Leistungsstufe eines Pools zu reagieren, **kann die Pool-eDTU-Stufe zentral hoch- oder herunterskaliert werden**. Wenn diese Fluktuation in regelmäßigen oder vorhersagbaren Abständen auftritt, **kann die Skalierung des Pools geplant werden und automatische erfolgen**. Skalieren Sie z.B. zentral herunter, wenn Sie wissen, dass der Workload gering sein wird, beispielsweise nachts oder an den Wochenenden.
* Um auf längerfristige Schwankungen oder Änderungen bei der Anzahl der Datenbanken zu reagieren, **können einzelne Datenbanken in anderen Pools verschoben werden**.
* Um auf kurzfristig Anstiege *einzelner* Datenbankauslastungen zu reagieren, **können einzelne Datenbanken für einen bestimmten Zeitraum aus dem Pool entfernt und einer individuellen Leistungsstufe zugewiesen werden**. Sobald die Last sich verringert, kann die Datenbank wieder dem Pool hinzugefügt werden. Wenn dies im Voraus bekannt ist, können Datenbanken vorsorglich verschoben werden, um sicherzustellen, dass die Datenbank immer über die erforderlichen Ressourcen verfügt, und um Auswirkungen auf andere Datenbanken im Pool zu vermeiden. Wenn diese Anforderungen vorhersagbar sind, wenn beispielsweise für eine beliebte Veranstaltung an einem Veranstaltungsort ein Run auf die Tickets zu erwarten ist, kann dieses Verwaltungsverhalten in die Anwendung integriert werden.

Das [Azure-Portal](https://portal.azure.com) bietet integrierte Überwachung und Benachrichtigungen für die meisten Ressourcen. In SQL-Datenbank sind Überwachung und Benachrichtigungen für Datenbanken und Pools verfügbar. Die integrierte Überwachungs- und Benachrichtigungsfunktionalität ist ressourcenspezifisch, daher lässt sie sich bequem für eine kleine Anzahl von Ressourcen verwenden, ist aber bei der Arbeit mit viele Ressourcen nicht sehr praktisch.

Für Szenarien mit hoher Nutzung kann Log Analytics verwendet werden (auch als OMS bekannt). Dies ist ein separater Azure-Dienst, der Analysen ausgegebener Diagnoseprotokolle und Telemetriedaten bietet, die im Protokollanalyse-Arbeitsbereich gesammelt werden, der Telemetriedaten aus vielen Diensten sammeln und zum Abfragen und Festlegen von Benachrichtigungen verwendet werden kann.

## <a name="get-the-wingtip-application-scripts"></a>Abrufen des Wingtip-Anwendungsskripts

Die Wingtip Tickets-Skripts und der Quellcode der Anwendung stehen im [WingtipSaaS](https://github.com/Microsoft/WingtipSaaS)-GitHub-Repository zur Verfügung. Skriptdateien befinden sich im Ordner [Learning Modules](https://github.com/Microsoft/WingtipSaaS/tree/master/Learning%20Modules) (Lernmodule). Laden Sie den Ordner **Learning Modules** auf den lokalen Computer herunter, wobei Sie dessen Ordnerstruktur beibehalten.

## <a name="provision-additional-tenants"></a>Bereitstellen zusätzlicher Mandanten

Pools können zwar bereits mit nur zwei S3-Datenbanken kostengünstig sein, je mehr Datenbanken der Pool aber enthält, desto günstiger fällt der Durchschnittskosteneffekt aus. Um die bedarfsgerechte Funktionsweise von Leistungsüberwachung und -verwaltung besser zu verstehen, müssen Sie für dieses Tutorial über mindestens 20 bereitgestellt Datenbanken verfügen.

Wenn Sie in einem vorherigen Tutorial bereits einen Batch von Mandanten bereitgestellt haben, können Sie mit dem Abschnitt [Simulieren der Nutzung auf allen Mandantendatenbanken](#simulate-usage-on-all-tenant-databases) fortfahren.

1. Öffnen Sie „…\\Learning Modules\\Provision and Catalog\\*Demo-PerformanceMonitoringAndManagement.ps1*“ in der **PowerShell-ISE**. Lassen Sie dieses Skript geöffnet, während Sie verschiedene Szenarien in diesem Tutorial ausführen.
1. Legen Sie **$DemoScenario** = **1**, **Bereitstellen eines Batches von Mandanten** fest.
1. Drücken Sie **F5** , um das Skript auszuführen.

Das Skript stellt in weniger als fünf Minuten 17 Mandanten bereit.

Das Skript *New-TenantBatch* verwendet einen geschachtelten oder verknüpften Satz von [Resource Manager](../azure-resource-manager/index.md)-Vorlagen, die einen Batch von Mandanten erstellen, der standardmäßig die Datenbank **baseTenantDb** auf den Katalogserver kopiert, um die neuen Mandantendatenbanken zu erstellen, diese dann im Katalog registriert und sie schließlich mit dem Mandantennamen und dem Veranstaltungsorttyp initialisiert. Dies ist konsistent mit der Bereitstellung eines neuen Mandanten durch die WTP-App. Alle an *baseTenantDB* vorgenommenen Änderungen werden auf alle danach bereitgestellten neuen Mandanten angewendet. Im [Tutorial zur Schemaverwaltung](sql-database-saas-tutorial-schema-management.md) erfahren Sie, wie Sie Schemaänderungen an *vorhandenen* Mandantendatenbanken ausführen (einschließlich der *goldenen* Datenbank).

## <a name="simulate-different-usage-patterns-by-generating-different-load-types"></a>Simulieren verschiedener Verwendungsmuster durch Generieren anderer Lasttypen

Das Skript *PerformanceMonitoringAndManagement.ps1* startet den Lastengenerator mit einem der verfügbaren *Lasttypen*:

| Demo | Szenario |
|:--|:--|
| 2 | Generieren einer Last mit normaler Intensität (ca. 40 DTUs) |
| 3 | Generieren einer Last mit längeren und häufigeren Spitzen pro Datenbank|
| 4 | Generieren einer Last mit höheren DTU-Spitzen pro Datenbank (ca. 80 DTUs)|
| 5 | Generieren einer normalen Last plus einer hohen Last auf einem einzelnen Mandanten (ca. 95 DTUs)|
| 6 | Generieren unausgeglichener Lasten über mehrere Pools|

## <a name="simulate-usage-on-all-tenant-databases"></a>Simulieren von Verbrauch auf allen Mandantendatenbanken

Der Lastengenerator wendet eine *synthetische* reine CPU-Last auf jede Mandantendatenbank an. Der Generator startet einen Einzelvorgang für jede Mandantendatenbank, der in regelmäßigen Abständen eine gespeicherte Prozedur aufruft, die die Last generiert. Der Auslastungsgrad (in eDTUs), die Dauer und die Intervalle variieren in allen Datenbanken, womit eine unvorhersehbare Mandantenaktivität simuliert wird.

1. Legen Sie **$DemoScenario** = **2**, *Generieren einer Last mit normaler Intensität* fest.
1. Drücken Sie **F5**, um Lasten auf all Ihre Mandantendatenbanken anzuwenden.

Da die Lasten sporadischer Natur sind, sollten Sie den Generator für 10 bis 20 Minuten ausführen, damit die Aktivität einen stabilen Zustand erreichen und ein einheitliches Muster ausbilden kann.

> [!IMPORTANT]
> Der Lastengenerator wird als eine Reihe von Einzelvorgängen in der lokalen PowerShell-Sitzung ausgeführt. Lassen Sie die Registerkarte *Demo-PerformanceMonitoringAndManagement.ps1* geöffnet. Wenn Sie die Registerkarte schließen oder Ihren Computer anhalten, wird der Lastengenerator beendet.

## <a name="monitor-resource-usage-using-the-portal"></a>Überwachen der Ressourcenverwendung über das Portal

Zum Überwachen der aus der angewendeten Last resultierenden Ressourcenverwendung öffnen Sie das Portal mit dem Pool, der die Mandantendatenbanken enthält.

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com), und browsen Sie zum Server „tenants1-&lt;BENUTZER&gt;“.
1. Es sollte die Liste der Mandantendatenbanken angezeigt werden, einschließlich des neuen Batches von Datenbanken.
1. Führen Sie einen Bildlauf nach unten durch, suchen Sie Pools für elastische Datenbanken, und klicken Sie auf **Pool1**. Dieser Pool enthält alle bisher erstellten Mandantendatenbanken.
1. Erweitern Sie das geöffnete Poolblatt, und schauen Sie sich die Auslastungsdiagramme für den Pool und die Topdatenbank an.

Die Poolauslastung ist im Wesentlichen das Aggregat der Datenbankauslastung für alle Datenbanken im Pool. Das Datenbankauslastungsdiagramm zeigt die fünf meist verwendeten Datenbanken an:

![](./media/sql-database-saas-tutorial-performance-monitoring/pool1.png)

Da neben den Top 5-Datenbanken weitere Datenbanken im Pool vorhanden sind, zeigt die Poolauslastung Aktivität, die sich im Diagramm der Top 5-Datenbanken nicht widergespiegelt. Um zusätzliche Details anzuzeigen, klicken Sie auf **Datenbankressourcenverwendung**:

![](./media/sql-database-saas-tutorial-performance-monitoring/database-utilization.png)


## <a name="set-performance-alerts-on-the-pool"></a>Festlegen von Leistungsbenachrichtigungen für den Pool

Gehen Sie folgendermaßen vor, um eine Benachrichtigungen für den Pool festzulegen, die ausgelöst wird, wenn durchgängig über fünf Minuten eine Auslastung von \>75 % vorliegt:

1. Öffnen Sie *Pool1* (auf dem Server *tenants1-\<Benutzer\>*) im [Azure-Portal](https://portal.azure.com).
1. Klicken Sie auf **Benachrichtigungsregeln** und dann auf **+ Benachrichtigung hinzufügen**:

   ![Benachrichtigung hinzufügen](media/sql-database-saas-tutorial-performance-monitoring/add-alert.png)

1. Geben Sie einen Namen an, z.B. **Hohe DTU**, 
1. Legen Sie die folgenden Werte fest:
   * **Metric = eDTU-Prozentanteil**
   * **Condition = größer als**.
   * **Threshold = 75**.
   * **Period = Innerhalb der letzten 30 Minuten**.

   ![Warnung festlegen](media/sql-database-saas-tutorial-performance-monitoring/alert-rule.png)

Sie können Benachrichtigungen an die E-Mail-Adresse Ihres Azure-Kontos und optional an weitere E-Mail-Adressen senden lassen (Sie sollten diese Möglichkeit nur nutzen, wenn Sie gleichzeitig Eigentümer des verwendeten Abonnements sind).

> [!NOTE]
> Da die Benachrichtigung nur ausgelöst wird, wenn der Schwellenwert für die letzten 30 Minuten überschritten wird, muss der Lastengenerator länger als 30 Minuten ausgeführt werden, um die Benachrichtigung zu testen.


## <a name="scale-up-a-busy-pool"></a>Zentrales Hochskalieren eines ausgelasteten Pools

Wenn der aggregierte Auslastungsgrad für einen Pool sich bis zu dem Punkt erhöht, an dem das Limit des Pools und sowie eine eDTU-Nutzung von 100 % erreicht ist, wird die Leistung einzelner Datenbanken beeinträchtigt, und die Antwortzeiten für Abfragen verlangsamen sich möglicherweise für alle Datenbanken im Pool.

Kurzfristig sollten Sie in Betracht ziehen, den Pool zentral hochzuskalieren, um zusätzliche Ressourcen bereitzustellen, oder Datenbanken aus dem Pool zu entfernen (indem Sie sie in andere Pools oder aus dem Pool auf eine eigenständige Dienstebene verschieben).

Längerfristig sollten Sie Abfragen oder die Indexverwendung optimieren, um die Leistung der Datenbank zu verbessern. Abhängig von der Empfindlichkeit der Anwendung gegenüber Leistungsproblemen sieht die bewährte Methode vor, einen Pool zentral hochzuskalieren, bevor er eine eDTU-Nutzung von 100 % erreicht. Lassen Sie sich im Voraus eine Benachrichtigung zusenden.

Sie können einen ausgelasteten Pool simulieren, indem Sie die vom Generator erzeugte Last erhöhen. Indem Sie höhere und länger andauernde Lastspitzen auf Datenbanken generieren, erhöht sich die aggregierte Auslastung des Pools, ohne dass die Anforderungen der einzelnen Datenbanken sich ändern. Im Portal oder über PowerShell können Sie den Pool problemlos zentral hochskalieren. In dieser Übung wird das Portal verwendet.

1. Legen Sie *$DemoScenario* = **3**, _Generieren einer Last mit längeren und häufigeren Spitzen pro Datenbank_ fest, um die Intensität der aggregierten Auslastung des Pools zu erhöhen, ohne die für jede Datenbank erforderliche Spitzenlast zu ändern.
1. Drücken Sie **F5**, um Lasten auf all Ihre Mandantendatenbanken anzuwenden.


1. **Öffnen Sie das Poolblatt für** **tenants1/Pool1**.


1. Überwachen Sie die erhöhte Pool-DTU-Nutzung im oberen Diagramm an. Es dauert einige Minuten, bis die neue, höhere Last zum Tragen kommt, aber Sie sollten schnell erkennen können, dass der Pool eine Auslastung von 100 % erreicht, dass sich die Auslastung in einem neuen Muster stabilisiert und dass der Pool schon bald überlastet ist.


1. Um den Pool zentral hochzuskalieren, klicken Sie auf **Pool konfigurieren**.
1. Passen Sie den Schieberegler **Pool-eDTU** auf 100 an (Sie sollten nicht höher gehen, um die Kosten zu beschränken). Beachten Sie, wie der verfügbare aggregierte Speicher für alle Datenbanken im Pool, angegeben als **Pool GB**, mit der eDTU-Einstellung verknüpft ist und ebenfalls erhöht wird. Das Ändern der Pool-eDTUs ändert die Einstellungen für die einzelnen Datenbanken nicht (die Obergrenze liegt weiterhin bei 50 eDTUs pro Datenbank). Die Einstellungen für die einzelnen Datenbanken werden auf der rechten Seite des Blatts **Pool konfigurieren** angezeigt.
1. Klicken Sie auf **Speichern**, um die Anforderung zu senden. Die Änderung wird für einen Standardpool in der Regel nach 3 bis 5 Minuten wirksam.

Wechseln Sie zurück zu **Pool1** > **Übersicht**, um die Überwachungsdiagramme anzuzeigen. Beachten Sie, welche Auswirkungen die Bereitstellung weiterer Ressourcen für den Pool hat (bei wenigen Datenbanken und einer zufälligen Last ist es allerdings nicht immer eindeutig zu erkennen). Berücksichtigen Sie beim Betrachten der Diagramme, dass 100 % im oberen Diagramm nun 100 eDTUs entspricht, während 100 % im unteren Diagramm weiterhin für 50 eDTUs steht, da die Obergrenze pro Datenbank weiterhin bei 50 eDTUs liegt.

Datenbanken bleiben online und sind während des gesamten Vorgangs vollständig verfügbar. Im letzten Moment, wenn alle Datenbanken zur Aktivierung mit der neuen Pool-eDTU bereit sind, werden alle aktiven Verbindungen unterbrochen. Anwendungscode sollte stets so verfasst sein, dass versucht wird, unterbrochene Verbindungen wiederherzustellen. Daher wird die Verbindung zur Datenbank im zentral hochskalierten Pool wieder hergestellt.

## <a name="create-a-second-pool-and-load-balance-databases-to-handle-increased-aggregate-load"></a>Erstellen eines zweiten Pools und Durchführen eines Lastenausgleichs der Datenbanken, um die ansteigende aggregierte Last zu bewältigen

Als Alternative zum zentralen Hochskalieren des Pools können Sie einen zweiten Pool erstellen und Datenbanken zum Lastenausgleich zwischen den zwei Pools in diesen Pool verschieben. Zu diesem Zweck muss der neue Pool auf dem gleichen Server wie der erste erstellt werden.

1. Öffnen Sie das **Serverblatt für den Server „customers1-&lt;BENUTZER&gt;“**. Wenn Sie sich auf einem Datenbank- oder Poolblatt befinden, können Sie die Dropdownliste des Steuerelements „Zusammenfassung“ öffnen und den Servernamen als Verknüpfung auswählen.
1. Klicken Sie auf **+ Neuer Pool**, um einen Pool auf dem aktuellen Server zu erstellen.
1. Führen Sie in der Vorlage für Pools für elastische Datenbanken folgende Schritte aus:

    1. Legen Sie **Name = Pool2** fest.
    1. Behalten Sie als Tarif **Standardpool** bei.
    1. Klicken auf **Pool konfigurieren**.
    1. Legen Sie auf dem Blatt „Pool konfigurieren“, das geöffnet wird, **Pool eDTU = 50 DTU** fest.
    1. Klicken Sie auf den Befehl **Datenbanken hinzufügen**, um eine Liste der Datenbanken auf diesem Server anzuzeigen, die nicht im aktuellen Pool sind.
    1. **Aktivieren** Sie in der Liste die Hälfte der Datenbanken (10 von 20), und verschieben Sie diese in den neuen Pool. Klicken Sie dann auf **Auswählen**.
    1. Klicken Sie erneut auf **Auswählen**, um die Konfigurationsänderungen zu übernehmen. Beachten Sie die Kostenschätzung für einen Monat der Nutzung mit den ausgewählten Optionen.
    1. Wählen Sie **OK** aus, um den neuen Pool mit der neuen Konfiguration zu erstellen und die Datenbanken zu verschieben.

Das Erstellen des Pools und das Verschieben der Datenbanken in den Pool dauert ein paar Minuten. Alle Datenbanken, die verschoben werden, bleiben bis zum letzten Moment online und vollständig verfügbar, Zeitpunkt. Dann erst werden alle geöffneten Verbindungen geschlossen. Wenn ein Client die Verbindung erneut herzustellen versucht, wird die Verbindung mit der Datenbank im neuen Pool hergestellt.

Nachdem der Pool erstellt wurde, wird er auf dem Serverblatt „customers1“ angezeigt. Klicken Sie auf den Poolnamen, um das Poolblatt zu öffnen und die Leistung zu überwachen.

Es sollte angezeigt werden, dass die Ressourcenverwendung in Pool1 gesunken ist und dass in Pool2 eine ähnliche Auslastung vorliegt.

## <a name="manage-an-increased-load-on-a-single-database"></a>Verwalten einer erhöhten Auslastung in einer Einzeldatenbank

Wenn in einer Einzeldatenbank in einem Pool eine anhaltend hohe Auslastung vorliegt, kann dies, je nach der Konfiguration des Ressourcenpools, die Ressourcen im Pool dominieren und Auswirkungen auf andere Datenbanken haben. Wenn die Aktivität voraussichtlich für einige Zeit anhalten wird, kann die Datenbank vorübergehend aus dem Pool verschoben werden. Auf diese Weise können der Datenbank zum einen mehr Ressourcen als anderen Datenbanken im Pool zugewiesen werden, zum anderen wird sie von den anderen Datenbanken isoliert. In dieser Übung werden die Auswirkungen simuliert, wenn der Ticketverkauf für ein beliebtes Konzert in der Contoso Concert Hall zu einer hohen Auslastung führt.

1. Im Skript „…\\**Demo-PerformanceManagementAndMonitoring**.ps1“
1. legen Sie **$DemoScenario = 5, Generieren einer normalen Last plus einer hohen Last auf einem einzelnen Mandanten (ca. 95 DTUs)** fest.
1. Legen Sie **$SingleTenantDatabaseName = contosoconcerthall** fest.
1. Führen Sie das Skript mit **F5** aus.
1. **Öffnen Sie das Poolblatt** **für Customers1/Pool1**.
1. Schauen Sie sich in der Anzeige **Überwachung des Pools für elastische Datenbanken** oben auf dem Blatt die erhöhte Pool-DTU-Nutzung an. Nach ein oder zwei Minuten sollte die höhere Auslastung sichtbar werden, und Sie sollten schnell erkennen, dass der Pool eine Auslastung von 100 % erreicht.
1. In der Anzeige **Überwachung der elastischen Datenbank** werden außerdem die meist verwendeten Datenbanken der letzten Stunde angezeigt. Die Datenbank „contosoconcerthall“ sollte schnell unter den 5 meist verwendeten Datenbanken angezeigt werden.
1. **Klicken Sie auf das Diagramm „Überwachung der elastischen Datenbank“**,**** und ein Blatt **Datenbank-Ressourcennutzung** wird geöffnet, mit dem Sie die Datenbanken einzeln auswählen und überwachen können. Dadurch können Sie die Anzeige für die Datenbank „contosoconcerthall“ isolieren.
1. **Klicken Sie in der Liste der Datenbanken auf contosoconcerthall**, um das entsprechende Datenbankblatt zu öffnen.
1. Klicken Sie auf **Tarif (DTUs skalieren)** im Kontextmenü, um das Blatt **Leistung konfigurieren** zu öffnen, auf dem Sie eine isolierte Leistungsstufe für die Datenbank festlegen können.
1. Klicken Sie auf die Registerkarte **Standard**, um die Skalierungsoptionen des Standard-Tarifs zu öffnen.
1. Schieben Sie den **DTU-Schieberegler** nach rechts, um 100 DTUs auszuwählen. Beachten Sie, dass dies dem in Klammern zwischen den Größenmesswerten „DTU“ und „Speicher“ angezeigten Dienstziel, **S3,** entspricht.
1. Klicken Sie auf **Übernehmen**, um die Datenbank aus dem Pool zu verschieben und zu einer Standard S3-Datenbank zu machen.
1. Sobald die Bereitstellung abgeschlossen ist, überwachen Sie auf dem Blatt „Pool für elastische Datenbanken“ und dem Datenbank-Blatt die Auswirkung auf die Datenbank „contosoconcerthall“ und den Pool, aus dem Sie entfernt wurde.

Sobald die erhöhte Auslastung der Datenbank „contosoconcerthall“ nachlässt, sollten Sie die Datenbank sofort wieder zum Pool hinzufügen, um die Kosten zu senken. Wenn unklar ist, wann dieser Zeitpunkt gekommen ist, können Sie eine Benachrichtigungen für die Datenbank festlegen, die ausgelöst wird, wenn ihre DTU-Nutzung unter den Höchstwert pro Datenbank für den Pool sinkt. Das Verschieben einer Datenbank in einen Pool wird im Schritt 5 beschrieben.

## <a name="other-performance-management-patterns"></a>Andere Leistungsverwaltungsmuster

**Präemptive Skalierung** In Übung 6, in der Sie erfahren haben, wie Sie eine isolierte Datenbank skalieren, wussten Sie, nach welcher Datenbank Sie suchen mussten. Wenn WTP von der Verwaltung der Contoso Concert Hall über den bevorstehenden Ticketverkauf informiert worden wäre, hätte die Datenbank vorsorglich aus dem Pool verschoben werden können. Andernfalls wäre es wahrscheinlich erforderlich gewesen, eine Benachrichtigung für den Pool oder die Datenbank auszugeben, um die Situation zu erkennen. Sehr ungünstig wäre es, wenn Sie erst davon erfahren hätten, weil sich die anderen Mandanten im Pool über die Beeinträchtigung der Leistung beschwert hätten. Wenn der Mandant einschätzen kann, wie lange zusätzliche Ressourcen benötigt werden, können Sie ein Azure Automation-Runbook einrichten, um die Datenbank nach einem definierten Zeitplan aus dem Pool in einen anderen Pool und dann wieder zurück zu verschieben.

**Self-Service-Skalierung des Mandanten** Da die Skalierung eine Aufgabe ist, die problemlos über das Verwaltungs-API aufgerufen werden kann, können Sie die Fähigkeit, Mandantendatenbanken zu skalieren, ganz leicht in Ihre mandantenseitige Anwendung integrieren und als Feature Ihres SaaS-Diensts anbieten. Lassen Sie Mandanten beispielsweise das zentrale Hoch- oder Herunterskalieren selbst verwalten, vielleicht direkt verknüpft mit der Abrechnung.

### <a name="scaling-a-pool-up-and-down-on-a-schedule-to-match-usage-patterns"></a>Zentrales Hoch- oder Herunterskalieren eines Pools nach einem Zeitplan entsprechend dem Verwendungsmuster

Wenn die aggregierte Mandantennutzung vorhersagbaren Verwendungsmustern folgt, können Sie einen Pool mit Azure Automation entsprechend einem Zeitplan zentral hoch- oder herunterskalieren. Skalieren Sie einen Pool beispielsweise an Wochentagen ab 18.00 Uhr zentral herunter und ab 6.00 Uhr wieder hoch, wenn Sie wissen, dass die Ressourcenanforderungen zu diesen Zeiten sinken.



## <a name="next-steps"></a>Nächste Schritte

In diesem Tutorial lernen Sie Folgendes:

> [!div class="checklist"]
> * Simulieren der Verwendung der Mandantendatenbanken durch Ausführen eines bereitgestellten Lastengenerators
> * Überwachen der Mandantendatenbanken während ihrer Reaktion auf einen Lastenanstieg
> * Zentrales Hochskalieren von Pools für elastische Datenbanken als Reaktion auf die erhöhte Datenbankauslastung
> * Bereitstellen eines zweiten Pools für elastische Datenbanken, um einen Lastenausgleich der Datenbankaktivität vorzunehmen

[Tutorial zum Wiederherstellen eines einzelnen Mandanten](sql-database-saas-tutorial-restore-single-tenant.md)


## <a name="additional-resources"></a>Zusätzliche Ressourcen

* [Weitere Tutorials, die auf der ersten Wingtip Tickets Platform-Anwendungsbereitstellung (WTP) aufbauen](sql-database-wtp-overview.md#sql-database-wtp-saas-tutorials)
* [Pools für elastische SQL-Datenbanken](sql-database-elastic-pool.md)
* [Azure Automation](../automation/automation-intro.md)
* [Log Analytics](sql-database-saas-tutorial-log-analytics.md): Tutorial zum Einrichten und Verwenden von Log Analytics

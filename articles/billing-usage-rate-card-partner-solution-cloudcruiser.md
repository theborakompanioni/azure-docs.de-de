---
title: Integration von Cloud Cruiser und der Microsoft Azure-Abrechnungs-API | Microsoft-Dokumentation
description: "Enthält eine einzigartige Sicht auf den Microsoft Azure-Abrechnungspartner Cloud Cruiser und dessen Umgebungen mit Integration der Azure-Abrechnungs-APIs in das Produkt.  Dies ist besonders hilfreich für Azure- und Cloud Cruiser-Kunden, die an der Verwendung bzw. am Testen von Cloud Cruiser für Microsoft Azure Pack interessiert sind."
services: 
documentationcenter: 
author: BryanLa
manager: mbaldwin
editor: 
tags: billing
ms.assetid: b65128cf-5d4d-4cbd-b81e-d3dceab44271
ms.service: billing
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: billing
ms.date: 09/08/2016
ms.author: mobandyo;sirishap;bryanla
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: b1783823218a883fc9fdec05e835fb7249eed97d


---
# <a name="cloud-cruiser-and-microsoft-azure-billing-api-integration"></a>Integration von Cloud Cruiser und Microsoft Azure-Abrechnungs-API
In diesem Artikel wird beschrieben, wie die mit den neuen Microsoft Azure-Abrechnungs-APIs gesammelten Informationen in Cloud Cruiser für die Simulation und Analyse von Workflowkosten verwendet werden können.

## <a name="azure-ratecard-api"></a>Azure-RateCard-API
Mit der RateCard-API werden Gebühren- und Preisinformationen aus Azure bereitgestellt. Nachdem Sie sich mit den richtigen Anmeldeinformationen authentifiziert haben, können Sie die API abfragen, um Metadaten zu den unter Azure verfügbaren Diensten sowie die Gebühren zu erfassen, die Ihrer Angebots-ID zugeordnet sind.

Im Folgenden finden Sie ein Beispiel für eine Antwort der API mit den Preisen für die Instanz „A0 (Windows)“:

    {
        "MeterId": "0e59ad56-03e5-4c3d-90d4-6670874d7e29",
        "MeterName": "Compute Hours",
        "MeterCategory": "Virtual Machines",
        "MeterSubCategory": "A0 VM (Windows)",
        "Unit": "Hours",
        "MeterRates":
        {
            "0": 0.029
        },
        "EffectiveDate": "2014-08-01T00:00:00Z",
        "IncludedQuantity": 0.0,
        "MeterStatus": "Active"
    },

### <a name="cloud-cruisers-interface-to-azure-ratecard-api"></a>Schnittstelle von Cloud Cruiser für die Azure-RateCard-API
Cloud Cruiser kann die Informationen der RateCard-API auf unterschiedliche Weise nutzen. In diesem Artikel wird gezeigt, wie damit IaaS-Workload-Kostensimulationen und -Analysen durchgeführt werden können.

Um diesen Anwendungsfall zu demonstrieren, stellen wir uns eine Workload vor, bei der mehrere Instanzen unter Microsoft Azure Pack (WAP) ausgeführt werden. Das Ziel besteht darin, diese Workload unter Azure zu simulieren und die Kosten für eine Migration dieser Art zu schätzen. Zum Erstellen dieser Simulation müssen zwei Hauptaufgaben durchgeführt werden:

1. **Importieren und Verarbeiten der von der RateCard-API erfassten Dienstinformationen.**  Diese Aufgabe wird auch in den Arbeitsmappen durchgeführt, in denen die per RateCard-API extrahierten Daten transformiert und in einer neuen Gebührenübersicht veröffentlicht werden. Diese neue Gebührenübersicht wird in den Simulationen zum Schätzen der Azure-Preise verwendet.
2. **Normalisieren der WAP-Dienste und Azure-Dienste für IaaS.** Standardmäßig basieren WAP-Dienste auf einzelnen Ressourcen (CPU, Arbeitsspeichergröße, Datenträgergröße usw.), während Azure-Dienste auf der Instanzgröße basieren (A0, A1, A2 usw.). Diese erste Aufgabe kann vom ETL-Modul (als Arbeitsmappen bezeichnet) von Cloud Cruiser durchgeführt werden, wobei diese Ressourcen analog zu Azure-Instanzdiensten für Instanzgrößen gebündelt werden können.

### <a name="import-data-from-the-ratecard-api"></a>Importieren von Daten aus der RateCard-API
Cloud Cruiser-Arbeitsmappen sind eine Möglichkeit, Informationen aus der RateCard-API auf automatisierte Weise zu erfassen und zu verarbeiten.  Mit ETL-Arbeitsmappen (Extrahieren-Transformieren-Laden) können Sie die Sammlung, Transformation und Veröffentlichung von Daten in der Cloud Cruiser-Datenbank konfigurieren.

Jede Arbeitsmappe kann über mehrere Sammlungen verfügen, sodass Sie Informationen aus unterschiedlichen Quellen korrelieren können, um die Nutzungsdaten zu ergänzen oder zu erweitern. Die beiden folgenden Screenshots zeigen, wie eine neue *Sammlung* in einer vorhandenen Arbeitsmappe erstellt wird und Informationen aus der RateCard-API in die *Sammlung* importiert werden:

![Abbildung 1: Erstellen einer neuen Sammlung][1]

![Abbildung 2: Importieren von Daten aus der neuen Sammlung][2]

Nach dem Importieren der Daten in die Arbeitsmappe können Sie mehrere Schritte und Transformationsprozesse erstellen, um die Daten zu ändern und zu modellieren. Da wir nur an Infrastructure-as-a-Service (IaaS) interessiert sind, können wir in diesem Beispiel die Transformationsschritte zum Entfernen von nicht benötigten Zeilen bzw. Datensätzen verwenden, die sich auf andere Dienste als IaaS beziehen.

Im folgenden Screenshot sind die Transformationsschritte zu sehen, die zum Verarbeiten von mit der RateCard-API erfassten Daten verwendet werden:

![Abbildung 3: Transformationsschritte zum Verarbeiten von gesammelten Daten aus der RateCard-API][3]

### <a name="defining-new-services-and-rate-plans"></a>Definieren neuer Dienste und Gebührenübersichten
Es gibt verschiedene Möglichkeiten, in Cloud Cruiser Dienste zu definieren. Eine der Optionen ist das Importieren der Dienste aus den Nutzungsdaten. Diese Methode wird häufig verwendet, wenn mit Public Clouds gearbeitet wird, bei denen die Dienste bereits vom Anbieter definiert wurden.

Eine Gebührenübersicht umfasst eine Reihe von Gebühren oder Preisen, die z. B. auf unterschiedliche Dienste (basierend auf Gültigkeitsdaten) oder Kundengruppen angewendet werden können. Außerdem können Gebührenübersichten in Cloud Cruiser zum Erstellen von Simulationen oder Was-wäre-wenn-Szenarien verwendet werden, um zu ermitteln, wie sich Änderungen von Diensten auf die Gesamtkosten einer Workload auswirken können.

In diesem Beispiel verwenden wir die Dienstinformationen der RateCard-API, um in Cloud Cruiser neue Dienste zu definieren. Ebenso können wir die Gebühren, die den Diensten zugeordnet sind, zum Erstellen einer neuen Gebührenübersicht in Cloud Cruiser verwenden.

Am Ende des Transformationsvorgangs ist es möglich, einen neuen Schritt zu erstellen und die Daten der RateCard-API als neue Dienste und Gebühren zu veröffentlichen.

![Abbildung 4: Veröffentlichen der Daten aus der RateCard-API als neue Dienste und Gebühren][4]

### <a name="verify-azure-services-and-rates"></a>Überprüfen von Azure-Diensten und -Gebühren
Nachdem Sie die Dienste und Gebühren veröffentlicht haben, können Sie die Liste mit den importierten Diensten in Cloud Cruiser auf der Registerkarte *Dienste* überprüfen:

![Abbildung 5: Überprüfen der neuen Dienste][5]

Auf der Registerkarte *Gebührenübersichten* können Sie die neue Gebührenübersicht mit dem Namen „AzureSimulation“ mit den aus der RateCard-API importierten Gebühren überprüfen.

![Abbildung 6: Überprüfen der neuen Gebührenübersicht und der zugeordneten Gebühren][6]

### <a name="normalize-wap-and-azure-services"></a>Normalisieren von WAP- und Azure-Diensten
Standardmäßig stelle WAP Nutzungsinformationen basierend auf der Verwendung der Compute-, Arbeitsspeicher- und Netzwerkressourcen bereit. In Cloud Cruiser können Sie Ihre Dienste direkt basierend auf der Zuweisung oder der gemessenen Nutzung dieser Ressourcen definieren. Beispielsweise können Sie eine Grundgebühr für jede Stunde der CPU-Nutzung festlegen oder die GB-Menge des Arbeitsspeichers berechnen, der einer Instanz zugeordnet ist.

In diesem Beispiel müssen wir zum Vergleichen der Kosten für WAP und Azure die Ressourcennutzung unter WAP in Bündeln zusammenfassen, die dann Azure-Diensten zugeordnet werden können. Die Implementierung dieser Transformation in die Arbeitsmappen ist einfach:

![Abbildung 7: Transformieren von WAP-Daten zum Normalisieren von Diensten][7]

Der letzte Schritt für die Arbeitsmappe ist die Veröffentlichung der Daten in der Cloud Cruiser-Datenbank. Während dieses Schritts werden die Nutzungsdaten jetzt zu Diensten zusammengefasst (mit Zuordnung zu den Azure-Diensten) und mit Standardgebühren verknüpft, um die Gebühren zu erstellen.

Nach Fertigstellung der Arbeitsmappe können Sie die Verarbeitung der Daten automatisieren, indem Sie im Scheduler einen Task hinzufügen und die Häufigkeit und Uhrzeit für die Ausführung der Arbeitsmappe angeben.

![Abbildung 8: Planen von Arbeitsmappen][8]

### <a name="create-reports-for-workload-cost-simulation-analysis"></a>Erstellen von Berichten für die Workload-Kostensimulationsanalyse
Nachdem die Nutzung erfasst und die Gebühren in die Cloud Cruiser-Datenbank geladen wurden, können wir das Cloud Cruiser Insights-Modul nutzen, um die gewünschte Simulation der Workloadkosten zu erstellen.

Zum Demonstrieren dieses Szenarios haben wir den folgenden Bericht erstellt:

![Kostenvergleich][9]

Das obere Diagramm zeigt einen Kostenvergleich, aufgeschlüsselt nach Diensten. Darin werden die Preise verglichen, die bei WAP (dunkelblau) und Azure (hellblau) für die Ausführung der Workload für jeden spezifischen Dienst berechnet werden.

Das untere Diagramm zeigt die gleichen Daten, diesmal aber aufgeteilt nach Abteilung. Das Diagramm zeigt die Kosten, die für jede Abteilung für die Ausführung ihrer Workload in WAP und Azure anfallen, sowie die Unterschiede im Balken „Einsparungen“ (grün).

## <a name="azure-usage-api"></a>Azure-Nutzungs-API
### <a name="introduction"></a>Einführung
Microsoft hat vor Kurzem die Azure-Nutzungs-API vorgestellt, die Abonnenten den programmgesteuerten Abruf von Nutzungsdaten ermöglicht, um sich einen Einblick in ihre Ressourcennutzung zu verschaffen. Dies ist eine gute Nachricht für Cloud Cruiser-Kunden, die dank dieser API einen umfassenderen Datenbestand nutzen können.

Cloud Cruiser kann von der Integration mit der Nutzungs-API auf verschiedene Weise profitieren. Die über die API verfügbare Granularität (stündliche Verwendungsinformationen) und Informationen zu Ressourcenmetadaten liefern die benötigten Daten zur Unterstützung flexibler Showback- und Chargeback-Modelle. 

In diesem Tutorial zeigen wir ein Beispiel, wie Cloud Cruiser von den Informationen profitieren kann, die die Nutzungs-API liefert. Genauer gesagt erstellen wir eine Ressourcengruppe in Azure, ordnen Tags für die Kontostruktur zu und beschreiben den Prozess zum Abrufen und Verarbeiten von Taginformationen in Cloud Cruiser.

Das Ziel ist letztlich die Erstellung von Berichten wie dem folgenden und die Analyse von Kosten und Nutzung basierend auf der von den Tags aufgefüllten Kontostruktur.

![Abbildung 10: Bericht mit Aufschlüsselungen mithilfe von Tags][10]

### <a name="microsoft-azure-tags"></a>Microsoft Azure-Tags
Die Daten, die über die API der Azure-Nutzung verfügbar sind, enthalten nicht nur Informationen zur Nutzung, sondern auch Ressourcenmetadaten einschließlich beliebiger zugeordneter Tags. Tags bieten eine einfache Möglichkeit zur Organisation Ihrer Ressourcen, aber um effektiv zu sein, müssen Sie Folgendes sicherstellen:

* Tags werden den Ressourcen zum Zeitpunkt der Bereitstellung ordnungsgemäß zugeordnet.
* Tags werden im Showback-/Chargeback-Prozess verwendet, um die Nutzung mit der Kontostruktur der Organisation zu verknüpfen.

Diese beiden Anforderungen können eine Herausforderung darstellen, und zwar insbesondere dann, wenn es auf Bereitstellungs- und Abrechnungsseite manuelle Prozesse gibt. Falsch eingegebene, fehlerhafte oder sogar fehlende Tags sind häufig ein Grund für Beschwerden von Kunden, wenn Tags zum Einsatz kommen, und diese Fehler können die Abläufe auf Abrechnungsseite sehr erschweren.

Mithilfe der neuen Azure-Nutzungs-API kann Cloud Cruiser Informationen in Ressourcentags abrufen und über ein ausgeklügeltes ETL-Tool – so genannten Arbeitsmappen – diese gängigen Taggingfehler korrigieren. Über Transformationsschritte, in denen reguläre Ausdrücke und die Korrelation von Daten zum Einsatz kommen, kann Cloud Cruiser Ressourcen mit falschen Tags bestimmen und die richtigen Tags zuordnen. Dadurch wird die ordnungsgemäße Zuordnung der Ressourcen zum Nutzer sichergestellt.

Auf Abrechnungsseite automatisiert Cloud Cruiser den Showback-/Chargeback-Prozess und kann die Informationen in Tags verwenden, um die Nutzung dem entsprechenden Nutzer (Abteilung, Geschäftsbereich, Projekt usw.) zuzuweisen. Diese Automatisierung bietet eine enorme Verbesserung und kann einen konsistenten und überwachbaren Abrechnungsprozess sicherstellen.

### <a name="creating-a-resource-group-with-tags-on-microsoft-azure"></a>Erstellen einer Ressourcengruppe mit Tags in Microsoft Azure
Der erste Schritt in diesem Tutorial besteht darin, zunächst im Azure-Portal eine Ressourcengruppe zu erstellen und anschließend neue Tags zu erstellen, die den Ressourcen zugeordnet werden. In diesem Beispiel erstellen wir die folgenden Tags: Department, Environment, Owner, Project (Abteilung, Umgebung, Besitzer, Projekt).

Der folgende Screenshot zeigt das Beispiel einer Ressourcengruppe mit den zugehörigen Tags.

![Abbildung 11: Ressourcengruppe mit zugeordneten Tags im Azure-Portal][11]

Der nächste Schritt ist das Abrufen der Informationen aus der Nutzungs-API in Cloud Cruiser. Die Nutzungs-API bietet derzeit Antworten im JSON-Format. Hier ist ein Beispiel der abgerufenen Daten:

    {
      "id": "/subscriptions/bb678b04-0e48-4b44-XXXX-XXXXXXX/providers/Microsoft.Commerce/UsageAggregates/Daily_BRSDT_20150623_0000",
      "name": "Daily_BRSDT_20150623_0000",
      "type": "Microsoft.Commerce/UsageAggregate",
      "properties":
      {
        "subscriptionId": "bb678b04-0e48-4b44-XXXX-XXXXXXXXX",
        "usageStartTime": "2015-06-22T00:00:00+00:00",
        "usageEndTime": "2015-06-23T00:00:00+00:00",
        "meterName": "Compute Hours",
        "meterRegion": "",
        "meterCategory": "Virtual Machines",
        "meterSubCategory": "Standard_D1 VM (Non-Windows)",
        "unit": "Hours",
        "instanceData": "{\"Microsoft.Resources\":{\"resourceUri\":\"/subscriptions/bb678b04-0e48-4b44-XXXX-XXXXXXXX/resourceGroups/DEMOUSAGEAPI/providers/Microsoft.Compute/virtualMachines/MyDockerVM\",\"location\":\"eastus\",\"tags\":{\"Department\":\"Sales\",\"Project\":\"Demo Usage API\",\"Environment\":\"Test\",\"Owner\":\"RSE\"},\"additionalInfo\":{\"ImageType\":\"Canonical\",\"ServiceType\":\"Standard_D1\"}}}",
        "meterId": "e60caf26-9ba0-413d-a422-6141f58081d6",
        "infoFields": {},
        "quantity": 8

      },
    },


### <a name="import-data-from-the-usage-api-into-cloud-cruiser"></a>Importieren von Daten aus der Nutzungs-API in Cloud Cruiser
Cloud Cruiser-Arbeitsmappen sind eine Möglichkeit, Informationen aus der Nutzungs-API auf automatisierte Weise zu erfassen und zu verarbeiten. Mit einer ETL-Arbeitsmappe (Extrahieren-Transformieren-Laden) können Sie die Sammlung, Transformation und Veröffentlichung von Daten in der Cloud Cruiser-Datenbank konfigurieren.

Jede Arbeitsmappe kann über eine oder mehrere Sammlungen verfügen. So können Sie Informationen aus unterschiedlichen Quellen korrelieren, um die Nutzungsdaten zu erweitern oder zu verbessern. In diesem Beispiel erstellen wir ein neues Blatt in der Arbeitsmappe mit der Azure-Vorlage (*UsageAPI)* und legen eine neue *Sammlung* zum Importieren von Informationen aus der Nutzungs-API fest.

![Abbildung 3: In das Blatt "UsageAPI" importierte Nutzungs-API-Daten][12]

Beachten Sie, dass diese Arbeitsmappe bereits andere Blätter zum Importieren von Diensten aus Azure (*ImportServices*) sowie zum Verarbeiten der aus der Abrechnung-API stammenden Nutzungsdaten (*PublishData*) enthält.

Als Nächstes verwenden wir die Nutzungs-API, um das Blatt *UsageAPI* aufzufüllen, und korrelieren diese Informationen mit den Nutzungsdaten aus der Abrechnung-API auf dem Blatt *PublishData*.

### <a name="processing-the-tag-information-from-the-usage-api"></a>Verarbeiten die Tag-Informationen aus der Nutzungs-API
Nach dem Import der Daten in die Arbeitsmappe erstellen wir auf dem Blatt *UsageAPI* Transformationsschritte, um die von der API gelieferten Informationen zu verarbeiten. Im ersten Schritt verwenden wir einen Prozessor vom Typ „JSON Split“ zum Extrahieren der Tags aus einem einzelnen Feld und erstellen neue Felder für jedes einzelne Tag (Department, Project, Owner und Environment).

![Abbildung 4: Erstellen neuer Felder für die Tag-Informationen][13]

Beachten Sie, dass beim Dienst „Networking“ die Taginformationen (gelb umrahmtes Feld) fehlen. Wir können jedoch im Feld *ResourceGroupName* überprüfen, ob dieser Dienst Teil derselben Ressourcengruppe ist. Da wir über Tags für die anderen Ressourcen aus dieser Ressourcengruppe verfügen, können wir diese Informationen nutzen, um dieser Ressource im weiteren Verlauf des Prozesses die fehlenden Tags zuzuordnen.

Der nächste Schritt ist die Erstellung einer Nachschlagetabelle, die zum Zuordnen der Informationen in den Tags zu *ResourceGroupName*dient. Diese Nachschlagetabelle wird im nächsten Schritt verwendet, um die Nutzungsdaten mit Tag-Informationen zu versehen.

### <a name="adding-the-tag-information-to-the-consumption-data"></a>Hinzufügen der Tag-Informationen zu den Nutzungsdaten
Nun können wir zum Blatt *PublishData* wechseln, auf dem die Nutzungsinformationen aus der Abrechnungs-API verarbeitet werden, und die aus den Tags extrahierten Felder hinzufügen. Dieser Prozess erfolgt mithilfe der im vorherigen Schritt erstellten Nachschlagetabelle mit *ResourceGroupName* als Schlüssel für die Suchvorgänge.

![Abbildung 5: Auffüllen der Kontostruktur mit den Informationen aus den Suchvorgängen][14]

Wie Sie sehen, wurden die entsprechenden Kontostrukturfelder für den Dienst "Networking" zugeordnet, sodass der Fehler aufgrund fehlender Tags behoben wurde. Wir haben auch Kontostrukturfelder für Ressourcen außerhalb unserer Ressourcengruppe mit "Other" aufgefüllt, um sie in den Berichten unterscheiden zu können.

Nun müssen wir nur noch einen Schritt hinzufügen, um die Nutzungsdaten zu veröffentlichen. In diesem Schritt werden die entsprechenden Gebühren für die einzelnen Dienste, die in unserer Gebührenübersicht festgelegt sind, den Nutzungsinformationen zugeordnet. Die resultierende Abrechnung wird in die Datenbank geladen.

Das Beste ist, dass Sie diesen Prozess nur einmal durchlaufen müssen. Wenn die Arbeitsmappe vollständig ist, müssen Sie sie nur dem Scheduler hinzufügen, damit sie zur geplanten Uhrzeit stündlich oder täglich ausgeführt wird. Dann geht es nur noch darum, neue Berichte zu erstellen oder vorhandene anzupassen, um die Daten zu analysieren und daraus aussagekräftige Erkenntnisse aus Ihrer Cloudnutzung zu gewinnen.

### <a name="next-steps"></a>Nächste Schritte
* Eine ausführliche Anleitung zur Erstellung von Cloud Cruiser-Arbeitsmappen und -Berichten finden Sie in der Cloud Cruiser- [Onlinedokumentation](http://docs.cloudcruiser.com/) (gültige Anmeldung erforderlich).  Weitere Informationen zu Cloud Cruiser erhalten Sie, indem Sie sich an [info@cloudcruiser.com](mailto:info@cloudcruiser.com)dient.
* Eine Übersicht über die Azure-APIs zur Ressourcennutzung und Gebührenkarte (Resource Usage und RateCard) finden Sie unter [Gewinnen von Einblicken in den Ressourcenverbrauch unter Microsoft Azure](billing-usage-rate-card-overview.md) .
* Ausführlichere Informationen zu beiden APIs finden Sie unter [Azure-Abrechnungs-REST-API – Referenz](https://msdn.microsoft.com/library/azure/1ea5b323-54bb-423d-916f-190de96c6a3c). Sie sind Teil der APIs, die vom Azure Resource Manager bereitgestellt werden.
* Wenn Sie sich weiter mit Beispielcode beschäftigen möchten, sehen Sie sich die Codebeispiele für die Microsoft Azure-Abrechnungs-API unter [Azure-Codebeispiele](https://azure.microsoft.com/documentation/samples/?term=billing)an.

### <a name="learn-more"></a>Weitere Informationen
* Weitere Informationen zum Azure-Ressourcen-Manager finden Sie unter [Übersicht über den Azure-Ressourcen-Manager](azure-resource-manager/resource-group-overview.md) .

<!--Image references-->

[1]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Create-New-Workbook-Collection.png "Abbildung 1: Erstellen einer neuen Sammlung"
[2]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Import-Data-From-RateCard.png "Abbildung 2: Importieren von Daten aus der neuen Sammlung"
[3]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Transformation-Steps-Process-RateCard-Data.png "Abbildung 3: Transformationsschritte zum Verarbeiten von gesammelten Daten aus der RateCard-API"
[4]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Publish-RateCard-Data-New-Services-Rates.png "Abbildung 4: Veröffentlichen der Daten aus der RateCard-API als neue Dienste und Gebühren"
[5]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Verify-Azure-Services-And-Pricing1.png "Abbildung 5: Überprüfen der neuen Dienste"
[6]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Verify-Azure-Services-And-Pricing2.png "Abbildung 6: Überprüfen der neuen Gebührenübersicht und der zugeordneten Gebühren"
[7]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Transforming-WAP-Normalize-Services.png "Abbildung 7: Transformieren von WAP-Daten zum Normalisieren von Diensten"
[8]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Workbook-Scheduling.png "Abbildung 8: Planen von Arbeitsmappen"
[9]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/Workload-Cost-Simulation-Report.png "Abbildung 9: Beispielbericht für das Workload-Kostenvergleichsszenario"
[10]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/1_ReportWithTags.png "Abbildung 10: Bericht mit Aufschlüsselungen mithilfe von Tags"
[11]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/2_ResourceGroupsWithTags.png "Abbildung 11: Ressourcengruppe mit zugeordneten Tags im Azure-Portal"
[12]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/3_ImportIntoUsageAPISheet.png "Abbildung 12: In das Blatt „UsageAPI“ importierte Nutzungs-API-Daten"
[13]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/4_NewTagField.png "Abbildung 13: Erstellen neuer Felder für die Taginformationen"
[14]: ./media/billing-usage-rate-card-partner-solution-cloudcruiser/5_PopulateAccountStructure.png "Abbildung 14: Auffüllen der Kontostruktur mit den Informationen aus den Suchvorgängen"



<!--HONumber=Nov16_HO3-->



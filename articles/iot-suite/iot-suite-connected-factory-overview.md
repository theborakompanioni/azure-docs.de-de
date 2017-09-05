---
title: "Azure IoT Suite Connected Factory – Übersicht | Microsoft-Dokumentation"
description: "Enthält eine Beschreibung der vorkonfigurierten Connected Factory-Lösung der Azure IoT Suite."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 
ms.service: iot-suite
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/27/2017
ms.author: dobett
ms.translationtype: HT
ms.sourcegitcommit: 646886ad82d47162a62835e343fcaa7dadfaa311
ms.openlocfilehash: d502c8e2e2715899279f6ebcf7ed89c19a1bb9a6
ms.contentlocale: de-de
ms.lasthandoff: 08/24/2017

---
# <a name="get-started-with-the-connected-factory-preconfigured-solution"></a>Erste Schritte mit der vorkonfigurierten Connected Factory-Lösung

[Vorkonfigurierte Lösungen][lnk-preconfigured-solutions] von Azure IoT Suite kombinieren mehrere Azure IoT-Dienste, um durchgängige Lösungen bereitzustellen, die allgemeine IoT-Unternehmensszenarien implementieren. Mit der vorkonfigurierten *Connected Factory*-Lösung wird eine Verbindung mit Ihren Industrieanlagen hergestellt und die Überwachung dieser Geräte durchgeführt. Sie können die Lösung zum Analysieren des Datenstroms von Ihren Geräten und zum Steigern der Produktivität und Rentabilität während des Betriebs verwenden.

Dieses Tutorial zeigt, wie Sie die vorkonfigurierte Connected Factory-Lösung bereitstellen. Außerdem lernen Sie die grundlegenden Funktionen der vorkonfigurierten Lösung kennen. Sie können auf einen Großteil dieser Features über das *Lösungsdashboard* zugreifen, das zusammen mit der vorkonfigurierten Lösung bereitgestellt wird:

![Vorkonfigurierte Connected Factory-Lösung: Dashboard][img-cf-home]

Um dieses Tutorial abzuschließen, benötigen Sie ein aktives Azure-Abonnement.

> [!NOTE]
> Wenn Sie über kein Konto verfügen, können Sie in nur wenigen Minuten ein kostenloses Testkonto erstellen. Ausführliche Informationen finden Sie unter [Kostenlose Azure-Testversion][lnk_free_trial].
> 
> 

## <a name="provision-the-solution"></a>Bereitstellen der Lösung

1. Melden Sie sich mit den Anmeldeinformationen für Ihr Azure-Konto bei „azureiotsuite.com“ an, und klicken Sie auf „**+**“, um eine Lösung zu erstellen.
2. Klicken Sie auf der Kachel **Connected factory** (Connected Factory) auf **Auswählen**.
3. Geben Sie einen **Lösungsnamen** für Ihre vorkonfigurierte Connected Factory-Lösung ein.
4. Wählen Sie die gewünschten Angaben für **Abonnement** und **Region**, um die Lösung bereitzustellen.
5. Klicken Sie auf **Lösung erstellen** , um den Bereitstellungsprozess zu beginnen. Dies dauert in der Regel einige Minuten.

### <a name="while-you-wait-for-the-provisioning-process-to-complete"></a>Während des Wartens auf den Abschluss des Bereitstellungsvorgangs

1. Klicken Sie auf die Kachel für Ihre Lösung mit dem Status **Bereitstellung** .
2. Beachten Sie die **Bereitstellungsstatus** während der Bereitstellung von Azure-Diensten im Azure-Abonnement.
3. Nach Abschluss der Bereitstellung ändert sich der Status in **Bereit**.
4. Klicken Sie auf die Kachel, um im rechten Bereich Details zur Lösung anzuzeigen.

> [!NOTE]
> Falls beim Bereitstellen der vorkonfigurierten Lösung Probleme auftreten, finden Sie unter [Berechtigungen für die Website „azureiotsuite.com“][lnk-permissions] und die [häufig gestellten Fragen zur verbundenen Factory](iot-suite-faq-cf.md) weitere Informationen. Sollten die Probleme weiterhin auftreten, können Sie im [Portal][lnk-portal] ein Dienstticket erstellen.

Erwarten Sie Informationen zu Ihrer Lösung, die nicht aufgeführt werden? Senden Sie uns Vorschläge für neue Funktionen über den [Feedbackbereich](https://feedback.azure.com/forums/321918-azure-iot)zu.

## <a name="scenario-overview"></a>Übersicht über das Szenario

Wenn Sie die vorkonfigurierte Connected Factory-Lösung bereitstellen, verfügt diese bereits über Ressourcen, mit denen Sie ein allgemeines Industrieszenario durchlaufen können. Bei diesem Szenario melden mehrere Werke (Factories), die mit der Lösung verbunden sind, die Datenwerte, die zum Berechnen der Gesamtanlageneffektivität (Overall Equipment Effectiveness, OEE) und der Key Performance Indicators (KPIs) erforderlich sind. Im weiteren Verlauf wird Folgendes vermittelt:

* Überwachen von Werk, Fertigungsstrecken, OEE von Stationen und KPI-Werten
* Analysieren der Telemetriedaten, die über diese Geräte mit Azure Time Series Insights generiert werden
* Reagieren auf Warnungen zum Beheben von Problemen

Ein wichtiges Merkmal dieses Szenarios: Die Aktionen können alle per Remotezugriff über das Lösungsdashboard ausgeführt werden. Sie benötigen also keinen physischen Zugriff auf die Geräte.

## <a name="view-the-solution-dashboard"></a>Anzeigen des Lösungsdashboards

Im Lösungsdashboard können Sie die bereitgestellte Lösung verwalten. Dies ist eine hierarchische Darstellung einer globalen Werkskonfiguration. Beispielsweise können Sie OEE und KPIs anzeigen und neue Knoten für Telemetriedaten und Aktionswarnungen veröffentlichen.

1. Wenn die Bereitstellung abgeschlossen ist und auf der Kachel für die vorkonfigurierte Lösung **Bereit** angezeigt wird, wählen Sie **Starten** aus, um eine neue Registerkarte mit dem Portal für die Connected Factory-Lösung zu öffnen.

    ![Vorkonfigurierte Lösung starten][img-launch-solution]

1. Standardmäßig zeigt das Lösungsportal das *Dashboard* an. Über das Menü auf der linken Seite gelangen Sie zu anderen Bereichen des Portals.

    ![Vorkonfigurierte Connected Factory-Lösung: Dashboard][cf-img-menu]

Im Dashboard werden die folgenden Informationen angezeigt:

* Ein Bereich mit der **Werksliste**, in dem der Status, der Speicherort und die aktuelle Produktionskonfiguration für die Lösung angezeigt werden. Beim ersten Ausführen der Lösung sind mehrere simulierte Geräte vorhanden. Die Simulation der Fertigungsstrecke besteht aus drei echten OPC UA-Servern (OPC Unified Architecture) pro Fertigungsstrecke zum Durchführen von simulierten Aufgaben und Freigeben von Daten. Weitere Informationen zu OPC UA finden Sie in den [häufig gestellten Fragen zur verbundenen Factory](iot-suite-faq-cf.md).
* Eine **Karte** mit den Standorten der einzelnen Geräte, die mit der Lösung verbunden sind. Für die Lösung kann die Bing Maps-API zum Ausgeben von Informationen auf der Karte verwendet werden. Wenn Ihr Abonnement für die Bing Maps Enterprise-API aktiviert ist, wird dieses Feature automatisch verwendet. Wenn nicht, helfen Ihnen die Informationen zur Verwendung einer dynamischen Karte in den [häufig gestellten Fragen][lnk-faq] weiter.
* Ein Bereich **Warnungen** zum Anzeigen von Warnungen, die generiert werden, wenn ein Telemetrie- oder OEE/KPI-Wert einen bestimmten Schwellenwert überschreitet.
* Ein Bereich **Overall Equipment Effectiveness**, in dem die OEE-Werte für das gesamte Unternehmen oder das jeweils angezeigte Werk, die Fertigungsstrecke oder die Station angegeben werden. Dieser Wert wird über die Stationsansicht auf Unternehmensebene aggregiert. Der OEE-Wert und die Elemente, aus denen dieser Wert besteht, können weiter analysiert werden.
* Ein Bereich **Key Performance Indicators**, in dem die Anzahl von produzierten Einheiten und der Energieverbrauch des gesamten Unternehmens bzw. des jeweiligen Werks, der Fertigungsstrecke oder der Station angezeigt werden. Diese Werte werden über eine Stationsansicht auf Unternehmensebene aggregiert.

## <a name="view-factories"></a>Anzeigen von Werken

Im Bereich *Factories* (Werke) werden der geografische Standort aller Werke der Lösung, ihr Status und die aktuelle Produktionskonfiguration angezeigt. Über die Liste mit den Standorten können Sie zu den anderen Ebenen der Lösungshierarchie navigieren. Die Zeilen in der Liste sind Hyperlinks, mit denen Details der Fertigungsstrecken am jeweiligen Standort verlinkt werden. Sie können dann einen Drilldown in die Details der Fertigungsstrecke und auf die Ebene der Stationsanzeige durchführen. Außerdem können Sie einen Filter auf die Liste anwenden.

![Vorkonfigurierte Connected Factory-Lösung: Werke][cf-img-factories] 

1. Im Bereich **Factory** (Werk) wird die Liste mit den Werken für diese Lösung angezeigt.

2. In der Liste mit den Werken werden anfänglich sechs Werke angezeigt, die über den Bereitstellungsprozess erstellt werden. Sie können der Lösung zusätzliche simulierte und physische Geräte hinzufügen.

3. Klicken Sie zum Anzeigen der Details eines Werks in der Liste mit den Werken auf die entsprechende Zeile.

4. Klicken Sie in der Liste auf die entsprechende Zeile, um die Details einer Fertigungsstrecke anzuzeigen.

5. Klicken Sie in der Liste auf die entsprechende Zeile, um die veröffentlichten OPC UA-Knoten einer Station der Fertigungsstrecke anzuzeigen.

6. Klicken Sie in der Liste auf die entsprechende Zeile, um die Details eines bestimmten Knotens der Station anzuzeigen. Mit dieser Aktion wird der Kontextbereich mit Time Series Insights-Visualisierungen gestartet. Klicken Sie auf diese Graphen, um in der Umgebung des Time Series Insights-Explorers weitere Analysen durchzuführen.

## <a name="view-map"></a>Anzeigen der Karte

Wenn für Ihr Abonnement Zugriff auf die Bing Maps-API besteht, werden Ihnen auf der Karte mit den *Werken* der geografische Standort und der Status aller Werke einer Lösung angezeigt. Klicken Sie auf die Standorte, die auf der Karte angezeigt werden, um Details zum jeweiligen Standort anzuzeigen.

![Vorkonfigurierte Connected Factory-Lösung: Karte][cf-img-map]

## <a name="view-alerts"></a>Anzeigen von Warnungen

Der Bereich **Warnung** enthält Warnungen, die generiert werden, wenn ein gemeldeter Wert oder ein berechneter OEE/KPI-Wert den konfigurierten Schwellenwert übersteigt. In diesem Bereich werden Warnungen auf jeder Ebene der Hierarchie angezeigt – von der Stationsebenenansicht bis zur globalen Ansicht. Die Warnungen enthalten eine Beschreibung der Warnung, des Datums, der Uhrzeit, des Standorts und der Häufigkeit. Sie erhalten einen Einblick in die Daten, die zu der Warnung geführt haben, indem die Time Series Insights-Daten verwendet werden. Die Time Series Insights-Daten werden in den Warnungen visualisiert, soweit dies möglich ist. Als Administrator können Sie Standardaktionen für die Warnungen durchführen, z.B.:

* Schließen der Warnung
* Bestätigen der Warnung

Optional können Sie auch komplexere Aktionen durchführen. Für den Knoten „Pressure OPC UA“ (OPC UA für Druck) der Baugruppe können Sie beispielsweise auch Folgendes durchführen:

* Anzeigen hilfreicher Informationen auf einer Webseite in einem neuen Browserfenster
* Beseitigen der Warnungsursache durch Aufrufen einer OPC UA-Methode auf dem Gerät
* Unterdrücken der Verfügbarkeit der Standardaktionen

    ![Vorkonfigurierte Connected Factory-Lösung: Warnungen][cf-img-alerts]

> [!NOTE]
> Diese Warnungen werden mit Regeln generiert, die in der vorkonfigurierten Lösung in einer Konfigurationsdatei angegeben werden. Mit diesen Regeln können Warnungen generiert werden, wenn die OEE- bzw. KPI-Werte oder die Werte des OPC UA-Knotens den konfigurierten Schwellenwert überschreiten.

1. Im Bereich mit den **Warnungen** werden die Warnungen angezeigt, die für diese Lösung generiert werden.

2. Klicken Sie im Bereich mit den Warnungen auf eine Warnung, um die dazugehörigen Details anzuzeigen.

3. Klicken Sie im Bereich mit den Warnungen zum weiteren Analysieren der Warnungsdaten auf den Graphen, um die Umgebung für den Time Series Insights-Explorer zu öffnen.

4. Im Bereich mit den Warnungen sind mehrere Aktionen zum Behandeln der Warnung verfügbar. Wählen Sie die jeweils passende Option aus, und klicken Sie auf die Schaltfläche zum Ausführen von Aktionen.

## <a name="view-overall-equipment-efficiency"></a>Anzeigen der Gesamtanlageneffektivität (Overall Equipment Effectiveness, OEE)

Mit dem OEE-Wert wird die Effizienz des Fertigungsprozesses bewertet, indem produktionsbezogene Betriebsparameter verwendet werden. OEE ist in der Industrie eine standardmäßige Kennzahl. Sie wird berechnet, indem der Verfügbarkeitsfaktor, der Leistungsfaktor und der Qualitätsfaktor miteinander multipliziert werden: OEE = Verfügbarkeit x Leistung x Qualität.

![Vorkonfigurierte Connected Factory-Lösung: OEE][cf-img-oee]

1. Navigieren Sie zur jeweils gewünschten Ansicht, um den OEE-Wert für eine Ebene der Hierarchie anzuzeigen. Der OEE-Wert für diese Ansicht wird im Bereich zusammen mit den einzelnen Elementen angezeigt, die den OEE-Prozentsatz bilden.

2. Zur eingehenderen Analyse des OEE-Werts für eine beliebige Ebene der Hierarchiedaten können Sie auf den OEE-Prozentsatz, den Verfügbarkeitsprozentsatz, den Leistungsprozentsatz oder den Qualitätsprozentsatz klicken. Es wird ein Kontextbereich mit auf Time Series Insights basierenden Visualisierungen angezeigt, in dem Daten der letzten Stunde, letzten 24 Stunden und letzten sieben Tage angegeben sind.

    ![Vorkonfigurierte Connected Factory-Lösung: TSI-Visualisierung][cf-img-tsi-visualization]

3. Klicken Sie im Bereich mit den Warnungen auf den Graphen, um die Warnungsdaten eingehender zu analysieren. Mit dieser Aktion wird die Umgebung für den Time Series Insights-Explorer geöffnet.

    ![Vorkonfigurierte Connected Factory-Lösung: TSI-Explorer][cf-img-tsi-explorer]

## <a name="view-key-performance-indicators"></a>Anzeigen von Key Performance Indicators

Die Lösung enthält zwei Key Performance Indicators: *Einheiten pro Stunde* und *Energieverbrauch in kWh*.

![Vorkonfigurierte Connected Factory-Lösung: KPI][cf-img-kpi]

1. Navigieren Sie zur jeweiligen Ansicht, um für eine beliebige Ebene der Hierarchie die Einheiten pro Stunde oder die verbrauchte Energie anzuzeigen. Die Einheiten pro Stunde und die verbrauchte Energie werden im Bereich angezeigt.

2. Zur eingehenderen Analyse der Einheiten pro Stunde oder des Energieverbrauchs für eine beliebige Ebene der Hierarchiedaten können Sie im Bereich **Key Performance Indicators** auf die entsprechende Anzeige klicken. Es wird ein Kontextbereich mit auf Time Series Insights basierenden Visualisierungen angezeigt, in dem Sie die Daten der letzten Stunde, letzten 24 Stunden und letzten sieben Tage aufrufen können.

## <a name="scenario-review"></a>Szenariorückblick

In diesem Szenario haben Sie die OEE- und KPI-Werte im Dashboard für Ihr Werk überwacht. Anschließend haben Sie Time Series Insights verwendet, um weitere Informationen bereitzustellen und einen Drilldown in die Telemetriedaten für die OEE- und KPI-Werte durchzuführen, um Anomalien erkennen zu können. Außerdem haben Sie den Bereich für Warnungen verwendet, um für Ihre Werke bestehende Probleme anzuzeigen, und Sie haben die verfügbaren Aktionen genutzt, um die Warnung zu behandeln.

## <a name="other-features"></a>Andere Funktionen

In den folgenden Abschnitten werden einige Zusatzfeatures der Connected Factory-Lösung beschrieben, die im vorherigen Szenario nicht behandelt wurden.

## <a name="apply-filters"></a>Anwenden von Filtern

1. Klicken Sie auf das **Chevron**, um eine Liste mit verfügbaren Filtern im Bereich mit den Standorten der Werke oder im Bereich mit den Warnungen anzuzeigen.

2. Der Filterbereich wird angezeigt. 

    ![Vorkonfigurierte Connected Factory-Lösung: Filter][cf-img-alert-filter]

3. Wählen Sie den benötigten Filter aus. Bei Bedarf können Sie in die Filterfelder auch einen anderen Text eingeben.

4. Anschließend wird der Filter für Sie angewendet. Im Dashboard wird mithilfe eines Trichters auch der Filterstatus angegeben, der in den Tabellen mit den Werken und Warnungen angezeigt wird.

    ![Vorkonfigurierte Connected Factory-Lösung: Filter][cf-img-alert-filter-funnel]

    > [!NOTE]
    > Ein aktiver Filter hat keine Auswirkung auf die angezeigten OEE- und KPI-Werte, sondern dient nur zum Filtern des Listeninhalts.

5. Zum Löschen eines Filters klicken Sie auf den Trichter und dann im Bereich mit dem Filterkontext auf die Option zum Filtern. In den Tabellen mit den Werken und Warnungen wird dann der Text **Alle** angezeigt.

## <a name="browse-an-opc-ua-server"></a>Durchsuchen eines OPC UA-Servers

Beim Bereitstellen der vorkonfigurierten Lösung stellen Sie automatisch simulierte OPC UA-Server bereit, die Sie mit dem Browser der Lösung durchsuchen können. Bei diesen Servern handelt es sich um *simulierte OPC UA-Server*. Simulierte Server erleichtern Ihnen das Experimentieren mit der vorkonfigurierten Lösung, ohne dass Sie echte physische Server bereitstellen müssen. Wenn Sie keinen echten OPC UA-Server mit der Lösung verbinden möchten, helfen Ihnen die Informationen im Tutorial [Connect your OPC UA device to the connected factory preconfigured solution][lnk-connect-cf] (Verbinden Ihres OPC UA-Geräts mit der vorkonfigurierten Connected Factory-Lösung) weiter.

1. Klicken Sie in der Navigationsleiste des Dashboards auf das **Symbol für „Werk“**.

    ![Vorkonfigurierte Connected Factory-Lösung: Serverbrowser][cf-img-server-browser]

2. Wählen Sie in der vorkonfigurierten Liste einen der aufgeführten Server aus. In dieser Liste werden die Server angezeigt, die für Sie in der vorkonfigurierten Lösung bereitgestellt werden.

    ![Vorkonfigurierte Connected Factory-Lösung: Serverauswahl][cf-img-server-choice]

3. Klicken Sie auf **Verbinden**. Ein Sicherheitsdialogfeld wird angezeigt. Es ist sicher, für die Simulation auf **Fortsetzen** zu klicken.

4. Klicken Sie zum Erweitern eines Knotens in der Serverstruktur auf den gewünschten Knoten. Knoten, die Telemetriedaten veröffentlichen, sind mit einem Häkchen gekennzeichnet.

    ![Vorkonfigurierte Connected Factory-Lösung: Serverstruktur][cf-img-server-tree]

5. Klicken Sie mit der rechten Maustaste auf ein Element, um Daten für den Knoten zu lesen, zu schreiben und zu veröffentlichen oder den Knoten aufzurufen. Es hängt von den Berechtigungen und Attributen des Knotens ab, welche Aktionen verfügbar sind. Bei der Option zum Lesen wird ein Kontextbereich mit dem Wert des jeweiligen Knotens angezeigt. Bei der Option zum Schreiben wird ein Kontextbereich angezeigt, in dem Sie einen neuen Wert eingeben können. Bei der Option zum Aufrufen wird ein Knoten angezeigt, über den Sie die Parameter für den Aufruf eingeben können.

## <a name="publish-a-node"></a>Veröffentlichen eines Knotens

Beim Durchsuchen eines *simulierten OPC UA-Servers* können Sie auch die Veröffentlichung neuer Knoten wählen. Sie können die Telemetrie dieser Knoten in der Lösung analysieren. Diese *simulierten OPC UA-Server* erleichtern das Experimentieren mit der vorkonfigurierten Lösung, ohne dass dafür echte physische Geräte bereitgestellt werden müssen.

1. Navigieren Sie zu einem Knoten in der Browserstruktur des OPC UA-Servers, den Sie veröffentlichen möchten.

2. Klicken Sie mit der rechten Maustaste auf den Knoten.

3. Wählen Sie **Veröffentlichen**.

    ![Connected Factory veröffentlicht Knoten][cf-img-publish-node]

4. Es wird ein Kontextbereich mit dem Hinweis angezeigt, dass die Veröffentlichung erfolgreich war. In der Stationsebenenansicht wird neben dem Knoten ein Häkchen angezeigt.

    ![Vorkonfigurierte Connected Factory-Lösung: Veröffentlichung erfolgreich][cf-img-publish-success]

## <a name="command-and-control"></a>Befehl und Steuerung

Mit dem Konzept des „verbundenen Werks“ (Connected Factory) können Sie Ihre Industrieanlagen direkt über die Cloud steuern. Sie können dieses Feature nutzen, um auf Warnungen zu reagieren, die vom Gerät generiert werden. Beispielsweise können Sie aus der Cloud einen Befehl an ein Gerät oder eine Anlage senden. Sie finden die verfügbaren Befehle in der Browserstruktur des OPC UA-Servers unter dem Knoten **StationCommands**. In diesem Szenario wird an einer Montagestation einer Fertigungsstraße in München ein Druckreduzierventil geöffnet. Zum Verwenden der Funktionen unter „Befehl und Steuerung“ müssen Sie für die Bereitstellung der vorkonfigurierten Lösung über die Rolle **Administrator** verfügen.

1. Navigieren Sie in der Browserstruktur des OPC UA-Servers zum Knoten **StationCommands**.

2. Wählen Sie den Befehl, den Sie verwenden möchten.

3. Klicken Sie mit der rechten Maustaste auf den Knoten.

4. Wählen Sie die Option **Aufrufen**.

    ![Vorkonfigurierte Connected Factory-Lösung: Befehl „Aufrufen“][cf-img-call-command]

5. Ein Kontextbereich wird angezeigt. Er enthält Informationen dazu, welche Methode aufgerufen werden soll und welche Parameterdetails verfügbar sind.

6. Wählen Sie die Option **Aufrufen**.

    ![Vorkonfigurierte Connected Factory-Lösung: Kontext des Aufrufs][cf-img-call-context]

7. Der Kontextbereich wird aktualisiert, um Sie darüber zu informieren, dass der Methodenaufruf erfolgreich war. Sie können überprüfen, ob der Aufruf erfolgreich war, indem Sie den Wert des Knotens für die Druckeinstellung ablesen, der als Folge des Aufrufs aktualisiert wurde.

    ![Vorkonfigurierte Connected Factory-Lösung: Aufruf erfolgreich][cf-img-call-success]


## <a name="behind-the-scenes"></a>Abläufe im Hintergrund

Wenn Sie eine vorkonfigurierte Lösung bereitstellen, werden vom Bereitstellungsprozess mehrere Ressourcen im gewählten Azure-Abonnement erstellt. Sie können diese Ressourcen im Azure-[Portal][lnk-portal] anzeigen. Der Bereitstellungsprozess erstellt eine **Ressourcengruppe** mit einem Namen basierend auf dem Namen, den Sie für Ihre vorkonfigurierte Lösung wählen:

![Vorkonfigurierte Lösung im Azure-Portal][img-cf-portal]

Sie können die Einstellungen der einzelnen Ressourcen anzeigen, indem Sie sie in der Ressourcengruppe in der Liste mit den Ressourcen auswählen.

Sie können auch den Quellcode für die vorkonfigurierte Lösung anzeigen. Der Quellcode der vorkonfigurierten Connected Factory-Lösung ist im GitHub-Repository [azure-iot-connected-factory][lnk-cfgithub] enthalten:

Wenn Sie fertig sind, können Sie die vorkonfigurierte Lösung auf der Website [azureiotsuite.com][lnk-azureiotsuite] aus Ihrem Azure-Abonnement löschen. Über diese Website können Sie problemlos alle Ressourcen löschen, die bei der Erstellung der vorkonfigurierten Lösung bereitgestellt wurden.

> [!NOTE]
> Um sicherzustellen, dass Sie alle Teile der vorkonfigurierten Lösung löschen, sollten Sie den Löschvorgang auf der Website [azureiotsuite.com][lnk-azureiotsuite] durchführen. Löschen Sie nicht die Ressourcengruppe im Portal.

## <a name="next-steps"></a>Nächste Schritte

Sie haben eine funktionierende vorkonfigurierte Lösung bereitgestellt und können nun mit den ersten Schritten mit IoT Suite fortfahren. Lesen Sie dazu die folgenden Artikel:

* [Vorkonfigurierte Connected Factory-Lösung – Exemplarische Vorgehensweise][lnk-rm-walkthrough]
* [Verbinden Ihres Geräts mit der vorkonfigurierten verbundenen Werkslösung][lnk-connect-cf]
* [Berechtigungen für die Website „azureiotsuite.com“][lnk-permissions]

[img-cf-home]:media/iot-suite-connected-factory-overview/cf-dashboard.png
[img-launch-solution]: media/iot-suite-connected-factory-overview/launch-cf.png
[cf-img-menu]: media/iot-suite-connected-factory-overview/cf-dashboard-menu.png
[cf-img-factories]:media/iot-suite-connected-factory-overview/cf-dashboard-factory.png
[cf-img-map]:media/iot-suite-connected-factory-overview/cf-dashboard-map.png
[cf-img-alerts]:media/iot-suite-connected-factory-overview/cf-dashboard-alerts.png
[cf-img-oee]:media/iot-suite-connected-factory-overview/cf-dashboard-oee.png
[cf-img-kpi]:media/iot-suite-connected-factory-overview/cf-dashboard-kpi.png
[cf-img-tsi-visualization]:media/iot-suite-connected-factory-overview/cf-dashboard-tsi.png
[cf-img-tsi-explorer]:media/iot-suite-connected-factory-overview/tsi-explorer.png
[cf-img-server-browser]: media/iot-suite-connected-factory-overview/cf-dashboard-browser.png
[cf-img-server-choice]: media/iot-suite-connected-factory-overview/cf-select-server.png
[cf-img-server-tree]:media/iot-suite-connected-factory-overview/cf-server-tree.png
[cf-img-publish-node]:media/iot-suite-connected-factory-overview/cf-publish-node1.png
[cf-img-publish-success]:media/iot-suite-connected-factory-overview/cf-publish-success.png
[cf-img-call-command]:media/iot-suite-connected-factory-overview/cf-command-and-control-call.png
[cf-img-call-context]:media/iot-suite-connected-factory-overview/cf-command-and-control-call-button.png
[cf-img-call-success]:media/iot-suite-connected-factory-overview/cf-command-and-control-succeed.png
[img-cf-portal]:media/iot-suite-connected-factory-overview/cf-resource-group.png
[cf-img-alert-filter]:media/iot-suite-connected-factory-overview/cf-filter.png
[cf-img-alert-filter-funnel]:media/iot-suite-connected-factory-overview/cf-filter-funnel.png

[lnk_free_trial]: http://azure.microsoft.com/pricing/free-trial/
[lnk-preconfigured-solutions]: iot-suite-what-are-preconfigured-solutions.md
[lnk-azureiotsuite]: https://www.azureiotsuite.com
[lnk-portal]: http://portal.azure.com/
[lnk-cfgithub]: https://github.com/Azure/azure-iot-connected-factory
[lnk-rm-walkthrough]: iot-suite-connected-factory-sample-walkthrough.md
[lnk-connect-cf]: iot-suite-connected-factory-gateway-deployment.md
[lnk-permissions]: iot-suite-permissions.md
[lnk-faq]: iot-suite-faq.md

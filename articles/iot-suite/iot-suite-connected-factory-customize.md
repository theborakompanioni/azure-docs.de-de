---
title: "Anpassen der mit Azure IoT Suite verbundenen Werkslösung | Microsoft-Dokumentation"
description: "Beschreibung der Anpassung des Verhaltens der vorkonfigurierten verbundenen Werkslösung."
services: 
suite: iot-suite
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 
ms.service: iot-suite
ms.devlang: c#
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/25/2017
ms.author: dobett
ms.translationtype: HT
ms.sourcegitcommit: 646886ad82d47162a62835e343fcaa7dadfaa311
ms.openlocfilehash: 90a6172dbd887ecda5a9f5d9082a4e136092bc10
ms.contentlocale: de-de
ms.lasthandoff: 08/24/2017

---
# <a name="customize-how-the-connected-factory-solution-displays-data-from-your-opc-ua-servers"></a>Anpassen, wie die verbundene Werkslösung Daten von Ihren OPC UA-Servern anzeigt

## <a name="introduction"></a>Einführung

Die verbundene Werkslösung aggregiert und zeigt Daten von OPC UA-Servern, die mit der Lösung verbunden sind. Sie können in Ihrer Lösung zu den OPC UA-Servern navigieren und Befehle an diese senden. Weitere Informationen zu OPC UA finden Sie in den [häufig gestellten Fragen zur verbundenen Factory](iot-suite-faq-cf.md).

Beispiele für aggregierte Daten in der Lösung sind die anlagenübergreifende Effizienz (Overall Equipment Efficiency, OEE) und Leistungsindikatoren (Key Performance Indicators, KPIs), die Sie im Dashboard auf Werks-, Fertigungsstraßen- und Stationsebene anzeigen können. Der folgende Screenshot zeigt die OEE- und KPI-Werte für die Station **Montage** in der **Fertigungsstraße 1** im Werk **München**:

![Beispiel für OEE- und KPI-Werte in der Lösung][img-oee-kpi]

Die Lösung ermöglicht Ihnen das Anzeigen detaillierter Informationen in bestimmten Datenelementen, die von den *Stationen* genannten OPC UA-Servern stammen. Der folgende Screenshot zeigt grafische Darstellungen der Anzahl der an einer bestimmten Station gefertigten Teile:

![Grafische Darstellungen der Anzahl gefertigter Teile][img-manufactured-items]

Wenn Sie auf eines der Diagramme klicken, können Sie die Daten mithilfe von Time Series Insights (TSI) weiter untersuchen:

![Untersuchen von Daten mit Time Series Insights][img-tsi]

Dieser Artikel beschreibt Folgendes:

- Das Verfügbarmachen der Daten in den verschiedenen Ansichten
- Das Anpassen der Anzeige der Daten durch die Lösung

## <a name="data-sources"></a>Datenquellen

Die verbundene Werkslösung zeigt Daten von OPC UA-Servern an, die mit der Lösung verbunden sind. Die Standardinstallation umfasst mehrere OPC UA-Server, auf denen eine Werkssimulation ausgeführt wird. Sie können eigene OPC UA-Server hinzufügen, die Sie [über ein Gateway][lnk-connect-cf] mit der Lösung verbinden können.

Sie können im Dashboard die Datenelemente durchsuchen, die ein verbundener OPC UA-Server an Ihre Lösung senden kann:

1. Navigieren Sie zur Ansicht **Select an OPC UA server** (OPC UA-Server auswählen):

    ![Navigieren zur Ansicht „Select an OPC UA server“ (OPC UA-Server auswählen)][img-select-server]

1. Wählen Sie einen Server aus, und klicken Sie auf **Connect** (Verbinden). Klicken Sie auf **Proceed** (Fortfahren), wenn die Sicherheitswarnung angezeigt wird.

    > [!NOTE]
    > Diese Warnung wird nur einmal je Server angezeigt und richtet eine Vertrauensstellung zwischen Lösungsdashboard und Server ein.

1. Sie können die Datenelemente durchsuchen, die der Server an die Lösung senden kann. Elemente, die an die Lösung gesendet werden, weisen ein grünes Häkchen auf:

    ![Veröffentlichte Elemente][img-published]

1. Wenn Sie ein *Administrator* in der Lösung sind, können Sie ein Datenelement veröffentlichen, um es in der verbundenen Werkslösung verfügbar zu machen. Als Administrator können Sie auch den Wert von Datenelementen und Aufrufmethoden im OPC UA-Server ändern.

## <a name="map-the-data"></a>Zuordnen der Daten

Mithilfe der verbundenen Werkslösung werden die vom OPC UA-Server stammenden veröffentlichten Datenelemente in verschiedenen Ansichten in der Lösung zugeordnet und aggregiert. Die verbundene Werkslösung wird in Ihrem Azure-Konto bereitgestellt, sobald Sie die Lösung bereitstellen. Diese Zuordnungsinformationen werden in der verbundenen Werkslösung in Visual Studio in einer JSON-Datei gespeichert. Sie können diese JSON-Konfigurationsdatei in der verbundenen Werkslösung in Visual Studio anzeigen und ändern. Nachdem Sie eine Änderung vorgenommen haben, können Sie die Lösung erneut bereitstellen.

Sie können die Konfigurationsdatei für folgende Aufgaben verwenden:

- Bearbeiten der vorhandenen simulierten Werke, Fertigungsstraßen und Stationen
- Zuordnen von Daten von realen OPC UA-Servern, die Sie mit der Lösung verbinden.

Zum Klonen einer Kopie der verbundenen Werklösung in einer Visual Studio-Projektmappe geben Sie den folgenden Git-Befehl an:

`git clone https://github.com/Azure/azure-iot-connected-factory.git`

Die Datei **ContosoTopologyDescription.json** definiert die Zuordnung von OPC UA Server-Datenelementen zu den Ansichten im Dashboard der verbundenen Werkslösung. Sie finden diese Konfigurationsdatei in der Visual Studio-Projektmappe im Ordner **Contoso\Topology** im Projekt **WebApp**.

Der Inhalt der JSON-Datei ist als Hierarchie von Knoten der Typen Werk, Fertigungsstraße und Station aufgebaut. Diese Hierarchie definiert die Navigationshierarchie im Dashboard des verbundenen Werks. Werte an jedem Knoten der Hierarchie bestimmen die Informationen, die im Dashboard angezeigt werden. Die JSON-Datei enthält beispielsweise die folgenden Werte für das Werk München:

```json
"Guid": "73B534AE-7C7E-4877-B826-F1C0EA339F65",
"Name": "Munich",
"Description": "Braking system",
"Location": {
    "City": "Munich",
    "Country": "Germany",
    "Latitude": 48.13641,
    "Longitude": 11.57754
},
"Image": "munich.jpg"
```

Name, Beschreibung und Ort werden in dieser Sicht im Dashboard angezeigt:

![Daten aus München im Dashboard][img-munich]

Für jedes Werk bzw. jede Fertigungsstraße und Station gibt es eine Bildeigenschaft. Sie finden diese JPEG-Dateien im Projekt **WebApp** im Ordner **Content\img**. Diese Bilddateien werden im Dashboard des verbundenen Werks angezeigt.

Jede Station weist mehrere detaillierte Eigenschaften auf, die der Zuordnung der OPC UA-Datenelemente dienen. Diese Eigenschaften werden in den folgenden Abschnitten beschrieben:

### <a name="opcuri"></a>OpcUri

Der **OpcUri**-Wert ist der OPC UA-Anwendungs-URI, der den OPC UA Server eindeutig bestimmt. Der **OpcUri**-Wert der Montagestation in der Fertigungsstraße 1 in München lautet z.B. wie folgt: **urn:scada2194:ua:munich:productionline0:assemblystation**.

Sie können die URIs der verbundenen OPC UA-Server im Dashboard der Lösung anzeigen:

![Anzeigen von URIs von OPC UA-Servern][img-server-uris]

### <a name="simulation"></a>Simulation

Die Informationen im Knoten **Simulation** gelten spezifisch für die OPC UA-Simulation, die auf den standardmäßig bereitgestellten OPC UA-Servern ausgeführt wird. Sie eignen sich nicht für einen realen OPC UA-Server.

### <a name="kpi1-and-kpi2"></a>Kpi1 und Kpi2

Mit diesen Knoten wird beschrieben, wie Daten aus der Station zu zwei KPI-Werten im Dashboard beitragen. In einer Standardbereitstellung sind diese KPI-Werte „Einheiten pro Stunde“ und „kWh pro Stunde“. Die Lösung berechnet KPI-Werte auf Stationsebene und aggregiert sie auf Fertigungsstraßen- und Werksebene.

Jeder KPI hat einen Mindest-, Höchst- und Zielwert. Jede KPI-Wert kann auch Warnungsaktionen für die verbundene Werkslösung definieren. Der folgende Codeausschnitt zeigt die KPI-Definitionen für die Montagstation an der Fertigungsstraße 1 in München:

```json
"Kpi1": {
  "Minimum": 150,
  "Target": 300,
  "Maximum": 600
},
"Kpi2": {
  "Minimum": 50,
  "Target": 100,
  "Maximum": 200,
  "MinimumAlertActions": [
    {
      "Type": "None"
    }
  ]
}
```

Der folgende Screenshot zeigt die KPI-Daten im Dashboard.

![KPI-Informationen im Dashboard][lnk-kpi]

### <a name="opcnodes"></a>OpcNodes

Der Knoten **OpcNodes** bestimmt die vom OPC UA-Server veröffentlichten Datenelemente und gibt an, wie diese Daten verarbeitet werden.

Der **NodeId**-Wert bestimmt die spezifische OPC UA-Knoten-ID des OPC UA-Servers. Der erste Knoten in der Montagstation der Fertigungsstraße 1 in München hat den Wert **ns=2;i=385**. Der **NodeId**-Wert gibt das Datenelement an, das aus dem OPC UA-Server gelesen werden soll. **SymbolicName** ist ein Anzeigename für diese Daten im Dashboard.

Andere Werte, die zu den jeweiligen Knoten gehören, sind in der folgenden Tabelle zusammengefasst:

| Wert | Beschreibung |
| ----- | ----------- |
| Relevance  | Die KPI- und OEE-Werte, zu denen diese Daten beitragen. |
| OpCode     | Gibt an, wie die Daten aggregiert werden. |
| Units      | Die Einheiten, die im Dashboard verwendet werden.  |
| Visible    | Gibt an, ob dieser Wert im Dashboard angezeigt wird. Einige Werte werden in Berechnungen verwendet, aber nicht angezeigt.  |
| Maximum    | Der Höchstwert, der eine Warnung im Dashboard auslöst. |
| MaximumAlertActions | Aktion, die als Reaktion auf eine Warnung ausgeführt werden soll. Beispiel: Senden eines Befehls an eine Station. |
| ConstValue | Konstanter Wert, der in einer Berechnung verwendet wird. |

## <a name="deploy-the-changes"></a>Bereitstellen der Änderungen

Nach Abschluss der Änderungen an der Datei **ContosoTopologyDescription.json** müssen Sie die verbundene Werkslösung erneut in Ihrem Azure-Konto bereitstellen.

Das Repository **azure-iot-connected-factory** enthält das PowerShell-Skript **build.ps1**, mit dem Sie die Lösung erneut erstellen und bereitstellen können.

## <a name="next-steps"></a>Nächste Schritte

In den folgenden Artikeln erfahren Sie mehr über die verbundene vorkonfigurierte Werkslösung:

* [Vorkonfigurierte Connected Factory-Lösung – Exemplarische Vorgehensweise][lnk-rm-walkthrough]
* [Bereitstellen eines Gateways für verbundene Factorys][lnk-connect-cf]
* [Berechtigungen für die Website „azureiotsuite.com“][lnk-permissions]
* [Verbundene Factory – häufig gestellte Fragen](iot-suite-faq-cf.md)
* [Häufig gestellte Fragen][lnk-faq]


[img-oee-kpi]: ./media/iot-suite-connected-factory-customize/oeenadkpi.png
[img-manufactured-items]: ./media/iot-suite-connected-factory-customize/manufactured.png
[img-tsi]: ./media/iot-suite-connected-factory-customize/tsi.png
[img-select-server]: ./media/iot-suite-connected-factory-customize/selectserver.png
[img-published]: ./media/iot-suite-connected-factory-customize/published.png
[img-munich]: ./media/iot-suite-connected-factory-customize/munich.png
[img-server-uris]: ./media/iot-suite-connected-factory-customize/serveruris.png
[lnk-kpi]: ./media/iot-suite-connected-factory-customize/kpidisplay.png

[lnk-rm-walkthrough]: iot-suite-connected-factory-sample-walkthrough.md
[lnk-connect-cf]: iot-suite-connected-factory-gateway-deployment.md
[lnk-permissions]: iot-suite-permissions.md
[lnk-faq]: iot-suite-faq.md

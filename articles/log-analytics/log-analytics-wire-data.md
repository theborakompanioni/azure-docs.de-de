---
title: "Wire Data-Lösung in Log Analytics | Microsoft Docs"
description: "Bei Wire Data handelt es sich um zusammengefasste Netzwerk- und Leistungsdaten von Computern mit OMS-Agents, z.B. Operations Manager und Agents mit Windows-Verbindung. Netzwerkdaten werden mit Ihren Protokolldaten kombiniert, um Ihnen das Korrelieren von Daten zu ermöglichen."
services: log-analytics
documentationcenter: 
author: bandersmsft
manager: jwhit
editor: 
ms.assetid: fc3d7127-0baa-4772-858a-5ba995d1519b
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 11/09/2016
ms.author: banders
translationtype: Human Translation
ms.sourcegitcommit: 15858f7b7436536e6bae7fcfd6a50c722d2d04a2
ms.openlocfilehash: be00cb9b1e8ba5d9d8368695ca8d448d466e8f47


---
# <a name="wire-data-solution-in-log-analytics"></a>Wire Data-Lösung in Log Analytics
Bei Wire Data handelt es sich um zusammengefasste Netzwerk- und Leistungsdaten von Computern mit OMS-Agents, z.B. Operations Manager und Agents mit Windows-Verbindung. Netzwerkdaten werden mit Ihren Protokolldaten kombiniert, um Ihnen das Korrelieren von Daten zu ermöglichen. Mit OMS-Agents, die auf Computern in Ihrer IT-Infrastruktur installiert sind, werden Netzwerkdaten überwacht, die für die Netzwerkschichten 2 und 3 des [OSI-Modells](https://en.wikipedia.org/wiki/OSI_model) an Computer und von Computern gesendet werden, z.B. die verschiedenen verwendeten Protokolle und Ports.

> [!NOTE]
> Die Wire Data-Lösung ist zum Hinzufügen zu Arbeitsbereichen derzeit nicht verfügbar. Kunden, für die die Wire Data-Lösung bereits aktiviert wurde, können diese Lösung weiterhin verwenden.
>
>

Standardmäßig erfasst OMS protokollierte Daten für CPU, Arbeitsspeicher, Datenträger und Netzwerk von Indikatoren, die in Windows integriert sind. Die Erfassung von Netzwerkdaten und anderen Daten wird für jeden Agent in Echtzeit durchgeführt, einschließlich der vom Computer verwendeten Subnetze und Anwendungsebenenprotokolle. Auf der Seite „Einstellungen“ können Sie auf der Registerkarte „Protokolle“ weitere Leistungsindikatoren hinzufügen.

Wenn Sie [sFlow](http://www.sflow.org/) oder andere Software mit dem [NetFlow-Protokoll von Cisco](http://www.cisco.com/c/en/us/products/collateral/ios-nx-os-software/ios-netflow/prod_white_paper0900aecd80406232.html) verwendet haben, werden Ihnen die in den Daten zur Kommunikation angezeigten Statistiken und Daten bekannt vorkommen.

Beispiele für die Typen von integrierten Abfragen von Protokollsuchen:

* Agents, die Wire Data bereitstellen
* IP-Adresse der Agents, die Wire Data bereitstellen
* Ausgehende Kommunikation über IP-Adressen
* Anzahl von gesendeten Bytes nach Anwendungsprotokollen
* Anzahl von gesendeten Bytes eines Anwendungsdiensts
* Von unterschiedlichen Protokollen empfangene Bytes
* Gesamte Bytes, die von einer IP-Adresse gesendet und empfangen wurden
* IP-Adressen, die mit Agents im Subnetz 10.0.0.0/8 kommuniziert haben
* Durchschnittliche Latenz für Verbindungen mit zuverlässiger Messung
* Computerprozesse, die Netzwerk-Datenverkehr initiiert oder empfangen haben
* Menge des Netzwerk-Datenverkehrs für einen Prozess

Wenn Sie mit Wire Data eine Suche durchführen, können Sie Daten filtern und gruppieren, um Informationen zu den wichtigsten Agents und Protokollen anzuzeigen. Außerdem können Sie sich ansehen, wann bestimmte Computer (IP-Adressen/MAC-Adressen) miteinander kommuniziert haben, wie lange dies gedauert hat und wie viele Daten gesendet wurden. Im Grunde genommen zeigen Sie die Metadaten zum Netzwerk-Datenverkehr an. Dies ist ein suchbasierter Vorgang.

Da Sie Metadaten anzeigen, ist der Vorgang nicht unbedingt gut für die eingehende Problembehandlung geeignet. Bei Wire Data in OMS wird keine vollständige Erfassung aller Netzwerkdaten durchgeführt. Die eingehende Problembehandlung auf Paketebene ist also nicht vorgesehen.
Der Vorteil einer Verwendung des Agents gegenüber anderen Erfassungmethoden besteht darin, dass Sie keine Appliances installieren, Netzwerkswitches neu konfigurieren oder komplizierten Konfigurationen durchführen müssen. Wire Data ist einfach Agent-basiert. Sie installieren den Agent auf einem Computer, um damit den Netzwerk-Datenverkehr zu überwachen. Ein weiterer Vorteil ergibt sich, wenn Sie Workloads überwachen möchten, die bei Cloudanbietern, Hostinganbietern oder unter Microsoft Azure ausgeführt werden und bei denen sich die Fabric-Ebene nicht im Besitz des Benutzers befindet.

Sie haben aber keinen vollständigen Überblick über die Vorgänge im Netzwerk, wenn Sie nicht auf allen Computern in der Netzwerkinfrastruktur Agents installieren.

## <a name="installing-and-configuring-the-solution"></a>Installieren und Konfigurieren der Lösung
Verwenden Sie die folgenden Informationen zum Installieren und Konfigurieren der Lösung.

* Mit der Wire Data-Lösung werden Daten von Computern erfasst, auf denen Windows Server 2012 R2, Windows 8.1 und neuere Betriebssysteme ausgeführt werden.
* Microsoft .NET Framework 4.0 oder höher ist auf Computern erforderlich, von denen Wire Data-Daten erfasst werden sollen.
* Fügen Sie mithilfe des unter [Hinzufügen von Log Analytics-Lösungen aus dem Lösungskatalog](log-analytics-add-solutions.md)beschriebenen Prozesses die Wire Data-Lösung Ihrem OMS-Arbeitsbereich hinzu.  Es ist keine weitere Konfiguration erforderlich.
* Wenn Sie Wire Data-Daten für eine bestimmte Lösung anzeigen möchten, muss die Lösung bereits dem OMS-Arbeitsbereich hinzugefügt worden sein.

## <a name="wire-data-data-collection-details"></a>Details zur Wire Data-Datensammlung
Wire Data sammelt Metadaten über den Netzwerkverkehr mit den Agents, die Sie aktiviert haben.

Die folgende Tabelle enthält die Datensammlungsmethoden und andere Details dazu, wie Daten für Wire Data erfasst werden.

| Plattform | Direkt-Agent | SCOM-Agent | Azure Storage | SCOM erforderlich? | Daten von SCOM-Agent über Verwaltungsgruppe gesendet | Sammlungshäufigkeit |
| --- | --- | --- | --- | --- | --- | --- |
| Windows (2012 R2 / 8.1 oder höher) |![Ja](./media/log-analytics-wire-data/oms-bullet-green.png) |![Ja](./media/log-analytics-wire-data/oms-bullet-green.png) |![Nein](./media/log-analytics-wire-data/oms-bullet-red.png) |![Nein](./media/log-analytics-wire-data/oms-bullet-red.png) |![Nein](./media/log-analytics-wire-data/oms-bullet-red.png) |Jede Minute |

## <a name="combining-wire-data-with-other-solution-data"></a>Kombinieren von Wire Data mit anderen Lösungsdaten
Die Daten, die für die obigen integrierten Abfragen zurückgegeben werden, können auch selbst interessant sein. Die Nützlichkeit von Wire Data wird aber deutlich, wenn Sie sie mit Informationen anderer OMS-Lösungen kombinieren. Beispielsweise können Sie Sicherheitsereignisdaten verwenden, die mit der Sicherheits- und Überwachungslösung erfasst wurden, und diese mit Wire Data kombinieren, um nach ungewöhnlichen versuchten Netzwerkanmeldungen für benannte Prozesse zu suchen.  In diesem Beispiel verwenden Sie die Operatoren IN und DISTINCT, um Datenpunkte in der Suchabfrage zu verknüpfen.

Anforderungen: Um das folgende Beispiel verwenden zu können, muss die Sicherheits- und Überwachungslösung installiert sein. Sie können aber Daten aus anderen Lösungen verwenden, um sie mit Wire Data zu kombinieren und ähnliche Ergebnisse zu erhalten.

### <a name="to-combine-wire-data-with-security-events"></a>So kombinieren Sie Wire Data mit Sicherheitsereignissen
1. Klicken Sie auf der Seite „Übersicht“ auf die Kachel **WireData** .
2. Klicken Sie in der Liste **Allgemeine WireData-Abfragen** auf **Menge des Netzwerkdatenverkehrs (in Byte) nach Prozess**, um die Liste mit den zurückgegebenen Prozessen anzuzeigen.
    ![WireData-Abfragen](./media/log-analytics-wire-data/oms-wiredata-01.png)
3. Wenn die Liste mit den Prozessen zum einfachen Anzeigen zu lang ist, können Sie die Suchabfrage wie folgt ändern:

    ```
    Type WireData | measure count() by ProcessName | where AggregatedValue <40
    ```
    Das folgende Beispiel enthält einen Prozess mit dem Namen „DancingPigs.exe“, der verdächtig erscheint.
    ![WireData-Suchergebnisse](./media/log-analytics-wire-data/oms-wiredata-02.png)
4. Klicken Sie auf einen benannten Prozess, indem Sie die in der Liste zurückgegebenen Daten verwenden. In diesem Beispiel wurde auf „DancingPigs.exe“ geklickt. Die unten angegebenen Ergebnisse beschreiben den Typ von Netzwerkdatenverkehr, z.B. ausgehende Kommunikation über verschiedene Protokolle.
    ![WireData-Ergebnisse mit Anzeige eines benannten Prozesses](./media/log-analytics-wire-data/oms-wiredata-03.png)
5. Da die Sicherheits- und Überwachungslösung installiert ist, können Sie die Sicherheitsereignisse überprüfen, die über den gleichen Wert für das Feld ProcessName verfügen. Ändern Sie hierzu die Suchabfrage, indem Sie die Operatoren IN und DISTINCT für die Suchabfrage verwenden. Dies können Sie durchführen, wenn sowohl Wire Data als auch andere Lösungsprotokolle Werte in demselben Format enthalten. Ändern Sie die Suchabfrage so, dass sie in etwa wie folgt aussieht:

    ```
    Type=SecurityEvent ProcessName IN {Type:WireData "DancingPigs.exe" | distinct ProcessName}
    ```    

    ![Wire Data-Ergebnisse mit Anzeige von kombinierten Daten](./media/log-analytics-wire-data/oms-wiredata-04.png)
6. In den obigen Ergebnissen sehen Sie, dass Kontoinformationen angezeigt werden. Sie können die Suchabfrage jetzt verfeinern, um zu ermitteln, wie oft das Konto, für das Daten zur Sicherheit und Überprüfung angezeigt werden, von dem Prozess verwendet wurde. Die Abfrage dazu sieht wie folgt aus:        

    ```
    Type=SecurityEvent ProcessName IN {Type:WireData "DancingPigs.exe" | distinct ProcessName} | measure count() by Account
    ```

    ![Wire Data-Ergebnisse mit Anzeige von Kontodaten](./media/log-analytics-wire-data/oms-wiredata-05.png)

## <a name="next-steps"></a>Nächste Schritte
* [Durchsuchen von Protokollen](log-analytics-log-searches.md) und darüber, wie Sie ausführliche Wire Data-Suchdatensätze anzeigen.
* Sehen Sie sich den Blogeintrag [Using Wire Data in Operations Management Suite Log Search](http://blogs.msdn.com/b/dmuscett/archive/2015/09/09/using-wire-data-in-operations-management-suite.aspx) (Verwenden von Wire Data-Daten für die Protokollsuche der Operations Management Suite) von Daniele Muscetta mit weiteren Informationen dazu an, wie oft Daten erfasst werden und wie Sie Sammlungseigenschaften für Operations Manager-Agents ändern können.



<!--HONumber=Nov16_HO3-->



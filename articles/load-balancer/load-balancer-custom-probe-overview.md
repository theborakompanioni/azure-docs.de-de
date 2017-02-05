---
title: "Benutzerdefinierte Load Balancer-Tests und Überwachen des Integritätsstatus | Microsoft Docs"
description: "Erfahren Sie, wie Sie mit benutzerdefinierten Tests für Azure Load Balancer Instanzen hinter einem Load Balancer überwachen."
services: load-balancer
documentationcenter: na
author: kumudd
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 46b152c5-6a27-4bfc-bea3-05de9ce06a57
ms.service: load-balancer
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/24/2016
ms.author: kumud
translationtype: Human Translation
ms.sourcegitcommit: ce2550ca8301fd12d61cca143b2851b84f1a0f50
ms.openlocfilehash: 01afa3a08bbb56d4c6b8b18c5eb07f49706c6482

---

# <a name="understand-load-balancer-probes"></a>Grundlegendes zu Load Balancer-Tests

Azure Load Balancer bietet die Möglichkeit, die Integrität der Serverinstanzen mithilfe von Tests zu überwachen. Wenn ein Test nicht reagiert, beendet der Load Balancer das Senden neuer Verbindungen an die fehlerhafte Instanz. Die vorhandenen Verbindungen sind nicht betroffen, und neue Verbindungen werden an fehlerfreie Instanzen gesendet.

Clouddienstrollen (Workerrollen und Webrollen) verwenden einen Gast-Agent für die Testüberwachung. Wenn Sie virtuelle Computer hinter einem Load Balancer verwenden, muss ein benutzerdefinierter TCP- oder HTTP-Test konfiguriert werden.

## <a name="understand-probe-count-and-timeout"></a>Grundlegendes zu Anzahl und Timeout von Tests

Das Verhalten von Tests hängt von folgenden Faktoren ab:

* Anzahl der erfolgreichen Tests, bei der eine Instanz als „In Betrieb“ bezeichnet werden kann.
* Anzahl der fehlerhaften Tests, bei der eine Instanz als „Ausgefallen“ bezeichnet wird.

Der in „SuccessFailCount“ festgelegte Wert für Timeout und Häufigkeit legt fest, ob eine Instanz ausgeführt oder nicht ausgeführt wird. Im Azure-Portal wird das Timeout auf das Doppelte des Werts für die Häufigkeit festgelegt.

Die Konfiguration von Tests muss für alle Instanzen mit Lastenausgleich für einen Endpunkt (mit anderen Worten: eine Gruppe mit Lastenausgleich) identisch sein. Das heißt, dass Sie nicht für jede Rolleninstanz oder VM im selben gehosteten Dienst für eine bestimmte Endpunktkombination eine unterschiedliche Testkonfiguration wählen können. Beispielsweise muss jede Instanz identische lokale Ports und Timeouts aufweisen.

> [!IMPORTANT]
> Ein Load Balancer-Test verwendet die IP-Adresse 168.63.129.16. Diese öffentliche IP-Adresse ermöglicht die Kommunikation mit internen Plattformressourcen für das Azure Virtual Network-Szenario mit eigener IP-Adresse. Die virtuelle öffentliche IP-Adresse 168.63.129.16 wird in allen Regionen verwendet und nicht geändert. Es wird empfohlen, dass Sie diese IP-Adresse in lokalen Firewallrichtlinien zulassen. Dies dürfte kein Sicherheitsrisiko darstellen, da nur die interne Azure-Plattform eine Nachricht von dieser Adresse senden kann. Andernfalls ist in einer Vielzahl von Szenarien ein unerwartetes Verhalten die Folge, wie z. B. beim Konfigurieren desselben IP-Adressbereichs von 168.63.129.16 mit duplizierten IP-Adressen.

## <a name="learn-about-the-types-of-probes"></a>Weitere Informationen über die Testtypen

### <a name="guest-agent-probe"></a>Gast-Agent-Test

Dieser Test ist nur für Azure Cloud Services verfügbar. Load Balancer nutzt den Gast-Agent auf dem virtuellen Computer. Dann lauscht und antwortet er nur mit einer HTTP-OK-Antwort 200, wenn die Instanz bereit ist (also nicht gerade beschäftigt oder angehalten ist oder recycelt wird).

Weitere Informationen finden Sie unter [Konfigurieren der Dienstdefinitionsdatei (CSDEF) für Integritätstests](https://msdn.microsoft.com/library/azure/ee758710.aspx) und [Erste Schritte zum Erstellen eines Lastenausgleich mit Internetzugriff für Clouddienste](load-balancer-get-started-internet-classic-cloud.md#check-load-balancer-health-status-for-cloud-services).

### <a name="what-makes-a-guest-agent-probe-mark-an-instance-as-unhealthy"></a>Was kann einen Gast-Agent-Test veranlassen, eine Instanz als fehlerhaft zu markieren?

Wenn der Gast-Agent nicht mit dem HTTP-OK-Code 200 antwortet, kennzeichnet der Lastenausgleich die Instanz als nicht reagierend und sendet keinen Datenverkehr mehr an diese Instanz. Das Lastenausgleichsmodul pingt die Instanz weiterhin. Wenn der Gast-Agent mit dem HTTP-Code 200 antwortet, sendet der Lastenausgleich wieder Datenverkehr an diese Instanz.

Wenn Sie eine Webrolle verwenden, wird der Websitecode in der Regel in „w3wp.exe“ ausgeführt. Dieses Programm wird nicht von der Azure-Fabric oder vom Gast-Agent überwacht. Das bedeutet, dass Fehler in „w3wp.exe“ (z. B. HTTP 500-Antworten) nicht an den Gast-Agent gemeldet werden und dass der Lastenausgleich diese Instanz nicht aus der Rotation entfernt.

### <a name="http-custom-probe"></a>Benutzerdefinierter HTTP-Test

Der benutzerdefinierte HTTP-Load Balancer-Test erhält Vorrang vor dem Standard-Gast-Agent-Test, sodass Sie Ihre eigene Logik zum Bestimmen der Integrität der Rolleninstanz erstellen können. Der Lastenausgleich testet Ihren Endpunkt standardmäßig alle 15 Sekunden. Die Instanz wird in die Lastenausgleichrotation einbezogen, wenn sie innerhalb des Zeitlimits (standardmäßig 31 Sekunden) mit HTTP 200 reagiert.

Dies kann für die Implementierung Ihrer eigenen Logik zum Entfernen von Instanzen aus der Lastenausgleichrotation nützlich sein. Sie könnten z. B. eine Instanz entfernen, wenn sie über 90 % CPU beansprucht und einen anderen Status als 200 zurückgibt. Wenn Ihre Webrollen „w3wp.exe“ verwenden, bedeutet dies auch eine automatische Überwachung der Website, da Fehler im Websitecode einen Nicht-200-Status an den Lastenausgleichtest zurückgeben.

> [!NOTE]
> Der benutzerdefinierte HTTP-Test unterstützt nur relative Pfade und das HTTP-Protokoll. HTTPS wird nicht unterstützt.

### <a name="what-makes-an-http-custom-probe-mark-an-instance-as-unhealthy"></a>Was kann einen benutzerdefinierten HTTP-Test veranlassen, eine Instanz als fehlerhaft zu markieren?

* Die HTTP-Anwendung gibt einen anderen HTTP-Antwortcode als 200 zurück (z. B. 403, 404 oder 500). Hierbei handelt es sich um eine positive Bestätigung, dass die Anwendungsinstanz sofort außer Betrieb genommen werden sollte.
* Der HTTP-Server reagiert nach dem Timeoutzeitraum nicht mehr. Je nach dem festgelegten Timeoutwert kann dies bedeuten, dass mehrere Testanforderungen unbeantwortet bleiben, bevor der Test als nicht ausgeführt markiert wird (d. h. bevor SuccessFailCount-Tests gesendet werden).
* Der Server schließt die Verbindung durch „TCP Reset“.

### <a name="tcp-custom-probe"></a>Benutzerdefinierter TCP-Test

TCP-Tests leiten eine Verbindung über einen Drei-Wege-Handshake mit dem definierten Port ein.

### <a name="what-makes-a-tcp-custom-probe-mark-an-instance-as-unhealthy"></a>Was kann einen benutzerdefinierten TCP-Test veranlassen, eine Instanz als fehlerhaft zu markieren?

* Der TCP-Server reagiert nach dem Timeoutzeitraum nicht mehr. Wann der Test als nicht ausgeführt markiert wird, hängt von der Anzahl fehlerhafter Testanforderungen ab, die unbeantwortet bleiben können, bevor der Test als nicht ausgeführt gilt.
* Der Test empfängt ein TCP Reset von der Rolleninstanz.

Weitere Informationen zum Konfigurieren eines HTTP-Integritätstests oder eines TCP-Tests finden Sie unter [Erste Schritte zum Erstellen eines Load Balancers mit Internetzugriff in Resource Manager unter Verwendung von PowerShell](load-balancer-get-started-internet-arm-ps.md).

## <a name="add-healthy-instances-back-into-load-balancer-rotation"></a>Erneutes Hinzufügen fehlerfreier Instanzen zur Lastenausgleichrotation

TCP- und HTTP-Tests werden als fehlerfrei eingestuft und markieren die Rolleninstanz als fehlerfrei, wenn:

* Beim ersten Start der VM erhält der Lastenausgleich einen positiven Test.
* Der Wert von „SuccessFailCount“ (oben beschrieben) definiert die Anzahl erfolgreicher Tests, die erforderlich sind, um die Rolleninstanz als fehlerfrei zu markieren. Wenn eine Rolleninstanz entfernt wurde, muss die Anzahl der erfolgreichen, aufeinanderfolgenden Tests gleich oder größer sein als der Wert von SuccessFailCount, damit die Rolleninstanz als aktiv markiert wird.

> [!NOTE]
> Wenn die Integrität einer Rolleninstanz schwankt, wartet der Lastenausgleich länger, ehe die Rolleninstanz wieder in den fehlerfreien Zustand versetzt wird. Dies erfolgt zum Schutz der Benutzer und Infrastruktur über die Richtlinie.

## <a name="use-log-analytics-for-load-balancer"></a>Verwenden der Protokollanalyse für den Load Balancer

Mit der [Protokollanalyse für den Load Balancer](load-balancer-monitor-log.md) können Sie den Testintegritätsstatus und die Testanzahl überprüfen. Die Protokollierung kann mit Power BI oder Azure Operational Insights verwendet werden, um Statistiken zum Integritätsstatus des Load Balancers bereitzustellen.



<!--HONumber=Nov16_HO3-->



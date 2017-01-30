---
title: "Übersicht über die Systemüberwachung für Azure Application Gateway | Microsoft Docs"
description: "Weitere Informationen zu den Überwachungsfunktionen in Azure Application Gateway"
services: application-gateway
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 7eeba328-bb2d-4d3e-bdac-7552e7900b7f
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/14/2016
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: e982d29f76d521720eea7cbb56c5084572f4a542
ms.openlocfilehash: 3286a6e98b52f2ce6f173f79ae69f0b86ab2179a


---

# <a name="application-gateway-health-monitoring-overview"></a>Systemüberwachung des Application Gateways – Übersicht

Azure Application Gateway überwacht standardmäßig die Integrität aller Ressourcen in seinem Back-End-Pool und entfernt automatisch alle als fehlerhaft geltenden Ressourcen aus dem Pool. Application Gateway überwacht die fehlerhaften Instanzen weiterhin und fügt sie dem fehlerfreien Back-End-Pool hinzu, sobald sie verfügbar sind und auf Zustandsüberprüfungen reagieren. Application Gateway sendet die Integritätstests über denselben Port, der in den HTTP-Einstellungen des Back-Ends festgelegt wurde. Durch diese Konfiguration wird sichergestellt, dass derselbe Port getestet wird, den der Kunde für die Verbindung mit dem Back-End verwenden würde.

![Beispiel für einen Application Gateway-Test][1]

Zusätzlich zur Nutzung der standardmäßigen Überwachung der Integritätsüberprüfung können Sie die Integritätsüberprüfung auch an die Anforderungen Ihrer Anwendung anpassen. In diesem Artikel werden sowohl standardmäßige als auch benutzerdefinierte Integritätstests behandelt.

## <a name="default-health-probe"></a>Standardmäßige Integritätsüberprüfung

Ein Anwendungsgateway konfiguriert automatisch eine standardmäßige Integritätsüberprüfung, wenn Sie keine benutzerdefinierte Überprüfungskonfiguration einrichten. Das Verhalten der Überwachung funktioniert durch das Erstellen einer HTTP-Anforderung an die für den Back-End-Pool konfigurierten IP-Adressen. Bei Standardüberprüfungen wird auch HTTPS für den Integritätstest der Back-Ends verwendet, wenn die HTTP-Einstellungen des Back-Ends für HTTPS konfiguriert sind.

Beispiel: Sie konfigurieren Ihr Application Gateway für die Verwendung der Back-End-Server A, B und C zum Empfang von HTTP-Netzwerkdatenverkehr an Port 80. Die standardmäßige Integritätsüberwachung testet die drei Server alle 30 Sekunden auf eine fehlerfreie HTTP-Antwort. Eine fehlerfreie HTTP-Antwort weist einen [Statuscode](https://msdn.microsoft.com/library/aa287675.aspx) zwischen 200 und 399 auf.

Wenn die Standardüberprüfung für Server A fehlschlägt, entfernt das Application Gateway sie aus dem Back-End-Pool, und der Netzwerkdatenverkehr an diesen Server wird angehalten. Die Standardüberprüfung führt weiterhin alle 30 Sekunden eine Überprüfung für Server A aus. Wenn Server A erfolgreich auf eine Anforderung einer standardmäßigen Integritätsüberprüfung antwortet, wird er dem Back-End-Pool wieder als fehlerfrei hinzugefügt, und der Datenverkehr an den Server startet erneut.

### <a name="default-health-probe-settings"></a>Einstellungen für die standardmäßige Integritätsüberprüfung

| Überprüfungseigenschaft | Wert | Beschreibung |
| --- | --- | --- |
| Überprüfungs-URL |http://127.0.0.1:\<Port\>/ |URL-Pfad |
| Intervall |30 |Überprüfungsintervall in Sekunden |
| Zeitüberschreitung |30 |Zeitüberschreitung der Überprüfung in Sekunden |
| Fehlerhafter Schwellenwert |3 |Anzahl der Wiederholungsversuche der Überprüfung Der Back-End-Server wird als außer Betrieb markiert, nachdem die Anzahl der aufeinanderfolgenden fehlgeschlagenen Überprüfungen den fehlerhaften Schwellenwert erreicht. |

> [!NOTE]
> Dieser Port stimmt mit dem Port in den HTTP-Einstellungen des Back-Ends überein.

Der Standardtest untersucht nur „http://127.0.0.1:\<Port\>“, um den Integritätsstatus zu bestimmen. Wenn Sie die Integritätsüberprüfung für eine benutzerdefinierte URL konfigurieren oder andere Einstellungen ändern möchten, müssen Sie benutzerdefinierte Überprüfungen wie in den folgenden Schritten beschrieben verwenden:

## <a name="custom-health-probe"></a>Benutzerdefinierte Integritätsüberprüfung

Benutzerdefinierte Überprüfungen ermöglichen Ihnen eine präzisere Kontrolle über die Überwachung des Systemzustands. Bei Verwendung von benutzerdefinierten Überprüfungen können Sie das Überprüfungsintervall, die URL und den zu überprüfenden Pfad konfigurieren und festlegen, wie viele fehlerhafte Antworten akzeptiert werden, bevor die Back-End-Pool-Instanz als fehlerhaft gekennzeichnet wird.

### <a name="custom-health-probe-settings"></a>Einstellungen für die benutzerdefinierte Integritätsüberprüfung

Die folgende Tabelle enthält Definitionen der Eigenschaften eines benutzerdefinierten Integritätstests.

| Überprüfungseigenschaft | Beschreibung |
| --- | --- |
| Name |Name der Überprüfung. Dieser Name wird verwendet, um in den Back-End-HTTP-Einstellungen auf die Überprüfung zu verweisen. |
| Protocol |Das zum Senden der Überprüfung verwendete Protokoll. Für die Überprüfung wird das in den HTTP-Einstellungen des Back-Ends festgelegte Protokoll verwendet. |
| Host |Hostname zum Senden der Überprüfung Nur relevant, wenn in Application Gateway mehrere Standorte konfiguriert sind. Andernfalls verwenden Sie 127.0.0.1. Dieser Wert entspricht nicht dem Hostnamen des virtuellen Computers. |
| Pfad |Relativer Pfad der Überprüfung. Der gültige Pfad beginnt mit „/“. |
| Intervall |Überprüfungsintervall in Sekunden Dieser Wert ist das Zeitintervall zwischen zwei aufeinanderfolgenden Überprüfungen. |
| Zeitüberschreitung |Zeitüberschreitung der Überprüfung in Sekunden. Die Überprüfung wird als fehlerhaft markiert, wenn innerhalb des Zeitraums für die Zeitüberschreitung keine gültige Antwort empfangen wird.  |
| Fehlerhafter Schwellenwert |Anzahl der Wiederholungsversuche der Überprüfung Der Back-End-Server wird als außer Betrieb markiert, nachdem die Anzahl der aufeinanderfolgenden fehlgeschlagenen Überprüfungen den fehlerhaften Schwellenwert erreicht. |

> [!IMPORTANT]
> Falls Application Gateway für einen einzelnen Standort konfiguriert ist, muss der Hostname standardmäßig als „127.0.0.1“ angegeben werden, sofern in der benutzerdefinierten Überprüfung nichts anderes konfiguriert ist.
> Als Verweis wird ein Test an \<Protokoll\>://\<Host\>:\<Port\>\<Pfad\> gesendet. Dieser Port ist immer der gleiche Port, der in den HTTP-Einstellungen des Back-Ends definiert ist.

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie sich mit der Systemüberwachung von Application Gateway vertraut gemacht haben, können Sie einen [benutzerdefinierten Integritätstest](application-gateway-create-probe-portal.md) im Azure-Portal oder einen [benutzerdefinierten Integritätstest](application-gateway-create-probe-ps.md) mit PowerShell und dem Azure Resource Manager-Bereitstellungsmodell konfigurieren.

[1]: ./media/application-gateway-probe-overview/appgatewayprobe.png



<!--HONumber=Dec16_HO3-->



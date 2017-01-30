---
title: "Erstellen eines benutzerdefinierten Tests für ein Anwendungsgateway über das Portal | Microsoft Docs"
description: "Erfahren Sie, wie Sie einen benutzerdefinierten Test für ein Anwendungsgateway über das Portal erstellen."
services: application-gateway
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: 33fd5564-43a7-4c54-a9ec-b1235f661f97
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/13/2016
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: aaf13418331f29287399621cb911e4b9f5b33dc0
ms.openlocfilehash: 0fafaff220dfb5d224bc5b57b630f8b2912155f7


---
# <a name="create-a-custom-probe-for-application-gateway-by-using-the-portal"></a>Erstellen eines benutzerdefinierten Tests für ein Anwendungsgateway über das Portal
> [!div class="op_single_selector"]
> * [Azure-Portal](application-gateway-create-probe-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-probe-ps.md)
> * [Klassische Azure PowerShell](application-gateway-create-probe-classic-ps.md)

[!INCLUDE [azure-probe-intro-include](../../includes/application-gateway-create-probe-intro-include.md)]

## <a name="scenario"></a>Szenario

Das folgende Szenario führt Sie durch die Erstellung eines benutzerdefinierten Integritätstests in einem vorhandenen Anwendungsgateway.
Für dieses Szenario wird davon ausgegangen, dass Sie die Schritte unter [Erstellen eines Anwendungsgateways](application-gateway-create-gateway-portal.md)bereits durchgeführt haben.

## <a name="a-namecreateprobeacreate-the-probe"></a><a name="createprobe"></a>Erstellen des Tests

Tests werden in einem aus zwei Schritten bestehenden Prozess im Portal konfiguriert. Im ersten Schritt wird der Test erstellt. Anschließend fügen Sie den Test den Back-End-HTTP-Einstellungen des Anwendungsgateways hinzu.

### <a name="step-1"></a>Schritt 1

Navigieren Sie zum [Azure-Portal](http://portal.azure.com), und wählen Sie ein vorhandenes Anwendungsgateway aus.

![Übersicht über Application Gateway][1]

### <a name="step-2"></a>Schritt 2

Klicken Sie auf **Tests** und dann auf die Schaltfläche **Hinzufügen**, um einen Test hinzuzufügen.

![Hinzufügen eines Blatts für einen Test mit bereitgestellten Informationen][2]

### <a name="step-3"></a>Schritt 3

Stellen Sie die erforderlichen Informationen für den Test bereit, und klicken Sie anschließend auf **OK**.

* **Name**: Dieser Wert ist der Anzeigename für den Test, auf den Sie über das Portal zugreifen können.
* **Host**: Dieser Wert ist der Hostname, der für den Test verwendet wird. Nur relevant, wenn in Application Gateway mehrere Standorte konfiguriert sind. Andernfalls verwenden Sie 127.0.0.1. Dieser Wert entspricht nicht dem Hostnamen des virtuellen Computers.
* **Pfad** : Dies ist der Rest der vollständigen URL für den benutzerdefinierten Test. Ein gültiger Pfad beginnt mit „/“.
* **Intervall (Sek.)** : Legen Sie fest, wie oft der Test ausgeführt werden soll, um die Integrität zu prüfen. Es wird nicht empfohlen, einen Wert unter 30 Sekunden einzustellen.
* **Timeout (Sek.)** : Legen Sie fest, wie lange der Test warten soll, bis ein Timeout auftritt. Das Timeoutintervall muss lang genug sein, damit ein HTTP-Aufruf erfolgen und sichergestellt werden kann, dass die Integritätsseite für das Back-End verfügbar ist.
* **Fehlerschwellenwert** : Dies ist die Anzahl erfolgloser Versuche, nach denen der Test als „fehlerhaft“ eingestuft wird. Ein Schwellenwert von 0 bedeutet, dass das Back-End bei einer fehlerhaften Integritätsprüfung sofort als fehlerhaft eingestuft wird.

> [!IMPORTANT]
> Der Hostname ist nicht identisch mit dem Servernamen. Dieser Wert ist der Name des virtuellen Hosts, der auf dem Anwendungsserver ausgeführt wird. Der Test wird an diese Adresse gesendet: http://(Hostname):(Port aus HTTP-Einstellungen)/urlPath.

![Konfigurationseinstellungen für Test][3]

## <a name="add-probe-to-the-gateway"></a>Hinzufügen des Tests zum Gateway

Nachdem der Test erstellt wurde, können Sie ihn jetzt zum Gateway hinzufügen. Testeinstellungen werden in den Back-End-HTTP-Einstellungen des Anwendungsgateways festgelegt.

### <a name="step-1"></a>Schritt 1

Klicken Sie auf die **HTTP-Einstellungen** des Anwendungsgateways, und klicken Sie auf die aktuellen Back-End-HTTP-Einstellungen im Fenster, um das Blatt „Konfiguration“ zu öffnen.

![Fenster mit HTTPS-Einstellungen][4]

### <a name="step-2"></a>Schritt 2

Klicken Sie auf dem Blatt mit den Einstellungen für **appGatewayBackEndHttp** auf **Benutzerdefinierten Test verwenden**, und wählen Sie den Test aus, den Sie im Abschnitt [Erstellen des Tests](#createprobe) erstellt haben.
Wenn Sie fertig sind, klicken Sie auf **OK** , um die Einstellungen anzuwenden.

![Blatt mit appgatewaybackend-Einstellungen][5]

Der Standardtest prüft den Standardzugriff auf die Webanwendung. Nachdem nun ein benutzerdefinierter Test erstellt wurde, verwendet das Anwendungsgateway den benutzerdefinierten Pfad, um die Integrität des ausgewählten Back-Ends zu überwachen. Das Anwendungsgateway überprüft die im Test angegebene Datei basierend auf den definierten Kriterien. Wenn der Aufruf an „host:Port/path“ keine HTTP-Statusantwort 200 (OK) zurückgibt, wird der Server aus der Rotation herausgenommen, nachdem der Fehlerschwellenwert erreicht ist. Die fehlerhafte Instanz wird weiterhin überprüft, um festzustellen, wann die Integrität wiederhergestellt ist. Sobald die Instanz dem fehlerfreien Serverpool wieder hinzugefügt wurde, wird wieder Datenverkehr an die Instanz übertragen, und die Instanz wird wie üblich in den vom Benutzer angegebenen Intervallen getestet.

## <a name="next-steps"></a>Nächste Schritte

Informationen zum Konfigurieren der SSL-Auslagerung mit Azure Application Gateway finden Sie unter [Konfigurieren der SSL-Auslagerung](application-gateway-ssl-portal.md).

[1]: ./media/application-gateway-create-probe-portal/figure1.png
[2]: ./media/application-gateway-create-probe-portal/figure2.png
[3]: ./media/application-gateway-create-probe-portal/figure3.png
[4]: ./media/application-gateway-create-probe-portal/figure4.png
[5]: ./media/application-gateway-create-probe-portal/figure5.png



<!--HONumber=Dec16_HO3-->



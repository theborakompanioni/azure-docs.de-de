---
title: "Öffnen der für eine Anwendungsproxyanwendung erforderlichen Firewallports | Microsoft-Dokumentation"
description: "Ermitteln, welche Ports für die ordnungsgemäße Funktionsweise des Azure AD-Anwendungsproxys geöffnet werden müssen"
services: active-directory
documentationcenter: 
author: ajamess
manager: femila
ms.assetid: 
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/04/2017
ms.author: asteen
translationtype: Human Translation
ms.sourcegitcommit: 0d6f6fb24f1f01d703104f925dcd03ee1ff46062
ms.openlocfilehash: 4cbffd6627bccc96400ec6de103d4d7fc5d77950
ms.lasthandoff: 04/17/2017


---

# <a name="how-to-open-the-firewall-ports-required-for-an-application-proxy-application"></a>Öffnen der für eine Anwendungsproxyanwendung erforderlichen Firewallports

Eine vollständige Liste der erforderlichen Ports und die Funktion der einzelnen Ports finden Sie im Abschnitt „Voraussetzungen“ der [Anwendungsproxy-Dokumentation](https://docs.microsoft.com/azure/active-directory/active-directory-application-proxy-enable). Beachten Sie, dass der Anwendungsproxy nur ausgehende Ports verwendet.

Sie können auch überprüfen, ob alle erforderlichen Ports geöffnet sind, indem Sie das [Testtool für Connectorports](https://aadap-portcheck.connectorporttest.msappproxy.net/) über Ihr lokales Netzwerk öffnen. Eine größere Anzahl grüner Häkchen bedeutet eine größere Resilienz. 

## <a name="app-proxy-regions"></a>App-Proxyregionen

Wir arbeiten an einer Möglichkeit, um Ihnen mitzuteilen, welche dieser Regionen für Sie grün sein müssen. Stellen Sie für den Moment sicher, dass sie alle grün sind. Unabhängig von der Region, in der Sie sich befinden, ist auch „USA, Mitte“ erforderlich.

Stellen Sie Folgendes sicher, damit Ihnen das Tool die richtigen Ergebnisse liefert:

-   Öffnen Sie das Tool auf einem Server, auf dem Sie den Connector installiert haben, in einem Browser.

-   Stellen Sie sicher, dass alle Proxys oder Firewalls, die für den Connector relevant sind, auch auf diese Seite angewendet werden. Wechseln Sie dazu in Internet Explorer zu **Einstellungen** -&gt; **Internetoptionen** -&gt; **Verbindungen** -&gt; **LAN-Einstellungen**. Auf dieser Seite wird das Feld „Proxyserver für das LAN verwenden“ angezeigt. Aktivieren Sie dieses Kontrollkästchen, und fügen Sie die Proxyadresse in das Feld „Adresse“ ein.

## <a name="next-steps"></a>Nächste Schritte
[Grundlegendes zu Azure AD-Anwendungsproxyconnectors](application-proxy-understand-connectors.md)


---
title: Hosten mehrerer Websites mit Azure Application Gateway| Microsoft-Dokumentation
description: "Auf dieser Seite finden Sie Anweisungen zum Konfigurieren eines vorhandenen Azure-Anwendungsgateways zum Hosten mehrerer Webanwendungen über dasselbe Gateway mit dem Azure-Portal."
documentationcenter: na
services: application-gateway
author: georgewallace
manager: timlt
editor: tysonn
ms.assetid: 95f892f6-fa27-47ee-b980-7abf4f2c66a9
ms.service: application-gateway
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/23/2017
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: fd5960a4488f2ecd93ba117a7d775e78272cbffd
ms.openlocfilehash: 90b7e2f7f5327684f173bd7e10f21e65bea8fbe7


---
# <a name="configure-an-existing-application-gateway-for-hosting-multiple-web-applications"></a>Konfigurieren eines vorhandenen Anwendungsgateways zum Hosten mehrerer Webanwendungen

> [!div class="op_single_selector"]
> * [Azure-Portal](application-gateway-create-multisite-portal.md)
> * [Azure Resource Manager PowerShell](application-gateway-create-multisite-azureresourcemanager-powershell.md)
> 
> 

Das Hosten mehrerer Websites ermöglicht es Ihnen, mehr als eine Webanwendung über dasselbe Anwendungsgateway bereitzustellen. Das Prinzip basiert auf einem Hostheader in der eingehenden HTTP-Anforderung, anhand dessen bestimmt wird, welcher Listener den Datenverkehr empfängt. Der Listener leitet dann den Datenverkehr an den in der Regeldefinition des Gateways konfigurierten Back-End-Pool. Bei SSL-fähigen Webanwendungen wählt das Anwendungsgateway den passenden Listener für den Webdatenverkehr anhand der SNI-Erweiterung (Servernamensanzeige) aus. Ein gängiges Szenario für das Hosten mehrerer Websites ist der Lastenausgleich von Anforderungen für verschiedene Webdomänen auf verschiedene Back-End-Serverpools. Auf ähnliche Weise können auch mehrere Unterdomänen derselben Stammdomäne im selben Anwendungsgateway gehostet werden.

## <a name="scenario"></a>Szenario

Im folgenden Beispiel verarbeitet das Anwendungsgateway Datenverkehr für „contoso.com“ und „fabrikam.com“ mit zwei Back-End-Serverpools: dem Contoso-Serverpool und dem Fabrikam-Serverpool. Mit einem ähnlichen Setup können auch Unterdomänen wie „app.contoso.com“ und „blog.contoso.com“ gehostet werden.

![Szenario mit mehreren Standorten][multisite]

## <a name="before-you-begin"></a>Voraussetzungen

Dieses Szenario fügt einem vorhandenen Anwendungsgateway Multisite-Unterstützung hinzu. Damit dieses Szenario vervollständigt werden kann, muss ein vorhandenes Anwendungsgateway zur Konfiguration verfügbar sein. Lesen Sie unter [Erstellen eines Anwendungsgateways mit dem Portal](application-gateway-create-gateway-portal.md), wie ein einfaches Anwendungsgateway im Portal erstellt wird.

Folgende Schritte sind zum Aktualisieren des Anwendungsgateways erforderlich:

1. Erstellen von Back-End-Pools für jede Website.
2. Erstellen eines neuen Listeners für jede Website, die das Anwendungsgateway unterstützt
3. Erstellen von Regeln zum Zuordnen der einzelnen Listener zum entsprechenden Back-End.

## <a name="requirements"></a>Anforderungen

* **Back-End-Serverpool:** Die Liste der IP-Adressen der Back-End-Server. Die aufgelisteten IP-Adressen sollten entweder dem Subnetz des virtuellen Netzwerks angehören oder eine öffentliche IP-Adresse/VIP sein. Der FQDN kann ebenfalls verwendet werden.
* **Einstellungen für den Back-End-Serverpool:** Jeder Pool weist Einstellungen wie Port, Protokoll und cookiebasierte Affinität auf. Diese Einstellungen sind an einen Pool gebunden und gelten für alle Server innerhalb des Pools.
* **Front-End-Port:** Dieser Port ist der öffentliche Port, der im Application Gateway geöffnet ist. Datenverkehr erreicht diesen Port und wird dann an einen der Back-End-Server umgeleitet.
* **Listener:** Der Listener verfügt über einen Front-End-Port, ein Protokoll („Http“ oder „Https“; jeweils unter Beachtung der Groß-/Kleinschreibung) und den Namen des SSL-Zertifikats (falls die SSL-Auslagerung konfiguriert wird). Bei Anwendungsgateways, die mehrere Websites hosten können, werden zudem der Hostname und SNI-Indikatoren hinzugefügt.
* **Regel:** Mit der Regel werden der Listener und der Back-End-Serverpool verbunden, und es wird definiert, an welchen Back-End-Serverpool der Datenverkehr geleitet werden soll, wenn er einen bestimmten Listener erreicht.
* **Zertifikate:** Für jeden Listener ist ein eindeutiges Zertifikat erforderlich, in diesem Beispiel werden 2 Listener für Multisite erstellt. Zwei PFX-Zertifikate und Kennwörter müssen dafür erstellt werden.

## <a name="create-back-end-pools-for-each-site"></a>Erstellen von Back-End-Pools für jede Website

Für jede Website, die das Anwendungsgateway unterstützt, ist ein Back-End-Pool erforderlich. In diesem Fall werden 2 erstellt, einer für contoso11.com und einer für fabrikam11.com.

### <a name="step-1"></a>Schritt 1

Navigieren Sie zu einem vorhandenen Anwendungsgateway im Azure-Portal (https://portal.azure.com). Wählen Sie **Back-End-Pools** und klicken Sie auf **Hinzufügen**

![Hinzufügen von Back-End-Pools][7]

### <a name="step-2"></a>Schritt 2

Geben Sie die Informationen für den Back-End-Pool **pool1** an, die IP-Adressen oder vollqualifizierte Domänennamen für die Back-End-Server, und klicken Sie auf **OK**

![Back-End-Pool pool1-Einstellungen][8]

### <a name="step-3"></a>Schritt 3

Klicken Sie auf dem Back-End-Pools-Blatt **Hinzufügen**, um einen zusätzlichen Back-End-Pool **pool2** hinzuzufügen, wobei die IP-Adressen oder vollqualifizierte Domänennamen für die Back-End-Server hinzugefügt werden, und klicken Sie auf **OK**

![Back-End-Pool pool2-Einstellungen][9]

## <a name="create-listeners-for-each-back-end"></a>Erstellen von Listeners für jeden Back-End

Application Gateway verwendet HTTP 1.1-Hostheader, um mehrere Websites an der gleichen öffentlichen IP-Adresse und dem gleichen Port zu hosten. Der im Portal erstellte Basis-Listener enthält diese Eigenschaft nicht.

### <a name="step-1"></a>Schritt 1

Klicken Sie auf **Listeners** im vorhandenen Anwendungsgateway und auf **Multisite**, um den ersten Listener hinzuzufügen.

![Listeners-Übersichtsblatt][1]

### <a name="step-2"></a>Schritt 2

Geben Sie die Informationen für den Listener ein. In diesem Beispiel ist SSL-Beendigung konfiguriert. Erstellen Sie einen neuen Front-End-Port. Laden Sie das PFX-Zertifikat für die SSL-Beendigung hoch. Der einzige Unterschied auf diesem Blatt im Vergleich zum Standard-Basis-Listener-Blatt ist der Hostname.

![Listener-Eigenschaftsblatt][2]

### <a name="step-3"></a>Schritt 3

Klicken Sie auf **Multisite**, um einen weiteren Listener zu erstellen, wie im vorherigen Schritt für die zweite Website beschrieben. Achten Sie darauf, dass Sie ein anderes Zertifikat für den zweiten Listener verwenden. Der einzige Unterschied auf diesem Blatt im Vergleich zum Standard-Basis-Listener-Blatt ist der Hostname. Geben Sie die Informationen für den Listener ein und klicken Sie auf **OK**.

![Listener-Eigenschaftsblatt][3]

> [!NOTE]
> Das Erstellen von Listeners im Azure-Portal für Anwendungsgateways ist eine langwierige Aufgabe, die möglicherweise einige Zeit in Anspruch nimmt, um die beiden Listener in diesem Szenario zu erstellen. Nach Abschluss zeigt das Portal die Listener wie in der folgenden Abbildung dargestellt an:

![Listener-Übersicht][4]

## <a name="create-rules-to-map-listeners-to-backend-pools"></a>Erstellen von Regeln zum Zuordnen von Listeners zu Back-End-Pools

### <a name="step-1"></a>Schritt 1

Navigieren Sie zu einem vorhandenen Anwendungsgateway im Azure-Portal (https://portal.azure.com). Wählen Sie **Regeln**, wählen Sie die vorhandene Standardregel **rule1**, und klicken Sie auf **Bearbeiten**.

### <a name="step-2"></a>Schritt 2

Füllen Sie das Regelblatt wie in der folgenden Abbildung dargestellt aus. Wählen Sie den ersten Listener und den ersten Pool. Klicken Sie zum Abschluss auf **Speichern**.

![Bearbeiten einer vorhandenen Regel][6]

### <a name="step-3"></a>Schritt 3

Klicken Sie auf **Basic rule** (Basisregel), um die&2;. Regel zu erstellen. Füllen Sie das Formular mit dem zweiten Listener und dem zweiten Back-End-Pool aus, und klicken Sie zum Speichern auf **OK**.

![Blatt „einfache Regel hinzufügen“][10]

Dieses Szenario schließt die Konfiguration eines vorhandenen Anwendungsgateways mit Unterstützung für mehrere Websites über das Azure-Portal ab.

## <a name="next-steps"></a>Nächste Schritte

Erfahren Sie unter [Web Application Firewall für Application Gateway](application-gateway-webapplicationfirewall-overview.md), wie Sie Ihre Websites schützen.

<!--Image references-->
[1]: ./media/application-gateway-create-multisite-portal/figure1.png
[2]: ./media/application-gateway-create-multisite-portal/figure2.png
[3]: ./media/application-gateway-create-multisite-portal/figure3.png
[4]: ./media/application-gateway-create-multisite-portal/figure4.png
[5]: ./media/application-gateway-create-multisite-portal/figure5.png
[6]: ./media/application-gateway-create-multisite-portal/figure6.png
[7]: ./media/application-gateway-create-multisite-portal/figure7.png
[8]: ./media/application-gateway-create-multisite-portal/figure8.png
[9]: ./media/application-gateway-create-multisite-portal/figure9.png
[10]: ./media/application-gateway-create-multisite-portal/figure10.png
[multisite]: ./media/application-gateway-create-multisite-portal/multisite.png



<!--HONumber=Jan17_HO4-->



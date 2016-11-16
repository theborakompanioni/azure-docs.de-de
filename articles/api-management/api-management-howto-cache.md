---
title: "Hinzufügen der Zwischenspeicherung zum Verbessern der Leistung in Azure API Management | Microsoft Docs"
description: "Erfahren Sie, wie Sie Latenz, Bandbreitennutzung und Webdienstauslastung für API Management-Dienstaufrufe verbessern."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 740f6a27-8323-474d-ade2-828ae0c75e7a
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 10/25/2016
ms.author: sdanie
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: c8cdb37fceb7b598c92b7b3cd41655c87c74e639


---
# <a name="add-caching-to-improve-performance-in-azure-api-management"></a>Hinzufügen der Zwischenspeicherung zum Verbessern der Leistung in Azure API Management
Operationen in API Management können für das Zwischenspeichern von Antworten konfiguriert werden. Das Zwischenspeichern von Antworten kann API-Latenz, Bandbreitennutzung und Webdienstlast für Daten, die sich eher selten ändern, drastisch senken.

In diesem Leitfaden wird gezeigt, wie Sie die Zwischenspeicherung von Antworten für Ihre API hinzufügen und Richtlinien für Operationen in der Echo-Beispiel-API konfigurieren. Um die Zwischenspeicherung in Aktion zu sehen, können Sie die Operation im Entwicklerportal aufrufen.

> [!NOTE]
> Informationen zum Zwischenspeichern von Elementen nach Schlüssel mithilfe von Richtlinienausdrücken finden Sie unter [Benutzerdefiniertes Zwischenspeichern in Azure API Management](api-management-sample-cache-by-key.md).
> 
> 

## <a name="prerequisites"></a>Voraussetzungen
Um die Schritte in diesem Leitfaden ausführen zu können, müssen Sie über eine API Management-Dienstinstanz mit einer konfigurierten API und einem konfigurierten Produkt verfügen. Falls Sie noch keine API Management-Dienstinstanz erstellt haben, finden Sie weitere Informationen im Abschnitt [Erstellen einer API Management-Dienstinstanz][Erstellen einer API Management-Dienstinstanz] im Tutorial [Erste Schritte mit Azure API Management][Erste Schritte mit Azure API Management].

## <a name="configure-caching"> </a>Konfigurieren einer Operation für die Zwischenspeicherung
In diesem Schritt prüfen Sie die Einstellungen der Zwischenspeicherung für die **GET Resource (cached)** -Operation der Echo-Beispiel-API.

> [!NOTE]
> Jede API Management-Dienstinstanz enthält eine vorkonfigurierte Echo-API, die Sie für Tests und erste Schritte mit API Management nutzen können. Weitere Informationen finden Sie unter [Erste Schritte mit Azure API Management][Erste Schritte mit Azure API Management].
> 
> 

Klicken Sie zunächst im Azure-Portal für Ihren API Management-Dienst auf **Entwicklerportal**. Daraufhin gelangen Sie zum API Management-Herausgeberportal.

![Herausgeberportal][api-management-management-console]

Klicken Sie auf der linken Seite im Menü **API Management** auf **APIs**, und klicken Sie anschließend auf **Echo API**.

![Echo API][api-management-echo-api]

Klicken Sie auf die Registerkarte **Operationen**, und klicken Sie dann auf die Operation **GET Resource (cached)** in der Liste der **Operationen**.

![Operationen der Echo API][api-management-echo-api-operations]

Klicken Sie auf die Registerkarte **Zwischenspeichern** , um die Einstellungen der Zwischenspeicherung für diese Operation anzuzeigen.

![Registerkarte "Zwischenspeichern"][api-management-caching-tab]

Aktivieren Sie das Kontrollkästchen **Aktivieren** , um die Zwischenspeicherung für eine Operation zu aktivieren. In diesem Beispiel ist die Zwischenspeicherung aktiviert.

Jedes Operationsergebnis erhält einen Schlüssel anhand der Felder, die in **Nach Abfrageparametern variieren** und **Nach Headern variieren** ausgewählt wurden. Falls Sie mehrere Antworten anhand von Abfrageparametern oder Headern zwischenspeichern möchten, können Sie dies in diesen beiden Feldern konfigurieren.

**Dauer** gibt das Ablaufintervall der gespeicherten Antworten an. In diesem Beispiel ist das Intervall **3600** Sekunden, was einer Stunde entspricht.

Mit der Konfiguration für das Zwischenspeichern in diesem Beispiel liefert die erste Anforderung an die **GET Resource (cached)** -Operation eine Antwort vom Back-End-Dienst zurück. Diese Antwort wird zwischengespeichert und erhält einen Schlüssel anhand der angegebenen Header und Abfrageparameter. Bei nachfolgenden Aufrufen der Operation mit denselben Parametern wird die zwischengespeicherte Antwort zurückgegeben, bis das Ablaufintervall abgelaufen ist.

## <a name="caching-policies"> </a>Prüfen der Richtlinien für die Zwischenspeicherung
In diesem Schritt prüfen Sie die Einstellungen der Zwischenspeicherung für die **GET Resource (cached)** -Operation der Echo-Beispiel-API.

Wenn Sie die Einstellungen der Zwischenspeicherung für eine Operation auf der Registerkarte **Zwischenspeichern** konfigurieren, werden Richtlinien für die Zwischenspeicherung für die Operation hinzugefügt. Sie können diese Richtlinien im Richtlinien-Editor anzeigen und bearbeiten.

Klicken Sie auf **Richtlinien** im Menü **API Management** auf der linken Seite, und wählen Sie dann **Echo API / GET Resource (cached)** in der Dropdownliste **Operation** aus.

![Richtlinien-Geltungsbereich Operation][api-management-operation-dropdown]

Daraufhin werden die Richtlinien für diese Operation im Richtlinien-Editor angezeigt.

![Richtlinien-Editor für API Management][api-management-policy-editor]

Die Richtliniendefinition für diese Operation enthält auch die Richtlinien für die Zwischenspeicherung, die Sie im vorherigen Schritt auf der Registerkarte **Zwischenspeichern** geprüft haben.

    <policies>
        <inbound>
            <base />
            <cache-lookup vary-by-developer="false" vary-by-developer-groups="false">
                <vary-by-header>Accept</vary-by-header>
                <vary-by-header>Accept-Charset</vary-by-header>
            </cache-lookup>
            <rewrite-uri template="/resource" />
        </inbound>
        <outbound>
            <base />
            <cache-store caching-mode="cache-on" duration="3600" />
        </outbound>
    </policies>

> [!NOTE]
> Änderungen, die Sie im Richtlinien-Editor vornehmen, werden auf der Registerkarte **Zwischenspeichern** der Operation übernommen, und umgekehrt.
> 
> 

## <a name="test-operation"> </a>Aufrufen einer Operation und Testen der Zwischenspeicherung
Um die Zwischenspeicherung in Aktion zu sehen, können Sie die Operation im Entwicklerportal aufrufen. Klicken Sie im Menü oben rechts auf **Entwicklerportal** .

![Entwicklerportal][api-management-developer-portal-menu]

Klicken Sie auf **APIs** im Hauptmenü, und wählen Sie dann **Echo API** aus.

![Echo API][api-management-apis-echo-api]

> Falls nur eine API konfiguriert oder für Ihr Konto sichtbar ist, können Sie auf APIs klicken, um direkt zu den Operationen für diese API zu gelangen.
> 
> 

Wählen Sie die Operation **GET Resource (cached)** aus, und klicken Sie dann auf **Konsole öffnen**.

![Konsole öffnen][api-management-open-console]

Mit der Konsole können Sie Operationen direkt aus dem Entwicklerportal heraus aufrufen.

![Konsole][api-management-console]

Übernehmen Sie die Standardwerte für **param1** und **param2**.

Wählen Sie den gewünschten Schlüssel in der Dropdownliste **Abonnement-Schlüssel** aus. Wenn das Konto nur über ein Abonnement verfügt, wird dieses automatisch ausgewählt.

Geben Sie **sampleheader:value1** in das Textfeld **Request headers** ein.

Klicken Sie auf **HTTP Get** , und notieren Sie sich die Antwortheader.

Geben Sie **sampleheader:value2** in das Textfeld **Request headers** ein, und klicken Sie dann auf **HTTP Get**.

**sampleheader** hat in der Antwort immer noch den Wert **value1**. Probieren Sie unterschiedliche Werte aus und beachten Sie, dass immer der zwischengespeicherte Wert aus dem ersten Aufruf zurückgegeben wird.

Geben Sie **25** in das Feld **param2** ein, und klicken Sie dann auf **HTTP Get**.

**sampleheader** hat in der Antwort nun den Wert **value2**. Die zuvor zwischengespeicherte Antwort wurde nicht zurückgegeben, da die Operationsergebnisse einen Schlüssel anhand der Abfragezeichenfolge erhalten.

## <a name="next-steps"> </a>Nächste Schritte
* Weitere Informationen zu Richtlinien für die Zwischenspeicherung finden Sie unter [Richtlinien für die Zwischenspeicherung][Richtlinien für die Zwischenspeicherung] in der [Richtlinienreferenz für API Management][Richtlinienreferenz für API Management].
* Informationen zum Zwischenspeichern von Elementen nach Schlüssel mithilfe von Richtlinienausdrücken finden Sie unter [Benutzerdefiniertes Zwischenspeichern in Azure API Management](api-management-sample-cache-by-key.md).

[api-management-management-console]: ./media/api-management-howto-cache/api-management-management-console.png
[api-management-echo-api]: ./media/api-management-howto-cache/api-management-echo-api.png
[api-management-echo-api-operations]: ./media/api-management-howto-cache/api-management-echo-api-operations.png
[api-management-caching-tab]: ./media/api-management-howto-cache/api-management-caching-tab.png
[api-management-operation-dropdown]: ./media/api-management-howto-cache/api-management-operation-dropdown.png
[api-management-policy-editor]: ./media/api-management-howto-cache/api-management-policy-editor.png
[api-management-developer-portal-menu]: ./media/api-management-howto-cache/api-management-developer-portal-menu.png
[api-management-apis-echo-api]: ./media/api-management-howto-cache/api-management-apis-echo-api.png
[api-management-open-console]: ./media/api-management-howto-cache/api-management-open-console.png
[api-management-console]: ./media/api-management-howto-cache/api-management-console.png


[Hinzufügen von Operationen zu einer API]: api-management-howto-add-operations.md
[Hinzufügen und Veröffentlichen eines Produkts]: api-management-howto-add-products.md
[Überwachung und Analyse]: api-management-monitoring.md
[Hinzufügen von APIs zu einem Produkt]: api-management-howto-add-products.md#add-apis
[Veröffentlichen eines Produkts]: api-management-howto-add-products.md#publish-product
[Erste Schritte mit Azure API Management]: api-management-get-started.md

[API Management-Richtlinienreferenz]: https://msdn.microsoft.com/library/azure/dn894081.aspx
[Cachingrichtlinien]: https://msdn.microsoft.com/library/azure/dn894086.aspx

[Erstellen einer API-Verwaltungsinstanz]: api-management-get-started.md#create-service-instance

[Konfigurieren einer Operation für die Zwischenspeicherung]: #configure-caching
[Prüfen der Richtlinien für die Zwischenspeicherung]: #caching-policies
[Aufrufen einer Operation und Testen der Zwischenspeicherung]: #test-operation
[Nächste Schritte]: #next-steps



<!--HONumber=Nov16_HO2-->



---
title: "Schützen Ihrer API mit Azure API Management | Microsoft Docs"
description: "Erfahren Sie, wie Sie Ihre API mithilfe von Richtlinien für Kontingente und Drosselung (Beschränken der Aufrufhäufigkeit) schützen."
services: api-management
documentationcenter: 
author: steved0x
manager: erikre
editor: 
ms.assetid: 450dc368-d005-401d-ae64-3e1a2229b12f
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: get-started-article
ms.date: 12/15/2016
ms.author: sdanie
translationtype: Human Translation
ms.sourcegitcommit: 30ec6f45da114b6c7bc081f8a2df46f037de61fd
ms.openlocfilehash: 73c9675490f95f68450716cd67e58df9c84daef8


---
# <a name="protect-your-api-with-rate-limits-using-azure-api-management"></a>Schützen Ihrer API mithilfe von Aufruflimits in Azure API Management
In diesem Leitfaden wird gezeigt, wie einfach Sie Ihre Back-End-API schützen können, indem Sie Richtlinien für Aufruflimits und Kontingente mithilfe von API Management konfigurieren.

In diesem Tutorial erstellen Sie ein kostenloses API-Testprodukt, das es Entwicklern auf Grundlage der Richtlinien [Aufrufrate nach Abonnement einschränken](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate) und [Nutzungskontingent nach Abonnement festlegen](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota) ermöglicht, bis zu zehn Aufrufe pro Minute und maximal 200 Aufrufe pro Woche an Ihre API zu richten. Anschließend veröffentlichen Sie die API und testen die Richtlinie für das Aufruflimit.

Informationen zu fortgeschritteneren Drosselungsszenarien mithilfe der Richtlinien [rate-limit-by-key](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRateByKey) und [quota-by-key](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuotaByKey) finden Sie unter [Erweiterte Anforderungsbegrenzung mit Azure API Management](api-management-sample-flexible-throttling.md).

## <a name="create-product"> </a>So erstellen Sie ein Produkt
In diesem Schritt erstellen Sie ein kostenloses Testprodukt, für das keine Abonnementgenehmigung erforderlich ist.

> [!NOTE]
> Wenn Sie bereits ein Produkt konfiguriert haben und dieses für das Tutorial verwenden möchten, können Sie direkt zum Abschnitt [Konfigurieren von Richtlinien für Aufruflimits und Kontingente][Configure call rate limit and quota policies] wechseln und die weiteren Schritte im Tutorial ausführen. Ersetzen Sie hierbei die kostenlose Testversion durch Ihr Produkt.
> 
> 

Klicken Sie zunächst im Azure-Portal für Ihren API Management-Dienst auf **Entwicklerportal**.

![Herausgeberportal][api-management-management-console]

> Falls Sie noch keine API Management-Dienstinstanz erstellt haben, finden Sie weitere Informationen im Abschnitt [Erstellen einer API Management-Dienstinstanz][Create an API Management service instance] des Tutorials [Verwalten Ihrer ersten API in Azure API Management][Manage your first API in Azure API Management].
> 
> 

Klicken Sie auf **Produkte** im Menü **API Management** auf der linken Seite, um die Seite **Produkte** anzuzeigen.

![Produkt hinzufügen][api-management-add-product]

Klicken Sie auf **Produkt hinzufügen**, um das Dialogfeld **Neues Produkt hinzufügen** anzuzeigen.

![Neues Produkt hinzufügen][api-management-new-product-window]

Geben Sie im Feld **Titel** den Text **Kostenlose Testversion** ein.

Geben Sie in das Textfeld **Beschreibung** Folgendes ein:  **Abonnenten können bis zu 10 Aufrufe pro Minute und bis zu 200 Aufrufe pro Woche ausführen, danach wird der Zugriff verweigert.**

Produkte in API Management können geschützt oder offen sein. Geschützte Produkte müssen abonniert werden, bevor sie verwendet werden können. Offene Produkte können ohne Abonnement genutzt werden. Vergewissern Sie sich, dass die Option **Abonnement erforderlich** ausgewählt ist, um ein geschütztes Produkt zu erstellen, das abonniert werden muss. Dies ist die Standardeinstellung.

Wenn Sie möchten, dass ein Administrator Abonnementanfragen für dieses Produkt prüfen und ablehnen oder akzeptieren muss, aktivieren Sie das Kontrollkästchen **Abonnementgenehmigung erforderlich**. Wenn das Kontrollkästchen nicht aktiviert ist, werden Abonnements automatisch genehmigt. In diesem Beispiel werden Abonnements automatisch genehmigt. Lassen Sie das Kontrollkästchen also deaktiviert.

Wenn Sie zulassen möchten, dass Entwicklerkonten das neue Produkt mehrfach abonnieren, aktivieren Sie das Kontrollkästchen **Mehrere gleichzeitige Abonnements zulassen**. In diesem Tutorial werden keine gleichzeitigen Abonnements verwendet. Lassen Sie das Kontrollkästchen daher deaktiviert.

Geben Sie alle Werte ein und klicken Sie auf **Speichern** , um das Produkt zu erstellen.

![Hinzugefügtes Produkt][api-management-product-added]

Standardmäßig sind neue Produkte für Benutzer in der Gruppe **Administratoren** sichtbar. Wir werden die Gruppe **Entwickler** hinzufügen. Klicken Sie auf **Kostenlose Testversion** und anschließend auf die Registerkarte **Sichtbarkeit**.

> In API Management werden Gruppen verwendet, um die Sichtbarkeit von Produkten für Entwickler zu verwalten. Produkte gewähren Sichtbarkeit für Gruppen, und Entwickler können alle Produkte anzeigen und abonnieren, die für die Gruppen sichtbar sind, in denen sie Mitglied sind. Weitere Informationen finden Sie unter [Erstellen und Verwenden von Gruppen in Azure API Management][How to create and use groups in Azure API Management].
> 
> 

![Hinzufügen der Gruppe "Entwickler"][api-management-add-developers-group]

Wählen Sie das Kontrollkästchen **Entwickler**, und klicken Sie dann auf **Speichern**.

## <a name="add-api"> </a>So fügen Sie eine API zum Produkt hinzu
In diesem Schritt des Lernprogramms werden Sie die Echo API zu dem neu erstellten Produkt "Kostenloser Test" hinzufügen.

> Jede API Management-Dienstinstanz enthält eine vorkonfigurierte Echo-API, die Sie für Tests und erste Schritte mit API Management nutzen können. Weitere Informationen finden Sie unter [Verwalten Ihrer ersten API in Azure API Management][Manage your first API in Azure API Management].
> 
> 

Klicken Sie im Menü **API Management** auf der linken Seite auf **Produkte**, und klicken Sie dann auf **Kostenlose Testversion**, um das Produkt zu konfigurieren.

![Produkt konfigurieren][api-management-configure-product]

Klicken Sie auf **API zu Produkt hinzufügen**.

![API zu Produkt hinzufügen][api-management-add-api]

Wählen Sie **Echo API** aus, und klicken Sie dann auf **Speichern**.

![Echo API hinzufügen][api-management-add-echo-api]

## <a name="policies"> </a>So konfigurieren Sie Richtlinien für Aufruflimits und Kontingente
Aufruflimits und Kontingente werden im Richtlinien-Editor konfiguriert. Klicken Sie im Menü **API Management** auf der linken Seite auf **Richtlinien**. Klicken Sie in der Liste **Produkt** auf **Kostenlose Testversion**.

![Produktrichtlinie][api-management-product-policy]

Klicken Sie auf **Richtlinie hinzufügen** , um die Richtlinienvorlage zu importieren und die Richtlinien für Aufruflimits und Kontingente zu erstellen.

![Richtlinie hinzufügen][api-management-add-policy]

Um Richtlinien hinzuzufügen, platzieren Sie den Cursor entweder im **Eingehend**- oder **Ausgehend**-Bereich der Richtlinienvorlage. Richtlinien für Aufruflimits und Kontingente sind eingehende Richtlinien. Platzieren Sie den Cursor also im Element für eingehende Richtlinien.

![Richtlinieneditor][api-management-policy-editor-inbound]

In diesem Tutorial erstellen wir die Richtlinien [Aufruflimit pro Abonnement](https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate) und [Nutzungskontingent pro Abonnement](https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota).

![Richtlinienanweisungen][api-management-limit-policies]

Nachdem Sie den Cursor im Richtlinienelement **Eingehend** platziert haben, klicken Sie auf den Pfeil neben **Aufruflimit pro Abonnement**, um die Richtlinienvorlage einzufügen.

```xml
<rate-limit calls="number" renewal-period="seconds">
<api name="name" calls="number">
<operation name="name" calls="number" />
</api>
</rate-limit>
```

**Aufruflimit pro Abonnement** kann auf der Produktebene sowie auf der API- und auf der Namensebene für einzelne Vorgänge verwendet werden. Dieses Lernprogramm verwendet Richtlinien auf Produktebene. Löschen Sie also die Elemente **api** und **operation** aus dem Element **rate-limit**, sodass nur das äußere **rate-limit**-Element verbleibt, wie im folgenden Beispiel gezeigt.

```xml
<rate-limit calls="number" renewal-period="seconds">
</rate-limit>
```

Für das Produkt „Kostenlose Testversion“ sind maximal 10 Aufrufe pro Minute erlaubt. Geben Sie also **10** als Wert für das **calls**-Attribut und **60** für das **renewal-period**-Attribut ein.

```xml
<rate-limit calls="10" renewal-period="60">
</rate-limit>
```

Um die Richtlinie **Nutzungskontingent pro Abonnement** zu konfigurieren, platzieren Sie den Cursor innerhalb des **inbound**-Elements direkt unterhalb des neu erstellten **rate-limit**-Elements und klicken dann auf den Pfeil links neben **Nutzungskontingent pro Abonnement einstellen**.

```xml
<quota calls="number" bandwidth="kilobytes" renewal-period="seconds">
<api name="name" calls="number" bandwidth="kilobytes">
<operation name="name" calls="number" bandwidth="kilobytes" />
</api>
</quota>
```

Da diese Richtlinie auf der Produktebene arbeiten soll, löschen Sie die Elemente mit den Namen **api** und **operation**, wie im folgenden Beispiel gezeigt.

```xml
<quota calls="number" bandwidth="kilobytes" renewal-period="seconds">
</quota>
```

Kontingente können basierend auf der Anzahl von Aufrufen pro Intervall, der Bandbreite oder beiden Werten konfiguriert werden. In diesem Lernprogramm drosseln wir nicht nach Bandbreite. Löschen Sie also das **bandwidth**-Attribut.

```xml
<quota calls="number" renewal-period="seconds">
</quota>
```

Für das Produkt Kostenloser Test gilt ein Kontingent von 200 Aufrufen pro Woche. Geben Sie **200** als Wert für das **calls**-Attribut und dann **604800** als Wert für das **renewal-period**-Attribut ein.

```xml
<quota calls="200" renewal-period="604800">
</quota>
```

> Die Richtlinienintervalle werden in Sekunden angegeben. Um die Intervalle für eine Woche zu berechnen, multiplizieren Sie die Anzahl der Tage (7) mit der Anzahl der Stunden pro Tag (24), der Anzahl der Minuten pro Stunde (60) und der Anzahl von Sekunden pro Minute (60): 7 * 24 * 60 x 60 = 604800.
> 
> 

Konfigurieren Sie die Richtlinie, sodass diese dem folgenden Beispiel entspricht.

```xml
<policies>
    <inbound>
        <rate-limit calls="10" renewal-period="60">
        </rate-limit>
        <quota calls="200" renewal-period="604800">
        </quota>
        <base />

</inbound>
<outbound>

    <base />

    </outbound>
</policies>
```

Nachdem Sie die gewünschten Richtlinien konfiguriert haben, klicken Sie auf **Speichern**.

![Richtlinie speichern][api-management-policy-save]

## <a name="publish-product"> </a> So veröffentlichen Sie das Produkt
Nachdem Sie die APIs hinzugefügt und die Richtlinien konfiguriert haben, muss das Produkt veröffentlicht werden, damit es von Entwicklern verwendet werden kann. Klicken Sie im Menü **API Management** auf der linken Seite auf **Produkte**, und klicken Sie dann auf **Kostenlose Testversion**, um das Produkt zu konfigurieren.

![Produkt konfigurieren][api-management-configure-product]

Klicken Sie auf **Veröffentlichen** und anschließend zur Bestätigung auf **Ja, veröffentlichen**.

![Veröffentlichen des Produkts][api-management-publish-product]

## <a name="subscribe-account"> </a>So abonnieren Sie das Produkt über ein Entwicklerkonto
Ihr Produkt ist nun veröffentlicht und kann von Entwicklern abonniert und verwendet werden.

> Administratoren einer API Management-Instanz werden automatisch für alle Produkte abonniert. In diesem Schritt des Lernprogramms werden wir das Produkt "Kostenloser Test" mit einem Entwicklerkonto abonnieren, bei dem es sich nicht um ein Administratorkonto handelt. Falls Ihr Entwicklerkonto Teil der Administratorrolle ist, können Sie diesen Schritt dennoch mitverfolgen, auch wenn Sie das Produkt bereits abonniert haben.
> 
> 

Klicken Sie im Menü **API Management** auf der linken Seite auf **Benutzer**, und klicken Sie dann auf den Namen Ihres Entwicklerkontos. In diesem Beispiel verwenden wir das Entwicklerkonto **Clayton Gragg** .

![Entwickler konfigurieren][api-management-configure-developer]

Klicken Sie auf **Abonnement hinzufügen**.

![Abonnement hinzufügen][api-management-add-subscription-menu]

Wählen Sie **Kostenlose Testversion**, und klicken Sie dann auf **Abonnieren**.

![Abonnement hinzufügen][api-management-add-subscription]

> [!NOTE]
> In diesem Tutorial sind für das Produkt „Kostenlose Testversion“ keine gleichzeitigen Abonnements aktiviert. Wenn Sie diese Option aktivieren, werden Sie zur Angabe des Abonnements aufgefordert, wie im folgenden Beispiel gezeigt.
> 
> 

![Abonnement hinzufügen][api-management-add-subscription-multiple]

Nach dem Klicken auf **Abonnieren** wird das Produkt in der Abonnementliste**** für den Benutzer angezeigt.

![Abonnement hinzugefügt][api-management-subscription-added]

## <a name="test-rate-limit"> </a>So rufen Sie eine Operation auf und testen das Aufruflimit
Sie haben das Produkt "Kostenloser Test" nun konfiguriert und veröffentlicht und können Operationen aufrufen, um die Richtlinie für das Aufruflimit zu testen.
Klicken Sie im Menü oben rechts auf **Entwicklerportal** , um zum Entwicklerportal zu gelangen.

![Entwicklerportal][api-management-developer-portal-menu]

Klicken Sie im Hauptmenü auf **APIs** und anschließend auf **Echo API**.

![Entwicklerportal][api-management-developer-portal-api-menu]

Klicken Sie auf **GET Resource** und anschließend auf **Ausprobieren**.

![Konsole öffnen][api-management-open-console]

Behalten Sie die standardmäßigen Parameterwerte bei, und wählen Sie Ihren Abonnementschlüssel für das Produkt „Kostenlose Testversion“ aus.

![Abonnementschlüssel][api-management-select-key]

> [!NOTE]
> Falls Sie mehrere Abonnements haben, achten Sie darauf, den Schlüssel für **Kostenlose Testversion** auszuwählen. Andernfalls treten die in den vorherigen Schritten konfigurierten Richtlinien nicht in Kraft.
> 
> 

Klicken Sie auf **Senden**, und sehen Sie sich dann die Antwort an. Beachten Sie den **Antwortstatus** von **200 OK**.

![Operationsergebnis][api-management-http-get-results]

Klicken Sie häufiger als das konfigurierte Limit von zehnmal pro Minute auf **Senden** . Nachdem das Aufruflimit überschritten wurde, wird der Antwortstatus **429 Too Many Requests** zurückgegeben.

![Operationsergebnis][api-management-http-get-429]

Der **Antwortinhalt** gibt das verbleibende Intervall an, bis neue Aufrufe möglich sind.

Wenn das Aufruflimit von 10 Aufrufen pro Minute aktiv ist, werden nachfolgende Aufrufe fehlschlagen, bis 60 Sekunden nach dem ersten der 10 erfolgreichen Aufrufe an das Produkt vergangen sind, bevor das Aufruflimit überschritten wurde. In diesem Beispiel beträgt das verbleibende Intervall 54 Sekunden.

## <a name="next-steps"> </a>Nächste Schritte
* Das folgende Video zeige eine Demo zum Festlegen von Aufruflimits und Kontingenten.

> [!VIDEO https://channel9.msdn.com/Blogs/AzureApiMgmt/Rate-Limits-and-Quotas/player]
> 
> 

[api-management-management-console]: ./media/api-management-howto-product-with-rules/api-management-management-console.png
[api-management-add-product]: ./media/api-management-howto-product-with-rules/api-management-add-product.png
[api-management-new-product-window]: ./media/api-management-howto-product-with-rules/api-management-new-product-window.png
[api-management-product-added]: ./media/api-management-howto-product-with-rules/api-management-product-added.png
[api-management-add-policy]: ./media/api-management-howto-product-with-rules/api-management-add-policy.png
[api-management-policy-editor-inbound]: ./media/api-management-howto-product-with-rules/api-management-policy-editor-inbound.png
[api-management-limit-policies]: ./media/api-management-howto-product-with-rules/api-management-limit-policies.png
[api-management-policy-save]: ./media/api-management-howto-product-with-rules/api-management-policy-save.png
[api-management-configure-product]: ./media/api-management-howto-product-with-rules/api-management-configure-product.png
[api-management-add-api]: ./media/api-management-howto-product-with-rules/api-management-add-api.png
[api-management-add-echo-api]: ./media/api-management-howto-product-with-rules/api-management-add-echo-api.png
[api-management-developer-portal-menu]: ./media/api-management-howto-product-with-rules/api-management-developer-portal-menu.png
[api-management-publish-product]: ./media/api-management-howto-product-with-rules/api-management-publish-product.png
[api-management-configure-developer]: ./media/api-management-howto-product-with-rules/api-management-configure-developer.png
[api-management-add-subscription-menu]: ./media/api-management-howto-product-with-rules/api-management-add-subscription-menu.png
[api-management-add-subscription]: ./media/api-management-howto-product-with-rules/api-management-add-subscription.png
[api-management-developer-portal-api-menu]: ./media/api-management-howto-product-with-rules/api-management-developer-portal-api-menu.png
[api-management-open-console]: ./media/api-management-howto-product-with-rules/api-management-open-console.png
[api-management-http-get]: ./media/api-management-howto-product-with-rules/api-management-http-get.png
[api-management-http-get-results]: ./media/api-management-howto-product-with-rules/api-management-http-get-results.png
[api-management-http-get-429]: ./media/api-management-howto-product-with-rules/api-management-http-get-429.png
[api-management-product-policy]: ./media/api-management-howto-product-with-rules/api-management-product-policy.png
[api-management-add-developers-group]: ./media/api-management-howto-product-with-rules/api-management-add-developers-group.png
[api-management-select-key]: ./media/api-management-howto-product-with-rules/api-management-select-key.png
[api-management-subscription-added]: ./media/api-management-howto-product-with-rules/api-management-subscription-added.png
[api-management-add-subscription-multiple]: ./media/api-management-howto-product-with-rules/api-management-add-subscription-multiple.png

[How to add operations to an API]: api-management-howto-add-operations.md
[How to add and publish a product]: api-management-howto-add-products.md
[Monitoring and analytics]: ../api-management-monitoring.md
[Add APIs to a product]: api-management-howto-add-products.md#add-apis
[Publish a product]: api-management-howto-add-products.md#publish-product
[Manage your first API in Azure API Management]: api-management-get-started.md
[How to create and use groups in Azure API Management]: api-management-howto-create-groups.md
[View subscribers to a product]: api-management-howto-add-products.md#view-subscribers
[Get started with Azure API Management]: api-management-get-started.md
[Create an API Management service instance]: api-management-get-started.md#create-service-instance
[Next steps]: #next-steps

[Create a product]: #create-product
[Configure call rate limit and quota policies]: #policies
[Add an API to the product]: #add-api
[Publish the product]: #publish-product
[Subscribe a developer account to the product]: #subscribe-account
[Call an operation and test the rate limit]: #test-rate-limit

[Limit call rate]: https://msdn.microsoft.com/library/azure/dn894078.aspx#LimitCallRate
[Set usage quota]: https://msdn.microsoft.com/library/azure/dn894078.aspx#SetUsageQuota



<!--HONumber=Jan17_HO1-->



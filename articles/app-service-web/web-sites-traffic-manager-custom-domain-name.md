---
title: "Konfigurieren Sie einen benutzerdefinierten Domänennamen für eine Web-App in Azure App Services mit Traffic Manager für den Lastenausgleich."
description: "Verwenden Sie einen benutzerdefinierten Domänennamen für eine Web-App in Azure App Services mit Traffic Manager für den Lastenausgleich."
services: app-service\web
documentationcenter: 
author: rmcmurray
manager: wpickett
editor: 
ms.assetid: 0f96c0e7-0901-489b-a95a-e3b66ca0a1c2
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/20/2016
ms.author: robmcm
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 0f8d2941b30079463954b4f9b8b9dfb10cfa3ea1


---
# <a name="configuring-a-custom-domain-name-for-a-web-app-in-azure-app-service-using-traffic-manager"></a>Konfigurieren eines benutzerdefinierten Domänennamens für eine Web-App in Azure App Services, der Traffic Manager verwendet.
[!INCLUDE [web-selector](../../includes/websites-custom-domain-selector.md)]

[!INCLUDE [intro](../../includes/custom-dns-web-site-intro-traffic-manager.md)]

Dieser Artikel enthält allgemeine Anweisungen zur Verwendung eines benutzerdefinierten Domänennamens mit Azure App Services unter Verwendung von Traffic Manager für den Lastenausgleich.

[!INCLUDE [tmwebsitefooter](../../includes/custom-dns-web-site-traffic-manager-notes.md)]

[!INCLUDE [introfooter](../../includes/custom-dns-web-site-intro-notes.md)]

<a name="understanding-records"></a>

## <a name="understanding-dns-records"></a>Interpretation von DNS-Datensätzen
[!INCLUDE [understandingdns](../../includes/custom-dns-web-site-understanding-dns-traffic-manager.md)]

<a name="bkmk_configsharedmode"></a>

## <a name="configure-your-web-apps-for-standard-mode"></a>Konfigurieren Ihrer Web-Apps für den Modus "Standard"
[!INCLUDE [modes](../../includes/custom-dns-web-site-modes-traffic-manager.md)]

<a name="bkmk_configurecname"></a>

## <a name="add-a-dns-record-for-your-custom-domain"></a>Hinzufügen eines DNS-Datensatzes zu Ihrer benutzerdefinierten Domäne
> [!NOTE]
> Wenn Sie eine Domäne über Azure App Service-Web-Apps erworben haben, überspringen Sie die folgenden Schritte, und lesen Sie den letzten Schritt des Artikels zum [Erwerb einer Domäne für Web-Apps](custom-dns-web-site-buydomains-web-app.md) .
> 
> 

Um Ihrer benutzerdefinierten Domäne eine Web-App in Azure App Services zuzuweisen, müssen Sie in der DNS-Tabelle für Ihre benutzerdefinierte Domäne mithilfe der von der Domänenregistrierungsstelle bereitgestellten Tools einen neuen Eintrag hinzufügen. Gehen Sie wie folgt vor, um die DNS-Tools zu suchen und zu verwenden:

1. Melden Sie sich bei Ihrem Konto mit der Domänenregistrierungsstelle an, und suchen Sie nach der Seite zum Verwalten von DNS-Datensätzen. Suchen Sie nach Links oder Bereichen der Website, die als **Domänenname**, **DNS** oder **Namenserververwaltung** bezeichnet werden. Häufig finden Sie in Ihren Kontoinformationen einen Link auf diese Seite. Suchen Sie anschließend nach einem Link, der Sie z.B. zu **Meine Domänen** führt.
2. Sobald Sie die Verwaltungsseite für Ihren Domänennamen gefunden haben, suchen Sie nach einem Link, über den Sie die DNS-Datensätze bearbeiten können. Dieser Konfigurationslink kann als **Zonendatei**, **DNS-Eintrag** oder als **Erweitert** bezeichnet werden.
   
   * Auf der Seite wurden wahrscheinlich bereits Datensätze erstellt, wie beispielsweise ein Eintrag, der „**@**“ oder „\*“ mit einer Domain-Parking-Seite verknüpft. Möglicherweise enthält sie auch bereits Datensätze für allgemeine Unterdomänen wie **www**.
   * Auf der Seite werden **CNAME-Einträge** angegeben, oder Sie finden ein Dropdownmenü, in dem Sie den Datensatztyp auswählen können. Möglicherweise werden auch andere Datensätze wie **A-Datensätze** und **MX-Einträge** aufgeführt. In manchen Fällen werden CNAME-Einträge auch z.B. als **Aliaseintrag** bezeichnet.
   * Auf der Seite werden außerdem Felder angezeigt, über die Sie über einen **Hostnamen** oder **Domänennamen** eine **Zuweisung** zu einem anderen Domänennamen vornehmen können.
3. Jede Registrierungsstelle ist spezifisch, generell weisen Sie jedoch *von* Ihrem benutzerdefinierten Domänennamen (z.B. **contoso.com**) *zum* Traffic Manager-Domänennamen (**contoso.trafficmanager.net**) zu, der für Ihre Web-App verwendet wird.
   
   > [!NOTE]
   > Wenn ein Eintrag bereits verwendet wird und Sie Ihre Apps präemptiv an ihn binden müssen, können Sie einen zusätzlichen CNAME-Eintrag erstellen. Um beispielsweise **www.contoso.com** präemptiv an Ihre Web-App zu binden, erstellen Sie einen CNAME-Eintrag zur Verknüpfung von **awverify.www** mit **contoso.trafficmanager.net**. Anschließend können Sie „www.contoso.com“ zu Ihrer Web-App hinzufügen, ohne den CNAME-Eintrag „www“ zu ändern. Weitere Informationen finden Sie unter [Erstellen von DNS-Einträgen für eine Web-App in einer benutzerdefinierten Domäne][CREATEDNS].
   > 
   > 
4. Speichern Sie die Änderungen, sobald Sie die DNS-Datensätze in Ihrer Registrierung hinzugefügt oder geändert haben.

<a name="enabledomain"></a>

## <a name="enable-traffic-manager"></a>Aktivieren des Traffic Manager
[!INCLUDE [modes](../../includes/custom-dns-web-site-enable-on-traffic-manager.md)]

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie im [Node.js Developer Center](/develop/nodejs/).

[!INCLUDE [app-service-web-whats-changed](../../includes/app-service-web-whats-changed.md)]

[!INCLUDE [app-service-web-try-app-service](../../includes/app-service-web-try-app-service.md)]

<!-- URL List -->

[CREATEDNS]: ../dns/dns-web-sites-custom-domain.md



<!--HONumber=Nov16_HO3-->



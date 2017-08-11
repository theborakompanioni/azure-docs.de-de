---
title: "Kaufen eines benutzerdefinierten Domänennamens für Azure-Web-Apps"
description: "Erfahren Sie, wie Sie einen benutzerdefinierten Domänennamen für eine Web-App in Azure App Service kaufen."
services: app-service\web
documentationcenter: 
author: rmcmurray
manager: erikre
editor: 
ms.assetid: 70fb0e6e-8727-4cca-ba82-98a4d21586ff
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/19/2017
ms.author: robmcm
ms.translationtype: HT
ms.sourcegitcommit: 79bebd10784ec74b4800e19576cbec253acf1be7
ms.openlocfilehash: c42e419984ee4b7f30c8423b9a0c420e2c981949
ms.contentlocale: de-de
ms.lasthandoff: 08/03/2017

---
# <a name="buy-a-custom-domain-name-for-azure-web-apps"></a>Kaufen eines benutzerdefinierten Domänennamens für Azure-Web-Apps

App Service-Domänen (Vorschau) sind Domänen der obersten Ebene, die direkt in Azure verwaltet werden. Sie erleichtern die Verwaltung von benutzerdefinierten Domänen für [Azure Web Apps (Azure-Web-Apps)](app-service-web-overview.md). In diesem Tutorial wird gezeigt, wie Sie eine App Service-Domäne kaufen und Azure-Web-Apps einen DNS-Namen zuweisen.

Dieser Artikel bezieht sich auf Azure App Service (Web-Apps, API-Apps, mobile Apps, Logic Apps). Informationen zu Cloud Services finden Sie unter [Konfigurieren eines benutzerdefinierten Domänennamens für einen Azure-Clouddienst](../cloud-services/cloud-services-custom-domain-name-portal.md).

## <a name="prerequisites"></a>Voraussetzungen

Für dieses Tutorial benötigen Sie Folgendes:

* [Erstellen Sie eine App Service-App](/azure/app-service/), oder verwenden Sie eine App, die Sie für ein anderes Tutorial erstellt haben.

## <a name="prepare-the-app"></a>Vorbereiten der App

Um benutzerdefinierte Domänen in Azure-Web-Apps zu verwenden, muss der [App Service-Plan](https://azure.microsoft.com/pricing/details/app-service/) Ihrer Web-App einen kostenpflichtigen Tarif (**Shared**, **Basic**, **Standard** oder **Premium**) aufweisen. Stellen Sie in diesem Schritt sicher, dass sich die Web-App im richtigen Tarif befindet.

### <a name="sign-in-to-azure"></a>Anmelden bei Azure

Öffnen Sie das [Azure-Portal](https://portal.azure.com), und melden Sie sich mit Ihrem Azure-Konto an.

### <a name="navigate-to-the-app-in-the-azure-portal"></a>Navigieren zur App im Azure-Portal

Wählen Sie im linken Menü **App Services** und anschließend den Namen der App aus.

![Portalnavigation zur Azure-App](./media/app-service-web-tutorial-custom-domain/select-app.png)

Die Verwaltungsseite der App Service-App wird angezeigt.  

### <a name="check-the-pricing-tier"></a>Überprüfen des Tarifs

Scrollen Sie im linken Navigationsbereich der App-Seite zum Abschnitt **Einstellungen**, und wählen Sie **Zentral hochskalieren (App Service-Plan)**.

![Menü „Zentral hochskalieren“](./media/app-service-web-tutorial-custom-domain/scale-up-menu.png)

Der aktuelle Tarif der App wird durch einen blauen Rahmen hervorgehoben. Stellen Sie sicher, dass sich die App nicht im Tarif **Free** befindet. Benutzerdefinierte DNS-Einträge werden im Tarif **Free** nicht unterstützt. 

![Überprüfen des Tarifs](./media/app-service-web-tutorial-custom-domain/check-pricing-tier.png)

Wenn für den App Service-Plan nicht der Tarif **Free** gilt, können Sie die Seite **Choose your pricing tier** (Preisstufe auswählen) schließen und zu [Buy the domain](#buy-the-domain) (Domäne kaufen) springen.

### <a name="scale-up-the-app-service-plan"></a>Zentrales Hochskalieren des App Service-Plans

Wählen Sie einen der kostenpflichtigen Tarife aus (**Shared**, **Basic**, **Standard** oder **Premium**). 

Klicken Sie auf **Auswählen**.

![Überprüfen des Tarifs](./media/app-service-web-tutorial-custom-domain/choose-pricing-tier.png)

Wenn die unten angegebene Benachrichtigung angezeigt wird, ist der Skalierungsvorgang abgeschlossen.

![Bestätigung des Skalierungsvorgangs](./media/app-service-web-tutorial-custom-domain/scale-notification.png)

## <a name="buy-the-domain"></a>Kaufen der Domäne

### <a name="sign-in-to-azure"></a>Anmelden bei Azure
Öffnen Sie das [Azure-Portal](https://portal.azure.com/), und melden Sie sich mit Ihrem Azure-Konto an.

### <a name="launch-buy-domains"></a>Starten Sie „Domänen kaufen“
Klicken Sie auf der Registerkarte **Web-Apps** auf den Namen der Web-App, wählen Sie **Einstellungen** aus, und wählen Sie anschließend **Benutzerdefinierte Domänen** aus.
   
![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-6.png)

Klicken Sie auf dem Blatt **Custom domains** (Benutzerdefinierte Domänen) auf **Buy domains** (Domänen kaufen).

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-1.png)

### <a name="configure-the-domain-purchase"></a>Konfigurieren des Domänenkaufs

Tippen Sie den Domänennamen auf der Seite **App Service Domain** (App Service-Domäne) in das Suchfeld **Search for domain** (Domäne suchen) ein und drücken Sie `Enter`. Die vorgeschlagenen verfügbaren Domänen werden direkt unter dem Textfeld angezeigt. Klicken Sie mindestens eine Domäne an, die Sie kaufen möchten. 
   
![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-2.png)

Klicken Sie auf **Kontaktinformationen**, und füllen Sie das Formular mit den Kontaktinformationen für die Domäne aus. Wenn Sie fertig sind, klicken Sie auf **OK**, um zur Seite „App Service-Domäne“ zurückzukehren.
   
Es ist wichtig, dass Sie alle erforderlichen Felder so genau wie möglich ausfüllen. Fehlerhafte Daten in den Kontaktinformationen können dazu führen, dass der Domänenkauf fehlschlägt. 

Wählen Sie als nächstes die gewünschten Optionen für Ihre Domäne aus. Die folgende Tabelle enthält einige Erläuterungen:

| Einstellung | Empfohlener Wert | Beschreibung |
|-|-|-|
|Automatisches Erneuern | **Aktivieren** | Erneuert Ihre App Service-Domäne jedes Jahr automatisch. Zum Zeitpunkt der Erneuerung wird Ihre Kreditkarte mit dem Kaufbetrag belastet. |
|Datenschutz | Aktivieren | Wählen Sie „Datenschutz“. Dieser ist im Kaufpreis _for free_ (gratis) enthalten (außer für Domänen der obersten Ebenen, deren Registrierungen keinen Datenschutz unterstützen, zum Beispiel _.co.in_, _.co.uk_ usw.). |
| Zuweisen von Standardhostnamen | **www** und **@** | Wählen Sie bei Bedarf die gewünschten Hostnamenbindungen. Wenn der Vorgang des Domänenkaufs abgeschlossen ist, kann auf Ihre Web-App über die ausgewählten Hostnamen zugegriffen werden. Wenn Ihre Web-App sich hinter dem [Azure Traffic Manager](https://azure.microsoft.com/services/traffic-manager/) befindet, wird Ihnen die Option nicht angezeigt, durch die die Stammdomäne (@) zugewiesen werden kann, da der Traffic Manager keine A-Datensätze unterstützt. Sie können Änderungen an den Zuweisungen der Hostnamen vornehmen, nachdem der Domänenkauf abgeschlossen ist. |

### <a name="accept-terms-and-purchase"></a>Akzeptieren der Bedingungen und Erwerben

Klicken Sie auf **Legal Terms** (Rechtliche Bedingungen), um die Bedingungen und die Gebühren anzuzeigen, und klicken Sie dann auf **Buy** (Kaufen).

> [!NOTE]
> App Service-Domänen verwenden Azure DNS, um Domänen zu hosten. Zusätzlich zu den Gebühren für die Domänenregistrierung fallen Nutzungsgebühren für Azure DNS an. Informationen finden Sie unter [Azure DNS Pricing (Azure DNS-Preisübersicht)](https://azure.microsoft.com/pricing/details/dns/).
>
>

Klicken Sie auf der Seite **App Service Domain** (App Service-Domäne) auf **OK**. Während der Vorgang ausgeführt wird, sehen Sie folgende Benachrichtigungen:

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-validate.png)

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-purchase-success.png)

### <a name="test-the-hostnames"></a>Testen des Hostnamens

Wenn Sie Ihrer Web-App Standardhostnamen zugewiesen haben, wird Ihnen für jeden ausgewählten Hostnamen eine Erfolgsbenachrichtigung angezeigt. 

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-bind-success.png)

Auf der Seite **Custom domains** (Benutzerdefinierte Domänen) werden Ihnen die ausgewählten Hostnamen im Abschnitt **Hostnames** (Hostnamen) angezeigt. 

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-hostnames-added.png)

Um den Hostnamen zu testen, navigieren Sie zu den aufgelisteten Hostnamen im Browser. Versuchen Sie wie im Beispiel im obigen Screenshot, zu den Seiten _kontoso.net_ und _www.kontoso.net_ zu navigieren.

## <a name="assign-hostnames-to-web-app"></a>Zuweisen von Hostnamen zu Web-App

Wenn Sie Ihrer Web-App während des Kaufvorgangs nicht einen oder mehrere Standardhostnamen zuweisen möchten, oder wenn Sie einen Hostnamen zuweisen müssen, der nicht aufgeführt ist, können Sie die Zuweisung von Hostnamen zu jedem anderen Zeitpunkt vornehmen.

In der App Service-Domäne können Sie auch jeder anderen Web-App einen Hostnamen zuweisen. Die Schritte hängen davon ab, ob die App Service-Domäne und die Web-App zum selben Abonnement gehören.

- Anderes Abonnement: Zuordnen benutzerdefinierter DNS-Datensätze (z.B. eine extern erworbene Domäne) von der App Service-Domäne zur Web-App. Weitere Informationen zum Hinzufügen von benutzerdefinierten DNS-Namen zu einer App Service-Domäne finden Sie unter [Manage custom DNS records (Verwalten von benutzerdefinierten DNS-Datensätzen)](#custom). Weitere Informationen zum Zuordnen einer extern erworbenen Domäne zu einer Web-App finden Sie unter [Map an existing custom DNS name to Azure Web Apps (Zuweisen eines bestehenden benutzerdefinierten DNS-Namens zu Azure-Web-Apps)](app-service-web-tutorial-custom-domain.md). 
- Gleiches Abonnement: Führen Sie folgende Schritte aus.

### <a name="launch-add-hostname"></a>Starten Sie „Hostname hinzufügen“
Wählen Sie auf der Seite **App Services** (App-Dienste) den Namen Ihrer Web-App aus, der Sie Hostnamen zuweisen möchten, klicken Sie auf **Settings** (Einstellungen), und dann auf **Custom domains** (Benutzerdefinierte Domänen).

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-6.png)

Stellen Sie sicher, dass Ihre erworbene Domäne im Abschnitt**App Service Domains** (App Service-Domänen) aufgelistet ist, aber klicken Sie sie nicht an. 

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-select-domain.png)

> [!NOTE]
> Alle App Service-Domänen, die zum selben Abonnement gehören, werden auf der Seite **Custom domains** (Benutzerdefinierte Domänen) in der Web-App angezeigt. Wenn Ihre Domäne sich im Abonnement der Web-App befindet, jedoch nicht auf der Seite **Custom domains** (Benutzerdefinierte Domänen) der Web-App angezeigt wird, öffnen Sie die Seite **Custom domains** (Benutzerdefinierte Domänen) erneut oder aktualisieren Sie die Webseite. Überprüfen Sie auch die Anzeige der Benachrichtigungsglocke am oberen Rand des Azure-Portals auf Fortschritts- oder Erstellungsfehler.
>
>

Wählen Sie **Hostnamen hinzufügen**.

### <a name="configure-hostname"></a>Konfigurieren des Hostnamens
Tippen Sie im Dialogfeld**Add hostname** (Hostname hinzufügen) den vollqualifizierten Domänennamen Ihrer App Service-Domäne oder einer Unterdomäne ein. Beispiel:

- kontoso.net
- www.kontoso.net
- abc.kontoso.net

Klicken Sie anschließend auf **Validate** (Überprüfen). Der Datensatztyp für Ihren Hostnamen wird automatisch für Sie ausgewählt.

Wählen Sie **Hostnamen hinzufügen**.

Wenn der Vorgang abgeschlossen ist, sehen Sie eine Erfolgsmeldung für den zugeordneten Hostnamen.  

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-bind-success.png)

### <a name="close-add-hostname"></a>Schließen Sie „Hostname hinzufügen“
Auf der Seite **Add hostname** (Hostname hinzufügen) können Sie Ihrer Web-App bei Bedarf einen beliebigen anderen Hostnamen zuweisen. Schließen Sie die Seite **Add hostname** (Hostname hinzufügen), wenn Sie fertig sind.

Der oder die neu zugewiesenen Hostname(n) sollten nun auf der Seite **Custom Domains** (Benutzerdefinierte Domänen) in Ihrer App angezeigt werden.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-hostnames-added2.png)

### <a name="test-the-hostnames"></a>Testen des Hostnamens

Navigieren Sie zu den aufgelisteten Hostnamen im Browser. Versuchen Sie wie im Beispiel im obigen Screenshot, zu der Seite _abc.kontoso.net_ zu navigieren.

<a name="custom" />

## <a name="manage-custom-dns-records"></a>Verwalten von benutzerdefinierten DNS-Datensätzen

In Azure werden DNS-Datensätze für eine App Service-Domäne mithilfe von [Azure DNS](https://azure.microsoft.com/services/dns/) verwaltet. Sie können DNS-Datensätze hinzufügen, entfernen und aktualisieren, auch für eine extern erworbene Domäne.

### <a name="open-app-service-domain"></a>Open-App Service-Domäne

Klicken Sie im linken Menü des Azure-Portals auf**More Services** (Weitere Dienste)  > **App Service Domains** (App Service-Domänen).

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-access.png)

Wählen Sie die Domäne, die verwaltet werden soll. 

### <a name="access-dns-zone"></a>Zugreifen auf die DNS-Zone

Klicken Sie im linken Menü der Domäne auf **DNS zone** (DNS-Zone).

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-dns-zone.png)

Diese Aktion öffnet die Seite [DNS zone](../dns/dns-zones-records.md) (DNS-Zone) Ihrer App Service-Domäne in Azure DNS. Informationen zum Bearbeiten von DNS-Datensätzen finden Sie unter [How to manage DNS Zones in the Azure portal (Vorgehensweise: Verwalten von DNS-Zonen im Azure-Portal)](../dns/dns-operations-dnszones-portal.md).

## <a name="cancel-purchase-delete-domain"></a>Abbrechen des Kaufs (Domäne löschen)

Nach dem Kauf der App Service-Domäne haben Sie fünf Tage Zeit, um Ihren Kauf zu stornieren und eine vollständige Rückerstattung zu erhalten. Nach fünf Tagen können Sie die App Service-Domäne zwar löschen, erhalten jedoch keine Rückerstattung.

### <a name="open-app-service-domain"></a>Open-App Service-Domäne

Klicken Sie im linken Menü des Azure-Portals auf**More Services** (Weitere Dienste)  > **App Service Domains** (App Service-Domänen).

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-access.png)

Wählen Sie die Domäne aus, die Sie stornieren oder löschen möchten. 

### <a name="delete-hostname-bindings"></a>Löschen von Hostnamenbindungen

Klicken Sie im linken Menü der Domäne auf **Hostname bindings** (Hostnamenbindungen). Die Hostnamenbindungen aus allen Azure-Diensten werden hier aufgelistet.

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-hostname-bindings.png)

Die App Service-Domäne kann nicht gelöscht werden, bis alle Hostnamenbindungen gelöscht wurden.

Löschen Sie jede Hostnamenbindung, indem Sie auf **...** > **Delete** (Löschen) klicken. Nachdem alle Bindungen gelöscht worden sind, klicken Sie auf **Save** (Speichern).

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-delete-hostname-bindings.png)

### <a name="cancel-or-delete"></a>Stornieren oder Löschen

Klicken Sie im linken Menü der Domäne auf **Overview** (Übersicht). 

Wenn die Kündigungsfrist für die erworbene Domäne nicht abgelaufen ist, klicken Sie auf **Cancel purchase** (Kauf stornieren). Andernfalls sehen Sie stattdessen die Schaltfläche **Delete** (Löschen). Klicken Sie zum Löschen der Domäne ohne Rückerstattung auf **Delete** (Löschen).

![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-cancel.png)

Klicken Sie auf **OK**, um den Vorgang zu bestätigen. Wenn Sie nicht fortfahren möchten, klicken Sie auf eine beliebige Stelle außerhalb des Bestätigungsdialogfelds.

Nachdem der Vorgang abgeschlossen ist, ist die Domäne aus Ihrem Abonnement freigegeben und steht wieder für jeden zum Kauf zur Verfügung. 


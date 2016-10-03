<properties
	pageTitle="Kaufen eines benutzerdefinierten Domänennamens für Azure App Service-Web-Apps"
	description="Erfahren Sie, wie Sie einen benutzerdefinierten Domänennamen für eine Web-App in Azure App Service kaufen."
	services="app-service\web"
	documentationCenter=""
	authors="rmcmurray"
	manager="wpickett"
	editor=""/>

<tags
	ms.service="app-service-web"
	ms.workload="web"
	ms.tgt_pltfrm="na"
	ms.devlang="na"
	ms.topic="article"
	ms.date="08/11/2016"
	ms.author="robmcm"/>

# Kaufen und Konfigurieren eines benutzerdefinierten Domänennamens in Azure App Service

[AZURE.INCLUDE [web-selector](../../includes/websites-custom-domain-selector.md)]

Wenn Sie eine Web-App erstellen, weist Azure dieser eine Unterdomäne von "azurewebsites.NET" zu. Wenn Ihre Web-App beispielsweise **contoso** heißt, lautet die URL **contoso.azurewebsites.net**. Darüber hinaus weist Azure eine virtuelle IP-Adresse zu.

Bei einer Produktions-Web-App sollten Sie Benutzern einen benutzerdefinierten Domänennamen anzeigen. In diesem Artikel wird erläutert, wie Sie eine benutzerdefinierte Domäne für [App Service-Web-Apps](http://go.microsoft.com/fwlink/?LinkId=529714) kaufen und konfigurieren.

[AZURE.INCLUDE [introfooter](../../includes/custom-dns-web-site-intro-notes.md)]


## Übersicht

Wenn Sie keinen Domänennamen für Ihre Web-App haben, können Sie diesen im [Azure-Portal](https://portal.azure.com/) auf einfache Weise erwerben. Während des Einkaufsprozesses können Sie festlegen, dass die DNS-Einträge für WWW und die Stammdomäne automatisch Ihrer Web-App zugeordnet werden. Sie können Ihre Domäne auch direkt im Azure-Portal verwalten.


Gehen Sie folgendermaßen vor, um Domänennamen zu erwerben und Ihrer Web-App zuzuweisen.

1. Öffnen Sie das [Azure-Portal](https://portal.azure.com/) in Ihrem Browser.

2. Klicken Sie auf der Registerkarte **Web-Apps** auf den Namen der Web-App, wählen Sie **Einstellungen** aus, und wählen Sie anschließend **Benutzerdefinierte Domänen** aus.

	![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-6.png)

3. Klicken Sie auf dem Blatt **Benutzerdefinierte Domänen** auf **Domänen erwerben**.

	![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-1.png)

4. Geben Sie auf dem Blatt **Buy Domains** im Textfeld den Domänennamen ein, den Sie kaufen möchten, und drücken Sie die EINGABETASTE. Die vorgeschlagenen verfügbaren Domänen werden direkt unter dem Textfeld angezeigt. Wählen Sie die Domäne aus, die Sie kaufen möchten. Sie können auch mehrere Domänen auf einmal kaufen.

  ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-2.png)

5. Klicken Sie auf **Contact Information**, und füllen Sie das Formular mit den Kontaktinformationen für die Domäne aus.

  ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-3.png)

    > [AZURE.NOTE] It is very important that you fill out all required fields with as much accuracy as possible, especially the email address. In case of purchasing the domain without "Privacy protection", you might be asked to verify your email before the domain becomes active. In some cases, incorrect data for contact information will result in failure to purchase domains. 

6. Nun stehen folgende Optionen zur Auswahl:

	a) "Auto renew", um die Domäne jedes Jahr automatisch zu verlängern
	
	b) „Datenschutz“, um den KOSTENLOS im Kaufpreis enthaltenen Datenschutz zu aktivieren (mit Ausnahme von TLDs, deren Registrierung den Datenschutz nicht unterstützt. Beispiel: .co.in, .co.uk usw.)
	
	c) "Assign default hostnames", um der aktuellen Web-App Standardhostnamen für WWW und Stammdomäne zuzuweisen

  ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-2.5.png)
  
    > [AZURE.NOTE] Option C configures DNS bindings and Hostname bindings automatically for you.  This way, your Web App can be accessed using custom domain as soon as the purchase is complete (baring DNS propagation delays in few cases). In case, your Web App is behind Azure Traffic Manager, you will not see an option to assign root domain, as A-Records do not work with the Traffic Manager. 
    You can always assign the domains/sub-domains purchased through one Web App to another Web App and vice-versa. See step 8 for more details. 
	
7. Klicken Sie auf dem Blatt **Buy Domains** auf **Auswählen**. Daraufhin wird das Blatt **Purchase confirmation** mit den Informationen zum Kauf angezeigt. Wenn Sie den rechtlichen Bedingungen zustimmen und auf **Buy** klicken, wird Ihre Bestellung gesendet und Sie können den Einkaufsprozess unter **Notification** überwachen. Der Domänenkauf kann einige Minuten dauern.

  ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-4.png)

  ![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-5.png)

8. Wenn Sie erfolgreich eine Domäne bestellt haben, können Sie die Domäne verwalten und Ihrer Web-App zuweisen. Klicken Sie rechts neben Ihrer Domäne auf **...**. Anschließend können Sie entweder **Cancel purchase** oder **Domäne verwalten** auswählen. Klicken Sie auf **Domäne verwalten**, damit auf dem Blatt **Domäne verwalten** die **Unterdomäne** an die Web-App gebunden werden kann. Wenn Sie eine **Unterdomäne** an eine andere Web-App binden möchten, führen Sie diesen Schritt aus dem Kontext der entsprechenden Web-App heraus durch. Hier können Sie die Domäne einem Traffic Manager-Endpunkt zuweisen (sofern die Web-App über Traffic Manager verwaltet wird), indem Sie einfach im Dropdownmenü den Traffic Manager-Namen auswählen. Auf diese Weise wird die Domäne/Unterdomäne automatisch allen Web-Apps hinter diesem Traffic Manager-Endpunkt zugewiesen.

	![](./media/custom-dns-web-site-buydomains-web-app/dncmntask-cname-buydomains-6.png)

    > [AZURE.NOTE] Sie können innerhalb von 5 Tagen auf "Cancel purchase" klicken, um den Kauf unter vollständiger Rückerstattung der Kosten zu stornieren. Nach 5 Tagen wird anstelle von "Cancel purchase" eine Option zum Löschen der Domäne angezeigt. Wenn Sie die Domäne löschen, wird diese ohne Erstattung aus dem Abonnement entfernt und wieder als verfügbare Domäne aufgeführt.

Sobald die Konfiguration abgeschlossen ist, wird der benutzerdefinierte Domänenname im Abschnitt **Hostname bindings** Ihrer Web-App aufgeführt.

Jetzt sollten Sie den benutzerdefinierten Domänennamen in Ihren Browser eingeben können und und auf diese Weise erfolgreich zu Ihrer Web-App gelangen.
 
## Was geschieht mit der benutzerdefinierten Domäne, die Sie gekauft haben?

Die benutzerdefinierte Domäne, die Sie auf dem Blatt **Benutzerdefinierte Domänen und SSL** gekauft haben, ist an das Azure-Abonnement gebunden. Als Azure-Ressource ist diese benutzerdefinierte Domäne getrennt und unabhängig von der App Service-App, für die Sie die Domäne zuerst gekauft haben. Dies bedeutet Folgendes:

- Im Azure-Portal können Sie die benutzerdefinierte Domäne, die Sie gekauft haben, für mehrere App Service-Apps verwenden, und nicht nur für die App, für die Sie die benutzerdefinierte Domäne zuerst gekauft haben.
- Um alle benutzerdefinierten Domänen, die Sie gekauft haben, im Azure-Abonnement zu verwalten, können Sie das Blatt **Benutzerdefinierte Domänen und SSL** einer *beliebigen* App Service-App in diesem Abonnement aufrufen.
- Sie können jede App Service-App aus dem gleichen Azure-Abonnement einer Unterdomäne innerhalb der benutzerdefinierten Domäne zuweisen.
- Wenn Sie eine App Service-App löschen möchten, können Sie wahlweise darauf verzichten, die benutzerdefinierte Domäne zu löschen, an die sie gebunden ist, wenn Sie sie für andere Apps weiterhin verwenden möchten.

## Was ist, wenn die benutzerdefinierte Domäne, die Sie gekauft haben, nicht angezeigt wird?

Wenn Sie die benutzerdefinierte Domäne über das Blatt **Benutzerdefinierte Domänen und SSL** gekauft haben, unter **Verwaltete Domänen** jedoch keine benutzerdefinierte Domäne angezeigt wird, überprüfen Sie Folgendes:

- Die Erstellung der benutzerdefinierten Domäne wurde möglicherweise noch nicht abgeschlossen. Überprüfen Sie die Anzeige der Benachrichtigungsglocke am oberen Rand des Azure-Portals für den Verlauf.
- Die Erstellung der benutzerdefinierten Domäne ist möglicherweise aus irgendeinem Grund fehlgeschlagen. Überprüfen Sie die Anzeige der Benachrichtigungsglocke am oberen Rand des Azure-Portals für den Verlauf.
- Die benutzerdefinierte Domäne wurde möglicherweise erfolgreich erstellt, aber das Blatt möglicherweise noch nicht aktualisiert. Versuchen Sie, das Blatt **Benutzerdefinierte Domänen und SSL** erneut zu öffnen.
- Möglicherweise haben Sie die benutzerdefinierte Domäne zu einem bestimmten Zeitpunkt gelöscht. Überprüfen Sie die Überwachungsprotokolle durch Klicken auf **Einstellungen** > **Überwachungsprotokolle** auf dem Hauptblatt Ihrer App.
- Das Blatt **Benutzerdefinierte Domänen und SSL**, auf dem Sie suchen, gehört vielleicht zu einer App, die in einem anderen Azure-Abonnement erstellt wurde. Wechseln Sie zu einer anderen App in einem anderen Abonnement, und überprüfen Sie das Blatt **Benutzerdefinierte Domänen und SSL**. Innerhalb des Portals können Sie keine benutzerdefinierten Domänen anzeigen oder verwalten, die in einem anderen Azure-Abonnement erstellt wurden als die App. Wenn Sie jedoch auf dem zu der Domäne gehörenden Blatt **Domäne verwalten** auf **Erweiterte Verwaltung** klicken, werden Sie zur der Website des Domänennamenanbieters weitergeleitet, wo Sie für Apps, die in einem anderen Azure-Abonnement erstellt wurden, [Ihre benutzerdefinierte Domäne wie alle externen Domänen manuell konfigurieren](web-sites-custom-domain-name.md) können.

<!---HONumber=AcomDC_0921_2016-->
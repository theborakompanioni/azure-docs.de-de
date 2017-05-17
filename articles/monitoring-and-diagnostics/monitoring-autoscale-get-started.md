---
title: Erste Schritte mit der automatischen Skalierung in Azure | Microsoft-Dokumentation
description: Erfahren Sie, wie Sie Ihre Ressource in Azure skalieren.
author: rajram
manager: rboucher
editor: 
services: monitoring-and-diagnostics
documentationcenter: monitoring-and-diagnostics
ms.assetid: d37d3fda-8ef1-477c-a360-a855b418de84
ms.service: monitoring-and-diagnostics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/07/2017
ms.author: rajram
ms.translationtype: Human Translation
ms.sourcegitcommit: 71fea4a41b2e3a60f2f610609a14372e678b7ec4
ms.openlocfilehash: a0a420adc57a0bcbb3779183324ab7bfafcb5bf1
ms.contentlocale: de-de
ms.lasthandoff: 05/10/2017


---
# <a name="get-started-with-auto-scale-in-azure"></a>Erste Schritte mit der automatischen Skalierung in Azure
In diesem Artikel wird beschrieben, wie Sie Ihre automatische Skalierungseinstellung für Ihre Ressource im Azure-Portal einrichten.

Die automatische Skalierung von Azure Monitor gilt nur für VM-Skalierungsgruppen (VMSS), Clouddienste, App Service-Pläne und App Service-Umgebungen. 

# <a name="lets-get-started"></a>Erste Schritte

## <a name="discover-the-auto-scale-settings-in-your-subscriptions"></a>Ermitteln der Einstellungen der automatischen Skalierung in Abonnements
Sie können alle Ressourcen ermitteln, für die die automatische Skalierung in Azure Monitor angewendet wird. Führen Sie die nachfolgenden Schritte für eine schrittweise Anleitung durch.

- Öffnen Sie das [Azure-Portal][1].
- Klicken Sie im linken Navigationsbereich auf das Azure Monitor-Symbol.
  ![Azure Monitor starten][2]
- Klicken Sie auf die Autoskalierungseinstellung, um alle Ressourcen, für die die automatische Skalierung angewendet wird, zusammen mit dem aktuellen Autoskalierungsstatus anzuzeigen. ![Ermitteln der automatischen Skalierung in Azure Monitor][3]

Über den Filterbereich im oberen Bereich können Sie die Liste nach unten verschieben, um Ressourcen in einer bestimmten Ressourcengruppe, bestimmte Ressourcentypen oder eine bestimmte Ressource auszuwählen.

Zu jeder Ressource finden Sie die aktuelle Anzahl der Instanzen sowie den jeweiligen Autoskalierungsstatus. Der Autoskalierungsstatus kann wie folgt lauten:

- „Nicht konfiguriert“: Sie haben die automatische Skalierung für diese Ressource noch nicht aktiviert.
- „Aktiviert“: Sie haben die Autoskalierungseinstellung für diese Ressource aktiviert.
- „Deaktiviert“: Sie haben die Autoskalierungseinstellung für diese Ressource deaktiviert.

## <a name="create-your-first-auto-scale-setting"></a>Erstellen der ersten Autoskalierungseinstellung

Nun können Sie eine einfache schrittweise exemplarische Vorgehensweise zum Erstellen Ihrer ersten Einstellung für die automatische Skalierung durchgehen.

- Öffnen Sie in Azure Monitor das Blatt „Automatisch skalieren“, und wählen Sie eine Ressource aus, die skaliert werden soll. (Die folgenden Schritte beruhen auf einem App Service-Plan, der einer Web-App zugeordnet ist. Sie können [Ihre erste ASP.NET-Web-App in Azure in fünf Minuten erstellen][4].)
- Beachten Sie, dass die aktuelle Anzahl der Instanzen für die Ressource auf dem Blatt „Skalierungseinstellung“ 1 beträgt. Klicken Sie auf „Automatische Skalierung aktivieren“.
  ![Skalierungseinstellung für die neue Web-App][5]
- Geben Sie einen Namen für die Skalierungseinstellung an, und klicken Sie dann auf „Regel hinzufügen“. Beachten Sie die Optionen für die Skalierungsregel, die auf der rechten Seite als Kontextbereich geöffnet wird. Standardmäßig wird die Option zum Skalieren der Anzahl Ihrer Instanzen auf „1“ gesetzt, wenn der CPU-Prozentsatz der Ressource 70 % überschreitet. Behalten Sie die Standardwerte bei, und klicken Sie auf „Hinzufügen“.
  ![Erstellen der Skalierungseinstellung für eine Web-App][6]
- Nun haben Sie Ihre erste Skalierungsregel erstellt. Beachten Sie, dass die UX bewährte Methoden empfiehlt und angibt, dass empfohlen wird, mindestens eine Skala in der Regel einzufügen. Klicken Sie hierzu auf „Regel hinzufügen“, und setzen Sie „Operator“ auf „Kleiner als“, „Schwellenwert“ auf „20“ und „Vorgang“ auf „Anzahl verringern um“. Sie sollten nun über eine Skalierungseinstellung verfügen, die basierend auf der CPU-Auslastung zentral hoch- bzw. herunterskaliert.
  ![Skalieren basierend auf der CPU][8]
- Klicken Sie auf „Speichern“.

Herzlichen Glückwunsch. Sie haben nun Ihre erste Skalierungseinstellung erfolgreich für die automatische Skalierung Ihrer Web-App basierend auf der CPU-Auslastung konfiguriert.

> Hinweis: Diese Schritte gelten auch für die ersten Schritte mit VMSS oder einer Clouddienstrolle.

# <a name="other-considerations"></a>Weitere Überlegungen
## <a name="scale-based-on-a-schedule"></a>Skalieren basierend auf einem Zeitplan
Neben der Skalierung basierend auf der CPU können Sie auch festlegen, dass die Skalierung unterschiedlich an bestimmten Wochentage skaliert werden soll.

- Klicken Sie auf „Skalierungsbedingung hinzufügen“.
- Legen Sie den Skalierungsmodus fest. Es gelten dieselben Regeln wie bei der Standardbedingung.
- Wählen Sie für den Zeitplan „An bestimmten Tagen wiederholen“ aus.
- Wählen Sie die Tage sowie Start- bzw. Endzeit aus, an denen bzw. an der die Skalierungsbedingung für die ausgewählten Tage angewendet werden soll.

![Skalierungsbedingung basierend auf einem Zeitplan][9]
## <a name="scale-differently-on-specific-dates"></a>Individuelles Skalieren an bestimmten Daten
Neben der Skalierung basierend auf der CPU können Sie auch festlegen, dass die Skalierung unterschiedlich an bestimmten Daten skaliert werden soll.

- Klicken Sie auf „Skalierungsbedingung hinzufügen“.
- Legen Sie den Skalierungsmodus fest. Es gelten dieselben Regeln wie bei der Standardbedingung.
- Wählen Sie für den Zeitplan „Start-/Enddatum angeben“ aus.
- Wählen Sie die Start- bzw. Enddaten sowie Start- bzw. Endzeit aus, an denen bzw. an der die Skalierungsbedingung für die ausgewählten Daten angewendet werden soll.

![Skalierungsbedingung basierend auf Daten][10]

## <a name="view-the-scale-history-of-your-resource"></a>Anzeigen des Skalierungsverlaufs der Ressource
Wenn die Ressource zentral hoch- oder herunterskaliert wird, wird ein Ereignis im Aktivitätsprotokoll protokolliert. Sie können den Skalierungsverlauf der Ressource der letzten 24 Stunden anzeigen, indem Sie zur Registerkarte „Verlauf ausführen“ wechseln.

![Ausführungsverlauf][11]

Wenn Sie den gesamten Skalierungsverlauf (für bis zu 90 Tage) anzeigen möchten, können Sie auf „Klicken Sie hier, um weitere Details anzuzeigen.“ klicken. Hierdurch wird das Aktivitätsprotokoll mit der Ressource und der vorausgewählten Kategorie „Automatisch skalieren“ aufgerufen.

## <a name="view-the-scale-definition-of-the-resource"></a>Anzeigen der Skalierungsdefinition der Ressource
Die Autoskalierungseinstellung ist eine ARM-Ressource. Sie können die Skalierungsdefinition im JSON-Format anzeigen, indem Sie zur Registerkarte „JSON“ wechseln.

![Skalierungsdefinition][12]

Bei Bedarf können Sie Änderungen direkt in JSON vornehmen. Diese Änderungen werden beim Speichern übernommen.

## <a name="disable-autoscale-and-manually-scale-your-instances"></a>Deaktivieren der automatischen Skalierung und manuelles Skalieren der Instanzen
Es kann möglicherweise vorkommen, dass Sie Ihre aktuelle Skalierungseinstellung deaktivieren und die Ressource manuell skalieren müssen.

Klicken Sie oben auf die Schaltfläche „Automatische Skalierung deaktivieren“.
![Deaktivieren der automatischen Skalierung][13]

Beachten Sie, dass Ihre Konfiguration durch diese Option deaktiviert wird. Sie können jedoch nach wie vor zu dieser Einstellung zurückkehren, indem Sie die automatische Skalierung wieder aktivieren. Nun können Sie die Anzahl der Instanzen festlegen, die manuell skaliert werden sollen.

![Festlegen der manuellen Skalierung][14]

Sie können immer zur automatischen Skalierung zurückkehren, indem Sie auf „Automatische Skalierung aktivieren“ und dann auf „Speichern“ klicken.

<!--Reference-->
[1]:https://portal.azure.com
[2]: ./media/monitoring-autoscale-get-started/azure-monitor-launch.png
[3]: ./media/monitoring-autoscale-get-started/discover-autoscale-azure-monitor.png
[4]: https://docs.microsoft.com/en-us/azure/app-service-web/app-service-web-get-started-dotnet
[5]: ./media/monitoring-autoscale-get-started/scale-setting-new-web-app.png
[6]: ./media/monitoring-autoscale-get-started/create-scale-setting-web-app.png
[7]: ./media/monitoring-autoscale-get-started/scale-in-recommendation.png
[8]: ./media/monitoring-autoscale-get-started/scale-based-on-cpu.png
[9]: ./media/monitoring-autoscale-get-started/scale-condition-schedule.png
[10]: ./media/monitoring-autoscale-get-started/scale-condition-dates.png
[11]: ./media/monitoring-autoscale-get-started/scale-history.png
[12]: ./media/monitoring-autoscale-get-started/scale-definition-json.png
[13]: ./media/monitoring-autoscale-get-started/disable-autoscale.png
[14]: ./media/monitoring-autoscale-get-started/set-manualscale.png

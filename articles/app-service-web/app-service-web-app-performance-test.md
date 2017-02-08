---
title: Testen der Leistung einer Azure-Web-App | Microsoft-Dokumentation
description: "Führen Sie Azure-Web-App-Leistungstests aus, um zu überprüfen, wie Ihre App Benutzerlasten verarbeitet. Messen Sie die Antwortzeit, und finden Sie Fehler, die auf Probleme hinweisen können."
services: app-service\web
documentationcenter: 
author: ecfan
manager: douge
editor: jimbe
ms.assetid: e4858f40-038d-4039-a3d4-beee7960ac7d
ms.service: app-service-web
ms.workload: web
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 05/25/2016
ms.author: estfan; manasma; ahomer
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: 71f0a6e8d63e8f9ebb9307cce051010649a31a53


---
# <a name="performance-test-your-azure-web-app-under-load"></a>Leistungstest der Azure-Web-App unter Last
Testen Sie die Leistung Ihrer Web-App, bevor Sie sie starten oder Updates für die Produktion bereitstellen. Auf diese Weise können Sie besser beurteilen, ob Ihre App zur Veröffentlichung bereit ist. Und Sie können sicher sein, dass Ihre App den Datenverkehr zu Spitzenlastzeiten oder während Ihrer nächsten Marketingkampagne verarbeiten kann.

Während der öffentlichen Vorschau können Sie Ihre App kostenlos im Azure-Portal testen.
Diese Tests simulieren über einen bestimmten Zeitraum eine bestimmte Benutzerlast für Ihre App und messen die Antwortzeit der App. Die Testergebnisse zeigen beispielsweise, wie schnell Ihre App bei einer bestimmten Anzahl von Benutzern reagiert. Sie zeigen auch, bei wie vielen Anforderungen Fehler aufgetreten sind, was auf Probleme mit der App hinweisen kann.      

![Leistungsprobleme in Ihrer Web-App finden](./media/app-service-web-app-performance-test/azure-np-perf-test-overview.png)

## <a name="before-you-start"></a>Vorbereitung
* Sie benötigen ein [Azure-Abonnement](https://account.windowsazure.com/subscriptions), falls Sie noch keins besitzen. Erfahren Sie, wie Sie [kostenlos ein Azure-Konto anlegen](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F).
* Sie benötigen ein Konto für [Visual Studio Team Services](https://www.visualstudio.com/products/what-is-visual-studio-online-vs) , um den Verlauf der Leistungstests zu speichern. Ein geeignetes Konto wird automatisch erstellt, wenn Sie Ihren Leistungstest einrichten. Sie können ein neues Konto erstellen oder ein vorhandenes Konto verwenden, wenn Sie der Kontobesitzer sind. 
* Sie können Ihre App zum Testen in einer nicht produktiven Umgebung bereitstellen. 
  Ihre App kann einen anderen App Service-Plan als in der Produktion verwenden. 
  So wirken sich Tests nicht auf vorhandene Kunden aus oder verlangsamen Ihre App in der Produktion. 

## <a name="set-up-and-run-your-performance-test"></a>Einrichten und Ausführen des Leistungstests
1. Melden Sie sich beim [Azure-Portal](https://portal.azure.com)an. 
   Um ein Visual Studio Team Services-Konto zu verwenden, dessen Besitzer Sie sind, melden Sie sich als Kontobesitzer an.
2. Wechseln Sie zu Ihrer Web-App.
   
   ![Zu "Alles durchsuchen" > "Web-Apps" > "Ihre App" wechseln](./media/app-service-web-app-performance-test/azure-np-web-apps.png)
3. Wechseln Sie zur **Leistungstest**.
   
   ![Zu "Tools" > "Leistungstest" wechseln](./media/app-service-web-app-performance-test/azure-np-web-app-details-tools-expanded.png)
4. Erstellen Sie jetzt eine Verknüpfung mit einem [Visual Studio Team Services](https://www.visualstudio.com/products/what-is-visual-studio-online-vs) -Konto, um den Verlauf der Leistungstests zu speichern.
   
    Wenn Sie über ein verwendbares Team Services-Konto verfügen, wählen Sie dieses Konto aus. Andernfalls erstellen Sie ein neues Konto.
   
    ![Vorhandenes Team Services-Konto auswählen oder neues Konto erstellen](./media/app-service-web-app-performance-test/azure-np-no-vso-account.png)
5. Erstellen Sie den Leistungstest. Geben Sie die Details an, und führen Sie den Test aus. 

Sie können die Ergebnisse während der Testausführung in Echtzeit anzeigen.

Angenommen, Sie hätten eine App, die im Weihnachtsgeschäft des letzten Jahres Coupons verschenkt hat. Das Ereignis dauerte 15 Minuten, und zu Spitzenzeiten waren 100 Kunden gleichzeitig anwesend. Dieses Jahr möchten Sie die Kundenzahl verdoppeln. Sie möchten auch die Kundenzufriedenheit verbessern, indem Sie die Seitenladezeit von 5 auf 2 Sekunden verkürzen. Sie testen also die Leistung Ihrer App über einen Zeitraum von 15 Minuten mit 250 Benutzern.

Sie simulieren die Last Ihrer App, indem Sie virtuelle Benutzer (Kunden) erzeugen, die Ihre Website zur gleichen Zeit besuchen. So erfahren Sie, bei wie vielen Anforderungen Fehler auftreten oder die Reaktion zu langsam ist.

  ![Leistungstest erstellen, einrichten und ausführen](./media/app-service-web-app-performance-test/azure-np-new-performance-test.png)

* Die Standard-URL Ihrer Web-App wird automatisch hinzugefügt. 
  Sie können die URL ändern, um andere Seiten zu testen (nur HTTP GET-Anforderungen).
* Um die lokalen Bedingungen zu simulieren und die Latenz zu verringern, wählen Sie einen Ort, der sich möglichst in der Nähe der Benutzer befindet, die die Last generieren.
  
  Hier sehen Sie den Testfortschritt. Während der ersten Minute lädt die Seite langsamer als Ihnen lieb sein kann.
  
  ![Leistungstest während der Ausführung mit Echtzeitdaten](./media/app-service-web-app-performance-test/azure-np-running-perf-test.png)
  
  Nach Abschluss des Tests ist klar, dass die Seite nach der ersten Minute viel schneller lädt. So können Sie herausfinden, wo Sie mit der Behebung des Problems beginnen sollten.
  
  ![Abgeschlossener Leistungstests mit Ergebnissen, einschließlich Anforderungen mit Fehlern](./media/app-service-web-app-performance-test/azure-np-perf-test-done.png)

## <a name="test-multiple-urls"></a>Testen mehrerer URLs
Sie können auch Leistungstest mit mehreren URLs durchführen, indem Sie eine Visual Studio-Webtestdatei hochladen. Damit können Sie ein Ende-zu-Ende-Benutzerszenario darstellen. Anhand der folgenden Verfahren können Sie eine Visual Studio Webtestdatei erstellen:

* [Erfassen Sie den Datenverkehr mithilfe von Fiddler, und exportieren Sie die Ergebnisse als Visual Studio-Webtestdatei.](http://docs.telerik.com/fiddler/Save-And-Load-Traffic/Tasks/VSWebTest)
* [Erstellen Sie eine Auslastungstestdatei in Visual Studio.](https://www.visualstudio.com/docs/test/performance-testing/run-performance-tests-app-before-release)

So laden Sie eine Visual Studio-Webtestdatei hoch und führen sie aus

1. Befolgen Sie die Schritte oben zum Öffnen des Blatts **Neuer Leistungstest** .
   Wählen Sie auf diesem Blatt die Option „CONFIGURE TEST USING“ (TEST KONFIGURIEREN MIT) aus, um das Blatt **Configure test using** (Test konfigurieren mit) zu öffnen.  
   
    ![Das Blatt zum Konfigurieren von Auslastungstests öffnen](./media/app-service-web-app-performance-test/multiple-01-authoring-blade.png)
2. Überprüfen Sie, ob als „TESTTYP“ **Visual Studio Web Test** (Visual Studio-Webtest) festgelegt ist, und wählen Sie die HTTP-Archivdatei aus.
    Verwenden Sie das Ordnersymbol, um das Dialogfeld für die Dateiauswahl zu öffnen.
   
    ![Eine Visual Studio-Webtestdatei für mehrere URLs hochladen](./media/app-service-web-app-performance-test/multiple-01-authoring-blade2.png)
   
    Nachdem die Datei hochgeladen wurde, wird im Abschnitt „URL DETAILS“ die Liste der zu testenden URLs angezeigt.
3. Geben Sie die Benutzerauslastung und die Testdauer an, und wählen Sie dann **Test ausführen**aus.
   
    ![Benutzerauslastung und Dauer auswählen](./media/app-service-web-app-performance-test/multiple-01-authoring-blade3.png)
   
    Nachdem der Test abgeschlossen ist, werden die Ergebnisse in zwei Bereichen angezeigt. Im linken Bereich werden die Leistungsdaten als eine Reihe von Diagrammen dargestellt.
   
    ![Der Ergebnisbereich](./media/app-service-web-app-performance-test/multiple-01a-results.png)
   
    Im rechten Bereich wird eine Liste der fehlgeschlagenen Anforderungen zusammen mit dem Fehlertyp und der Anzahl von Vorkommen angezeigt.
   
    ![Der Bereich mit den Anforderungsfehlern](./media/app-service-web-app-performance-test/multiple-01b-results.png)
4. Führen Sie den Test erneut aus, indem Sie im rechten Bereich oben das Symbol **Erneut ausführen** auswählen.
   
    ![Den Test erneut ausführen](./media/app-service-web-app-performance-test/multiple-rerun-test.png)

## <a name="q--a"></a>Fragen und Antworten
#### <a name="q-is-there-a-limit-on-how-long-i-can-run-a-test"></a>F: Gibt es eine Beschränkung, wie lange ich einen Test ausführen kann?
**A:** Ja. Sie können den Test bis zu eine Stunde lang im Azure-Portal ausführen.

#### <a name="q-how-much-time-do-i-get-to-run-performance-tests"></a>F: Wie viel Zeit habe ich zum Ausführen von Leistungstests?
**A:**Nach der öffentlichen Vorschau erhalten Sie jeden Monat 20.000 virtuelle Benutzerminuten (Virtual User Minute, VUM) kostenlos mit Ihrem Visual Studio Team Services-Konto. Bei einer VUM handelt es sich um die Anzahl an Benutzern multipliziert mit der Anzahl an Minuten in Ihrem Test. Wenn Ihre Anforderungen über dieses kostenlose Kontingent hinausgehen, können Sie mehr Zeit erwerben und bezahlen dabei nur die Zeit, die Sie wirklich verwenden.

#### <a name="q-where-can-i-check-how-many-vums-ive-used-so-far"></a>F: Wo kann ich prüfen, wie viele virtuelle Benutzerminuten ich bereits verwendet habe?
**A:**Sie können die Anzahl im Azure-Portal prüfen.

![Team Services-Konto aufrufen](./media/app-service-web-app-performance-test/azure-np-vso-accounts.png)

![Verwendete VUM überprüfen](./media/app-service-web-app-performance-test/azure-np-vso-accounts-vum-summary.png)

#### <a name="q-what-is-the-default-option-and-are-my-existing-tests-impacted"></a>F: Was ist die Standardoption, und sind meine vorhandenen Tests betroffen?
**A**: Die Standardoption für Leistungstests zur Auslastung ist wie zuvor ein manueller Test – die Option zum Testen mehrerer URLs wurde im Portal hinzugefügt.
Ihre vorhandenen Tests verwenden weiterhin die konfigurierte URL und funktionieren wie bisher.

#### <a name="q-what-features-not-supported-in-the-visual-studio-web-test-file"></a>F: Welche Features werden in der Visual Studio-Webtestdatei nicht unterstützt?
**A:**Zurzeit unterstützt dieses Feature keine Webtest-Plug-Ins, Datenquellen und Extraktionsregeln. Sie müssen die Webtestdatei bearbeiten und diese entfernen. Wir hoffen, eine Unterstützung für diese Features in zukünftigen Updates hinzufügen zu können.

#### <a name="q-does-it-support-any-other-web-test-file-formats"></a>F: Werden noch andere Webtest-Dateiformate unterstützt?
**A:**Zurzeit werden nur Dateien im Format von Visual Studio-Webtestdateien unterstützt.
Sie können uns gerne mitteilen, wenn Sie Unterstützung für andere Dateiformate benötigen. Senden Sie uns eine E-Mail an [vsoloadtest@microsoft.com](mailto:vsoloadtest@microsoft.com).

#### <a name="q-what-else-can-i-do-with-a-visual-studio-team-services-account"></a>F: Welche anderen Möglichkeiten habe ich mit einem Visual Studio Team Services-Konto?
**A:** Sie finden Ihr neues Konto hier: ```https://{accountname}.visualstudio.com```. Code freigeben, Software erstellen, testen, nachverfolgen und ausliefern – all dies in der Cloud, mit jedem Tool und in jeder Sprache. Erfahren Sie, wie die Funktionen und Dienste von [Visual Studio Team Services](https://www.visualstudio.com/products/what-is-visual-studio-online-vs) Ihr Team dabei unterstützen, noch einfacher zusammenzuarbeiten und kontinuierlich neue Apps bereitzustellen.

## <a name="see-also"></a>Weitere Informationen
* [Run simple cloud performance tests](https://www.visualstudio.com/docs/test/performance-testing/getting-started/get-started-simple-cloud-load-test)
* [Run Apache Jmeter performance tests](https://www.visualstudio.com/docs/test/performance-testing/getting-started/get-started-jmeter-test)
* [Record and replay cloud-based load tests](https://www.visualstudio.com/docs/test/performance-testing/getting-started/record-and-replay-cloud-load-tests)
* [Performance test your app in the cloud](https://www.visualstudio.com/docs/test/performance-testing/getting-started/getting-started-with-performance-testing)




<!--HONumber=Nov16_HO3-->



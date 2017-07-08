---
title: "MyDriving-Beispiel für Azure IoT – Schnellstart | Microsoft Docs"
description: "Erste Schritte mit einer App, die in einer umfassenden Demonstration zeigt, wie Sie mit Microsoft Azure ein IoT-System einschließlich Stream Analytics, Machine Learning und Event Hubs zusammenstellen."
services: 
documentationcenter: .net
suite: 
author: harikmenon
manager: douge
ms.assetid: f40ea71b-5721-4a6b-a886-53c2e9dffe8f
ms.service: multiple
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.devlang: dotnet
ms.topic: article
ms.date: 03/25/2016
ms.author: harikm
ms.translationtype: Human Translation
ms.sourcegitcommit: dcda8b30adde930ab373a087d6955b900365c4cc
ms.openlocfilehash: 031b492df1f186087e7b91102cbb44f552999293
ms.contentlocale: de-de
ms.lasthandoff: 07/06/2017


---
# <a name="mydriving-iot-system-quick-start"></a>IoT-System „MyDriving“: Schnellstart
MyDriving ist ein System, das den Entwurf und die Implementierung einer typischen [Internet der Dinge](iot-suite-overview.md) (IoT)-Lösung veranschaulicht, die Telemetriedaten von Geräten erfasst, diese Daten in der Cloud verarbeitet und mithilfe von Machine Learning eine adaptive Reaktion bereitstellt. In der Demo werden Daten zu Ihren Autofahrten sowohl von Ihrem Mobiltelefon als auch einem Adapter protokolliert, der Informationen vom Steuerungssystem Ihres Fahrzeugs erfasst. Anhand dieser Daten wird Feedback zu Ihrem Fahrstil im Vergleich zu anderen Benutzern bereitgestellt.

Der eigentliche Zweck von MyDriving besteht darin, Sie beim Einstieg in die Erstellung einer eigenen IoT-Lösung zu unterstützen. Zunächst erhalten Sie jedoch eine Einführung in die MyDriving-App an sich – als Mitglied unseres Testbenutzerteams. So können Sie sich als Verbraucher mit der Verwendung der App und dem zugrunde liegenden System vertraut machen, bevor Sie sich eingehender mit der Architektur befassen. Zudem erhalten Sie eine Einführung in HockeyApp – eine praktische Möglichkeit zum Verwalten von Alpha- und Beta-Distributionen Ihrer Apps an Testbenutzer.

## <a name="use-the-mobile-experience"></a>Verwenden der mobilen Oberfläche
Sie können die MyDriving-App verwenden, wenn Sie ein Android-, iOS- oder Windows 10-Gerät besitzen.

### <a name="android-and-windows-10-mobile-installation"></a>Installation unter Android und Windows 10 Mobile
Auf Ihrem Gerät:

1. Entwicklungs-Apps zulassen:
   
   * Android: Lassen Sie unter **Einstellungen** > **Sicherheit** Apps aus **unbekannten Quellen** zu.
   * Windows 10: Legen Sie unter **Einstellungen** > **Updates** > **Für Entwickler** den **Entwicklermodus** fest.
2. Treten Sie unserem Betatestteam bei, indem Sie sich bei [HockeyApp](https://rink.hockeyapp.net)registrieren oder dort anmelden. HockeyApp erleichtert die Verteilung früher Veröffentlichungen Ihrer App an Testbenutzer.
   
   Verwenden Sie unter Windows 10 den Edge-Browser.
   
   Melden Sie sich als Build 2016-Teilnehmer über eine der Microsoft-Schaltflächen an, und verwenden Sie dabei die gleiche Microsoft-Konto-E-Mail-Adresse wie bei der Registrierung für die Konferenz. Sie sind bereits bei HockeyApp registriert.
   
   ![Anmeldeseite von HockeyApp](./media/iot-solution-get-started/image1.png)
3. Hier können Sie die App herunterladen und installieren:
   
   * [Android](http://rink.io/spMyDrivingAndroid)
   * [Windows 10](http://rink.io/spMyDrivingUWP)
   
   Der Vorgang umfasst zwei Schritte: Installieren Sie das Zertifikat in **Vertrauenswürdige Personen**. Installieren Sie dann die App.

*Sind beim Starten der App unter Windows 10 Mobile Probleme aufgetreten?* Vielleicht liegt Ihr Telefon ein oder zwei Updates zurück. Vergewissern Sie sich, dass Sie über die aktuellen Updates verfügen, oder installieren Sie:

* [Microsoft.NET.Native.Framework.1.2.appx](https://download.hockeyapp.net/packages/win10/Microsoft.NET.Native.Framework.1.2.appx) 
* [Microsoft.NET.Native.Runtime.1.1.appx](https://download.hockeyapp.net/packages/win10/Microsoft.NET.Native.Runtime.1.1.appx) 
* [Microsoft.VCLibs.ARM.14.00.appx](https://download.hockeyapp.net/packages/win10/Microsoft.VCLibs.ARM.14.00.appx)

### <a name="ios-installation"></a>iOS-Installation
Wenn Sie an Build 2016 teilgenommen haben, laden Sie die App als Mitglied unseres HockeyApp-Testteams herunter.

1. Melden Sie sich auf Ihrem iOS-Gerät bei [HockeyApp](https://rink.hockeyapp.net)an.
   Melden Sie sich über eine der Microsoft-Anmeldeschaltflächen an, und verwenden Sie dabei die gleiche Microsoft-Konto-Mail-Adresse wie bei der Registrierung für die Konferenz. (Verwenden Sie nicht die Felder für E-Mail-Adresse und Kennwort.)
   
   ![Anmeldeseite von HockeyApp](./media/iot-solution-get-started/image1.png)
2. Wählen Sie im HockeyApp-Dashboard „MyDriving“ aus, und laden Sie es herunter.
3. Autorisieren der Betaversion von HockeyApp:
   
   a. Wechseln Sie zu **Einstellungen** > **Allgemein** > **Profile und Geräteverwaltung.**
   
   b. Vertrauen Sie dem Zertifikat **Bit Stadium GmbH** .

Wenn Sie nicht an Build 2016 teilgenommen haben, können Sie die App selbst erstellen und bereitstellen:

1. Laden Sie den Code [von GitHub]herunter.
2. [Verwenden Sie Xamarin]zum Erstellen und Bereitstellen.

Weitere Informationen finden Sie im [MyDriving-Referenzhandbuch](http://aka.ms/mydrivingdocs).

## <a name="get-an-obd-adapter-optional"></a>OBD-Adapter erwerben (optional)
Dieser Adapter macht ein wahres „Internet der Dinge“-System aus! Er ist für die Verwendung der App nicht erforderlich, sorgt aber für mehr Spaß, da Sie den vollen Funktionsumfang nutzen können, und er ist nicht teuer.

Mithilfe der On-Board-Diagnose (On-Board Diagnostics, OBD) stellt die Werkstatt Ihr Auto auf optimale Leistung ein und stellt fest, warum merkwürdige Geräusche entstehen und Kontrollleuchten aufleuchten. Sofern Ihr Auto keine Antiquität ist, finden Sie irgendwo im Innenraum einen Anschluss, in der Regel hinter einer Klappe am Armaturenbrett. Mit dem richtigen Stecker können Sie Metriken der Motorleistung abrufen und bestimmte Anpassungen vornehmen. Einen handelsüblichen OBD-Stecker können Sie günstig erwerben. Die Verbindung mit der App auf Ihrem Telefon wird über Bluetooth oder WLAN hergestellt.

In diesem Fall verbinden wir Ihr Auto mit der Cloud. Die direkte Verbindung besteht zwar zwischen dem OBD und Ihrem Telefon, unsere App funktioniert jedoch als Relais. Die Telemetriedaten Ihres Autos werden direkt an den IoT Hub von MyDriving gesendet, wo sie verarbeitet werden, um ein Protokoll Ihrer Fahrten zu erstellen und Ihren Fahrstil zu bewerten.

Verbinden eines OBD-Geräts:

1. Überprüfen Sie, ob Ihr Auto einen OBD-Anschluss besitzt.
2. Kaufen Sie einen OBD-Adapter:
   
   * Wenn Sie ein Android- oder Windows-Telefon verwenden, benötigen Sie einen Bluetooth-fähigen OBD II-Adapter. Wir haben den [Scanner „34t5 Bluetooth OBDII“ von BAFX Products]verwendet.
   * Wenn Sie ein iOS-Telefon verwenden, benötigen Sie einen WLAN-fähigen OBD-Adapter. Wir haben den [OBD-Adapter/Diagnosescanner „ScanTool OBDLink MX Wi-Fi“]verwendet.
3. Folgen Sie den Anweisungen des jeweiligen OBD-Adapters, um die Verbindung mit Ihrem Mobiltelefon herzustellen. Berücksichtigen Sie Folgendes:
   
   * Ein Bluetooth-Adapter muss auf der Seite **Einstellungen** mit dem Mobiltelefon gekoppelt werden.
   * Ein WLAN-Adapter muss eine Adresse im Bereich „192.168.xxx.xxx“ aufweisen.
4. Wenn Sie mehrere Autos besitzen, können Sie für jedes einen eigenen Adapter (jedoch höchstens drei) verwenden.

Wenn Sie keinen OBD-Adapter haben, sendet die App trotzdem Standort- und Geschwindigkeitsdaten vom GPS-Empfänger des Telefons an das Back-End und fragt, ob Sie eine OBD simulieren möchten.

Weitere Informationen zur Verwendung der Daten des OBD-Adapters durch die App sowie zu Optionen zum Erstellen Ihres eigenen OBD-Geräts finden Sie in Abschnitt 2.1 „IoT-Geräte“ des [MyDriving-Referenzhandbuchs](http://aka.ms/mydrivingdocs).

## <a name="use-the-app"></a>Verwenden der App
Starten Sie die App. Ein Schnellstart am Anfang führt Sie Schritt für Schritt durch die Verwendung.

### <a name="track-your-trips"></a>Verfolgen Sie Ihre Fahrten.
Tippen Sie auf die Schaltfläche zum Aufzeichnen (großer roter Kreis am unteren Bildschirmrand), um eine Fahrt zu starten. Durch erneutes Tippen wird sie beendet.

![Abbildung der Aufzeichnungsschaltfläche für die Nachverfolgung der Fahrt](./media/iot-solution-get-started/image2.png)

Wenn kein OBD-Gerät vorhanden ist, werden Sie bei jedem Start einer Fahrt gefragt, ob Sie den Simulator verwenden möchten.

Tippen Sie am Ende der Fahrt auf die Schaltfläche zum Beenden, und Sie erhalten eine Zusammenfassung.

![Beispiel einer Fahrtzusammenfassung](./media/iot-solution-get-started/image3.png)

### <a name="review-your-trips"></a>Überprüfen Ihrer Fahrten
![Beispiel einer vergangenen Fahrt](./media/iot-solution-get-started/image4.png)

### <a name="review-your-profile"></a>Überprüfen Ihres Profils
![Beispiel eines Fahrstilprofils](./media/iot-solution-get-started/image5.png)

## <a name="send-us-your-test-feedback"></a>Senden Sie uns Ihr Testfeedback
Da wir MyDriving als Starthilfe für Ihre eigenen IoT-Systeme erstellt haben, würden wir uns freuen, wenn Sie uns Feedback zu Ihren Ergebnissen senden. Teilen Sie uns mit, ob:

* Sie auf Probleme oder Herausforderungen gestoßen sind.
* die App mit einer bestimmten Erweiterung für Ihr Szenario besser geeignet wäre.
* Sie eine effizientere Möglichkeit finden, bestimmte Anforderungen zu erfüllen.
* Sie andere Vorschläge haben, um MyDriving oder diese Dokumentation zu verbessern.

Innerhalb der MyDriving-App selbst können Sie den integrierten HockeyApp-Feedbackmechanismus nutzen: mit iOS und Android können Sie Ihr Telefon einfach schütteln oder den Menübefehl **Feedback** verwenden. Dadurch wird automatisch ein Screenshot angehängt, sodass wir wissen, wovon Sie sprechen. Und wenn die App unglücklicherweise abstürzen sollte, sammelt HockeyApp die Absturzprotokolle, um uns die Informationen zukommen zu lassen. Im [HockeyApp-Portal]können Sie ebenfalls Feedback hinterlassen.

Sie haben auch die Möglichkeit, ein [Problem bei GitHub] zu beschreiben oder unten einen Kommentar zu hinterlassen (EN-US-Version).

Wir freuen uns darauf, von Ihnen zu hören!

## <a name="next-steps"></a>Nächste Schritte
* Lesen Sie im [MyDriving-Referenzhandbuch](http://aka.ms/mydrivingdocs) nach, wie wir das gesamte MyDriving-System entworfen und erstellt haben.
* [selbst ein System zu erstellen und bereitzustellen](iot-solution-build-system.md) . Das [MyDriving-Referenzhandbuch](http://aka.ms/mydrivingdocs) führt Sie auch durch Bereiche, in denen Sie die meisten Anpassungen vornehmen müssen.

[von GitHub]: https://github.com/Azure-Samples/MyDriving
[Verwenden Sie Xamarin]: https://developer.xamarin.com/guides/ios/getting_started/installation/
[Scanner „34t5 Bluetooth OBDII“ von BAFX Products]: http://www.amazon.com/gp/product/B005NLQAHS
[OBD-Adapter/Diagnosescanner „ScanTool OBDLink MX Wi-Fi“]: http://www.amazon.com/gp/product/B00OCYXTYY/ref=s9_simh_gw_g263_i1_r?pf_rd_m=ATVPDKIKX0DER&pf_rd_s=desktop-2&pf_rd_r=1MWRMKXK4KK9VYMJ44MP
[HockeyApp-Portal]: https://rink.hockeyapp.org
[Problem bei GitHub]: https://github.com/Azure-Samples/MyDriving/issues


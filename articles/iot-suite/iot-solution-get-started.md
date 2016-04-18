<properties 
	pageTitle="MyDriving-Beispiel für Azure IoT – Schnellstart | Microsoft Azure" 
	description="Erste Schritte mit einer App, mit der umfassend demonstriert wird, wie Sie ein IoT-System mit Microsoft Azure zusammenstellen, einschließlich Stream Analytics, Machine Learning und Event Hubs." 
	services="" 
    documentationCenter=""
    suite="iot-suite"
	authors="alancameronwills" 
	manager="douge"/>

<tags 
	ms.service="iot-suite" 
	ms.workload="tbd" 
	ms.tgt_pltfrm="ibiza" 
	ms.devlang="na" 
	ms.topic="article" 
	ms.date="03/25/2016" 
	ms.author="awills"/>

# IoT-System „MyDriving“: Schnellstart

MyDriving ist ein System, das den Entwurf und die Implementierung einer typischen [Internet der Dinge](iot-suite-overview.md) (IoT)-Lösung veranschaulicht, die Telemetriedaten von Geräten erfasst, diese Daten in der Cloud verarbeitet und mithilfe von Machine Learning eine adaptive Reaktion bereitstellt. In der Demo werden Daten zu Ihren Autofahrten sowohl von Ihrem Mobiltelefon als auch einem OBD (On-Board-Diagnose)-Adapter protokolliert, der Informationen vom Steuerungssystem Ihres Fahrzeugs erfasst. Anhand dieser Daten wird Feedback zu Ihrem Fahrstil im Vergleich zu anderen Benutzern bereitgestellt.


![](./media/iot-solution-get-started/image5.png)

Der eigentliche Zweck von MyDriving besteht darin, Sie beim Einstieg in die Erstellung einer eigenen IoT-Lösung zu unterstützen. Zunächst erhalten Sie jedoch eine Einführung in die MyDriving-App an sich – als Mitglied unseres Testbenutzerteams. So können Sie sich als Verbraucher mit der Verwendung der App und dem zugrunde liegenden System vertraut machen, bevor Sie sich eingehender mit der Architektur befassen. Zudem erhalten Sie eine Einführung in HockeyApp – eine praktische Möglichkeit zum Verwalten von Alpha- und Beta-Distributionen Ihrer Apps an Testbenutzer.

## Verwenden der mobilen Oberfläche

**Voraussetzungen**:

Android-, iOS- oder Windows 10-Gerät.

## Installation unter Android und Windows Phone 10

Auf Ihrem Gerät:

1.  **Entwicklungs-Apps zulassen**

    -   Android: Lassen Sie unter **Einstellungen**, **Sicherheit** Apps aus **unbekannten Quellen** zu.

    -   Windows 10: Legen Sie unter **Einstellungen**, **Updates**, **Für Entwickler** den **Entwicklermodus** fest.

2.  **Unserem Betatestteam beitreten**.

    HockeyApp erleichtert die Verteilung früher Veröffentlichungen Ihrer App an Testbenutzer.

    Auf Ihrem Mobilgerät:

    -   **Registrieren Sie sich unter <https://rink.hockeyapp.net> für HockeyApp bzw. melden sich an**.
    
        (Verwenden Sie unter Windows 10 den Edge-Browser.)

        *Build 2016-Teilnehmer* – melden Sie sich über eine der Microsoft-Schaltflächen an, und verwenden Sie dabei dieselbe MSA-E-Mail-Adresse wie bei der Registrierung für die Konferenz. Sie sind bereits bei HockeyApp registriert.

        ![](./media/iot-solution-get-started/image1.png)

3.  Hier können Sie die App **herunterladen und installieren**:

    -   Android: <http://rink.io/spMyDrivingAndroid>

    -   Windows 10 Phone: <http://rink.io/spMyDrivingUWP>

        Der Vorgang umfasst zwei Schritte: Installieren Sie das Zertifikat in „Vertrauenswürdige Personen“ Installieren Sie dann die App.
    
*Treten beim Starten auf Windows Phone 10 Probleme auf?* Möglicherweise sind Updates für Ihr Telefon erforderlich. Vergewissern Sie sich, dass Sie über die aktuellen Updates verfügen, oder installieren Sie:

 - [Microsoft.NET.Native.Framework.1.2.appx](https://download.hockeyapp.net/packages/win10/Microsoft.NET.Native.Framework.1.2.appx) 
 - [Microsoft.NET.Native.Runtime.1.1.appx](https://download.hockeyapp.net/packages/win10/Microsoft.NET.Native.Runtime.1.1.appx) 
 - [Microsoft.VCLibs.ARM.14.00.appx](https://download.hockeyapp.net/packages/win10/Microsoft.VCLibs.ARM.14.00.appx) 
   

## iOS-Installation

### Build 2016-Teilnehmer

Wenn Sie an Build 2016 teilnehmen, laden Sie die App als Mitglied unseres HockeyApp-Testteams herunter.

1.  Melden Sie sich auf Ihrem iOS-Gerät bei <https://rink.hockeyapp.net> an. Melden Sie sich über eine der Microsoft-Anmeldeschaltflächen an, und verwenden Sie dabei dieselbe MSA-E-Mail-Adresse wie bei der Registrierung für die Konferenz. (Verwenden Sie nicht die Felder für E-Mail-Adresse und Kennwort.)

    ![](./media/iot-solution-get-started/image1.png)

2.  Wählen Sie im HockeyApp-Dashboard „MyDriving“ aus, und laden Sie es herunter.

3.  Autorisieren der Betaversion von HockeyApp:

    Wechseln Sie zu **Einstellungen** &gt;**Allgemein** &gt;**Profile und Geräteverwaltung**.

    Vertrauen Sie dem Zertifikat **Bit Stadium GmbH**.

### Falls Sie nicht an Build 2016 teilgenommen haben

Wir veröffentlichen die App in Kürze im iOS Store.

Vorerst können Sie die App selbst erstellen und bereitstellen:

-   Laden Sie den Code [von GitHub] herunter.

-   Verwenden Sie [Xamarin] zum Erstellen und Bereitstellen.

Weitere Informationen finden Sie im [MyDriving-Referenzhandbuch](http://aka.ms/mydrivingdocs).

## OBD-Adapter erwerben (optional)

Dieser Adapter macht ein wahres „Internet der Dinge“-System aus! Er ist für die Verwendung der App nicht erforderlich, sorgt aber für mehr Spaß, da Sie den vollen Funktionsumfang nutzen können, und er ist nicht teuer.

On-Board-Diagnose (On-Board Diagnostics, OBD) ist die Funktion, die von der Werkstatt bei der Inspektion verwendet wird und mit der ungewöhnliche Geräusche und Kontrollleuchten diagnostiziert werden. Sofern Ihr Auto nicht sehr alt ist, finden Sie unter dem Armaturenbrett eine Steckdose. Mit dem richtigen Stecker können Sie Metriken der Motorleistung abrufen und bestimmte Anpassungen vornehmen. Einen handelsüblichen OBD-Stecker können Sie günstig erwerben. Die Verbindung mit der App auf Ihrem Telefon wird über Bluetooth oder WLAN hergestellt.

In diesem Fall verbinden wir Ihr Auto jedoch mit der Cloud. Die direkte Verbindung besteht zwar zwischen dem OBD und Ihrem Telefon, unsere App funktioniert jedoch als Relais. Die Telemetriedaten Ihres Autos werden direkt an den IoT Hub von MyDriving gesendet, wo sie verarbeitet werden, um ein Protokoll Ihrer Fahrten zu erstellen und Ihren Fahrstil zu bewerten.

### Verbinden eines OBD-Geräts


1.  Vergewissern Sie sich, dass Ihr Auto über einen OBD-Anschluss verfügt (wenn Sie keinen Oldtimer fahren, ist dies wahrscheinlich der Fall). Er befindet sich im Innenraum, in der Regel hinter einer Klappe unter den Armaturenbrett.

2.  Kaufen Sie einen OBD-Adapter. Wir haben folgende Typen verwendet:

    Wenn Sie

    -   ein **Android- oder Windows**-Telefon verwenden, benötigen Sie einen **Bluetooth-fähigen OBD II**-Adapter. Wir haben den [Scanner „34t5 Bluetooth OBDII“ von BAFX Products] verwendet.

    -   ein **iOS**-Telefon verwenden, benötigen Sie einen **WLAN-fähigen** OBD-Adapter. Wir haben [den OBD-Adapter/Diagnosescanner „ScanTool OBDLink MX Wi-Fi“] verwendet.

3.  Folgen Sie den Anweisungen des jeweiligen OBD-Adapters, um die Verbindung mit Ihrem Mobiltelefon herzustellen.

    -   Ein Bluetooth-Adapter muss auf der Einstellungsseite mit dem Mobiltelefon gekoppelt werden.

    -   Ein WLAN-Adapter muss eine Adresse im Bereich „192.168.xxx.xxx“ aufweisen.

4.  Wenn Sie mehrere Autos besitzen, können Sie für jedes einen eigenen Adapter (jedoch höchstens drei) verwenden.

Wenn Sie keinen OBD-Adapter haben, sendet die App trotzdem Standort- und Geschwindigkeitsdaten vom GPS-Empfänger des Telefons an das Back-End und fragt, ob Sie eine OBD simulieren möchten.

Weitere Informationen zur Verwendung der Daten des OBD-Adapters durch die App sowie zu Optionen zum Erstellen Ihres eigenen OBD-Geräts finden Sie in Abschnitt 2.1 „IoT-Geräte“ des [MyDriving-Referenzhandbuchs](http://aka.ms/mydrivingdocs).

## Verwenden der App

**Starten** Sie die App. Ein Schnellstart am Anfang führt Sie Schritt für Schritt durch die Verwendung.

-   **Verfolgen Sie Ihre Fahrten nach:** Tippen Sie auf die Schaltfläche zum Aufzeichnen (großer roter Kreis am unteren Bildschirmrand) um eine Fahrt zu starten. Durch erneutes Tippen wird sie beendet.


    ![](./media/iot-solution-get-started/image2.png)

-   Wenn kein OBD-Gerät vorhanden ist, werden Sie bei jedem Start einer Fahrt gefragt, ob Sie den Simulator verwenden möchten.

-   Klicken Sie am Ende der Fahrt auf die Schaltfläche zum Beenden, und Sie erhalten Sie eine Zusammenfassung:

    ![](./media/iot-solution-get-started/image3.png)

-   **Überprüfen Ihrer Fahrten:**

    ![](./media/iot-solution-get-started/image4.png)

-   **Überprüfen Ihres Profils:**

    ![](./media/iot-solution-get-started/image5.png)

-   **Senden Sie uns Ihr Feedback zum Test** mithilfe der Schaltfläche in der App aus, oder schütteln Sie einfach Ihr Telefon! Dadurch wird automatisch ein Screenshot angehängt, sodass wir wissen, wovon Sie sprechen. Und wenn die App unglücklicherweise abstürzen sollte, sammelt HockeyApp die Absturzprotokolle, um uns die Informationen zukommen zu lassen.

## Feedback 

Da wir MyDriving als Starthilfe für Ihre eigenen IoT-Systeme erstellt haben, würden wir uns freuen, wenn Sie uns Feedback zu Ihren Ergebnissen senden. Teilen Sie uns mit, falls bei Ihnen Schwierigkeiten oder Probleme auftreten, falls eine Erweiterungsmöglichkeit besteht, um die Eignung für Ihr Szenario zu verbessern, falls Sie einen effizienteren Weg zur Erreichung bestimmter Ziele finden, oder falls Sie andere Vorschläge zur Verbesserung von MyDriving oder dieser Dokumentation haben.

Innerhalb der MyDriving-App selbst können Sie den integrierten HockeyApp-Feedbackmechanismus nutzen: mit iOS und Android müssen Sie Ihr Telefon einfach schütteln oder den Feedback-Menübefehl verwenden. Im [HockeyApp-Portal] können Sie ebenfalls Feedback hinterlassen.

Sie haben auch die Möglichkeit, ein [Problem bei GitHub] zu beschreiben oder unten einen Kommentar zu hinterlassen (de-DE-Version).

Wir freuen uns darauf, von Ihnen zu hören!

## Nächste Schritte

-   Lesen Sie im [MyDriving-Referenzhandbuch](http://aka.ms/mydrivingdocs) nach, um zu verstehen, wie wir das gesamte MyDriving-System entworfen und erstellt haben.

-   Verwenden Sie unsere Azure Resource Manager-Skripts, um [selbst ein System zu erstellen und bereitzustellen](iot-solution-build-system.md). Das [MyDriving-Referenzhandbuch](http://aka.ms/mydrivingdocs) führt Sie auch durch Bereiche, in denen Sie die meisten Anpassungen vornehmen müssen.

  [von GitHub]: https://github.com/Azure-Samples/MyDriving
  [Xamarin]: https://developer.xamarin.com/guides/ios/getting_started/installation/
  [Scanner „34t5 Bluetooth OBDII“ von BAFX Products]: http://www.amazon.com/gp/product/B005NLQAHS
  [den OBD-Adapter/Diagnosescanner „ScanTool OBDLink MX Wi-Fi“]: http://www.amazon.com/gp/product/B00OCYXTYY/ref=s9_simh_gw_g263_i1_r?pf_rd_m=ATVPDKIKX0DER&pf_rd_s=desktop-2&pf_rd_r=1MWRMKXK4KK9VYMJ44MP
  [HockeyApp-Portal]: https://rink.hockeyapp.org
  [Problem bei GitHub]: https://github.com/Azure-Samples/MyDriving/issues

<!---HONumber=AcomDC_0406_2016-->
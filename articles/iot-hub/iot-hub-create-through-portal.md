---
title: "Erstellen eines IoT Hubs über das Azure-Portal | Microsoft Docs"
description: "Erstellen, Verwalten und Löschen von Azure IoT Hubs über das Azure-Portal. Enthält Informationen zu Tarifen, Skalierung, Sicherheit und die Messagingkonfiguration."
services: iot-hub
documentationcenter: 
author: dominicbetts
manager: timlt
editor: 
ms.assetid: 0909cd2b-4c1e-49e0-b68a-75532caf0a6a
ms.service: iot-hub
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 05/02/2017
ms.author: dobett
ms.translationtype: Human Translation
ms.sourcegitcommit: e7da3c6d4cfad588e8cc6850143112989ff3e481
ms.openlocfilehash: 7a68a2764d17b53164ded5096ddee68ab8c723ed
ms.contentlocale: de-de
ms.lasthandoff: 05/16/2017


---
# <a name="create-an-iot-hub-using-the-azure-portal"></a>Erstellen eines IoT Hubs über das Portal
[!INCLUDE [iot-hub-resource-manager-selector](../../includes/iot-hub-resource-manager-selector.md)]

## <a name="introduction"></a>Einführung
In diesem Artikel wird beschrieben, wie Sie den IoT Hub-Dienst im Azure-Portal finden und IoT Hubs erstellen und verwalten.

## <a name="where-to-find-iot-hubs"></a>Suchen nach IoT Hubs
Es gibt verschiedene Orte, wo Sie IoT Hubs finden können.

1. **+ Neu**: **Azure IoT Hub** ist ein IoT-Dienst, den Sie wie andere Dienste unter der Kategorie **Internet der Dinge** unter **+ Neu** finden.
2. Auf IoT Hubs können Sie auch über den Marketplace zugreifen – wie der Hero-Dienst unter **Internet der Dinge**.

## <a name="create-an-iot-hub"></a>Erstellen eines IoT Hubs
Sie können einen IoT Hub mit den folgenden Methoden erstellen:

* Wenn Sie einen IoT Hub über die Option **+ Neu** erstellen, wird das folgende Blatt aus dem nächsten Screenshot angezeigt. Beim Erstellen des IoT Hubs mit dieser Methode gehen Sie genauso vor wie beim Erstellen über den Marketplace.
* Erstellen eines IoT Hubs über den Marketplace: Wenn Sie auf **Erstellen** klicken, wird ein Blatt geöffnet, das mit dem vorherigen Blatt der Option **+ Neu** identisch ist. In den nächsten Abschnitten werden die verschiedenen Schritte zum Erstellen eines IoT Hubs aufgeführt.

### <a name="choose-the-name-of-the-iot-hub"></a>Auswählen des Namen des IoT Hubs
Um einen IoT Hub zu erstellen, müssen Sie einen Namen für den IoT Hub festlegen. Dieser Name muss unter den IoT Hubs eindeutig sein. Da im Lösungs-Back-End keine Duplizierung von Hubs zulässig ist, wird eine möglichst eindeutige Benennung dieses Hubs empfohlen.

### <a name="choose-the-pricing-tier"></a>Auswählen des Tarifs
Sie können zwischen vier Tarifen wählen: **Free**, **Standard 1**, **Standard 2** und **Standard S3**. Mit dem Free-Tarif können nur 500 Geräte mit dem IoT Hub verbunden sein, und bis zu 8.000 Nachrichten pro Tag sind möglich.

**Standard S1**: Die IoT Hub-Edition S1 ist für IoT-Lösungen mit einer großen Anzahl von Geräten ausgelegt, die relativ kleine Datenmengen pro Gerät generieren. Mit jeder S1-Einheit können bis zu 400.000 Nachrichten pro Tag (auf allen verbundenen Geräten) übermittelt werden.

**Standard S2**: Die IoT Hub S2-Edition ist auf IoT-Lösungen ausgelegt, in denen Geräte große Datenmengen generieren. Mit jeder Einheit der Edition S2 können bis zu 6 Millionen Nachrichten pro Tag zwischen allen verbundenen Geräten übermittelt werden.

**Standard S3**: Die IoT Hub-Edition S3 eignet sich für IoT-Lösungen, die große Datenmengen generieren. Mit jeder Einheit der Edition S3 können bis zu 300 Millionen Nachrichten pro Tag zwischen allen verbundenen Geräten übermittelt werden.

![][4]

> [!NOTE]
> IoT Hub bietet pro Azure-Abonnement nur einen Hub im Tarif „Free“.
> 
> 

### <a name="iot-hub-units"></a>IoT Hub-Einheiten
Die Anzahl der pro Einheit und Tag zulässigen Nachrichten hängt von Ihrem Hubtarif ab. Beispiel: Wenn der IoT Hub 700.000 eingehende Nachrichten unterstützen soll, wählen Sie zwei Einheiten des Tarifs S1.

### <a name="device-to-cloud-partitions-and-resource-group"></a>Gerät für Cloud-Partitionen und Ressourcengruppe
Sie können die Anzahl der Partitionen für einen IoT Hub ändern. Standardmäßig sind 4 Partitionen festgelegt, Sie können jedoch eine andere Anzahl von Partitionen aus einer Dropdownliste auswählen.

Für Ressourcengruppen muss nicht zwingend eine leere Ressourcengruppe erstellt werden. Beim Erstellen einer Ressource können Sie entweder eine neue Ressourcengruppe erstellen oder eine vorhandene Ressourcengruppe verwenden.

![][5]

### <a name="choose-subscriptions"></a>Auswählen von Abonnements
Azure IoT Hub zeigt automatisch die Liste der Azure-Abonnements an, mit denen das Benutzerkonto verknüpft ist. Sie können eine dieser Optionen auswählen, um den IoT Hub diesem Azure-Abonnement zuzuordnen.

### <a name="choose-the-location"></a>Wählen des Standorts
Die Option „Standort“ enthält eine Liste der Regionen, in denen IoT Hub angeboten wird. IoT Hub ist in den folgenden Regionen zur Bereitstellung verfügbar: Australien, Osten; Australien, Südosten; Asien-Pazifik, Osten; Asien-Pazifik, Südosten; Europa, Norden; Europa, Westen; Japan, Osten; Japan, Westen; USA, Osten; USA, Westen.

### <a name="create-the-iot-hub"></a>Erstellen des IoT Hubs
Wenn alle vorherigen Schritte abgeschlossen sind, kann der IoT Hub erstellt werden. Klicken Sie auf **Erstellen** , um den Back-End-Prozess zur Erstellung dieses IoT Hubs mit den festgelegten Optionen zu starten und die Bereitstellung am angegebenen Ort durchzuführen.

Die Erstellung des IoT Hubs kann einige Minuten in Anspruch nehmen, da es etwas dauert, bis die Back-End-Bereitstellung auf den jeweiligen Servern abgeschlossen ist.

## <a name="change-the-settings-of-the-iot-hub"></a>Ändern der Einstellungen des IoT Hubs
Sie können die Einstellungen für einen IoT Hub nach dessen Erstellung im Blatt „IoT Hub“ ändern.

![][8]

**Richtlinien für gemeinsamen Zugriff**: Diese Richtlinien definieren die Berechtigungen für Geräte und Dienste zum Herstellen einer Verbindung mit dem IoT Hub. Sie finden diese Richtlinien, indem Sie unter **Allgemein** auf **Richtlinien für gemeinsamen** Zugriff klicken. Auf diesem Blatt können Sie vorhandene Richtlinien ändern oder eine neue Richtlinie hinzufügen.

### <a name="create-a-policy"></a>Erstellen einer Richtlinie
* Klicken Sie auf **Hinzufügen**, um ein Blatt zu öffnen. Hier können Sie den Namen der neuen Richtlinie und die Berechtigungen, die dieser Richtlinie zugeordnet werden sollen, wie in der folgenden Abbildung gezeigt eingeben:
  
    Diesen freigegebenen Richtlinien können mehrere Berechtigungen zugeordnet werden. Die ersten beiden Richtlinien, **Registrierung lesen** und **In Registrierung schreiben**, berechtigen zum Lesen und Schreiben im Geräteidentitätsspeicher oder der Identitätsregistrierung. Beim Auswählen der Option mit Schreibzugriff wird auch die Option mit Lesezugriff automatisch ausgewählt.
  
     Die Richtlinie **Dienstverbindung** gewährt die Berechtigung zum Zugreifen auf die cloudseitigen Endpunkte, z.B. auf die Consumergruppe für Dienste, die eine Verbindung mit dem IoT Hub herstellen. Die Richtlinie **Geräteverbindung** gewährt Berechtigungen zum Senden und Empfangen von Nachrichten auf den geräteseitigen Endpunkten des IoT Hubs.
* Klicken Sie auf **Erstellen** , um der vorhandenen Liste diese neu erstellte Richtlinie hinzuzufügen.

![][10]

## <a name="endpoints"></a>Endpunkte
Klicken Sie auf **Endpunkte**, um eine Liste der Endpunkte für den IoT-Hub anzuzeigen, den Sie ändern. Es gibt zwei Typen von Endpunkten: Endpunkte, die in den IoT-Hub integriert sind, und Endpunkte, die Sie dem IoT-Hub nach seiner Erstellung hinzufügen.

![][11]

### <a name="built-in-endpoints"></a>Integrierte Endpunkte
Es gibt zwei integrierte Endpunkte: **Cloud-zu-Gerät-Feedback** und **Ereignisse**.

* Einstellungen für **Cloud-zu-Gerät-Feedback**: Diese Einstellung hat zwei untergeordnete Einstellungen: **Cloud-zu-Gerät-TTL** (Time to Live, Gültigkeitsdauer) und **Aufbewahrungszeit** (in Stunden) für die Nachrichten. Bei der ersten Erstellung eines IoT-Hubs ist für beide Einstellungen ein Standardwert von einer Stunde festgelegt. Zum Anpassen dieser Einstellungen bewegen Sie die Schieberegler oder geben die entsprechenden Werte ein.
* Einstellungen für **Ereignisse**: Für diese Einstellung sind verschiedene Untereinstellungen verfügbar. Manche davon sind schreibgeschützt. Diese Einstellungen werden in der folgende Liste beschrieben:

    * **Partitionen**: Beim Erstellen des IoT-Hubs wird ein Standardwert festgelegt. Sie können die Anzahl von Partitionen über diese Einstellung ändern.

    * **Event Hub-kompatibler Name und Event-Hub-kompatibler Endpunkt**: Bei Erstellung des IoT Hubs wird intern ein Event Hub erstellt, auf den Sie unter bestimmten Umständen zugreifen müssen. Die Event Hub-kompatiblen Werte für Name und Endpunkt können nicht angepasst, aber kopiert werden. Klicken Sie dazu auf **Kopieren**.

    * **Aufbewahrungszeit**: Standardmäßig auf 1 Tag festgelegt, kann aber mithilfe der Dropdownliste geändert werden. Dieser Wert wird für die Gerät-zu-Cloud-Einstellung in Tagen angegeben.

    * **Consumergruppen**: Consumergruppen ähneln als Einstellung anderen Messaging-Systemen, die zum Abrufen von Daten auf bestimmte Weise verwendet werden können, um andere Programme oder Dienste mit IoT Hub zu verbinden. Jeder IoT Hub wird mit einer Standard-Consumergruppe erstellt. Sie können mit dieser Einstellung Ihren IoT-Hubs jedoch Consumergruppen hinzufügen oder diese daraus löschen.

    > [!NOTE]
    > Die Standard-Consumergruppe kann nicht bearbeitet oder gelöscht werden.
    > 
    > 

### <a name="custom-endpoints"></a>Benutzerdefinierte Endpunkte
Sie können Ihrem IoT-Hub über das Portal benutzerdefinierte Endpunkte hinzufügen. Klicken Sie auf dem Blatt **Endpunkte** oben auf **Hinzufügen**, um das Blatt **Endpunkt hinzufügen** zu öffnen. Geben Sie die erforderlichen Informationen ein, und klicken Sie dann auf **OK**. Ihr benutzerdefinierter Endpunkt wird nun auf dem Hauptblatt für **Endpunkte** aufgeführt.

![][13]

In [Referenz: IoT Hub-Endpunkte][lnk-devguide-endpoints] erfahren Sie mehr über benutzerdefinierte Endpunkte.

## <a name="routes"></a>Routen
Klicken Sie auf **Routen**, um zu verwalten, wie IoT Hub Ihre D2C-Nachrichten sendet.

![][14]

Sie können Ihrem IoT-Hub Routen hinzufügen, indem Sie am oberen Rand des Blatts **Routen*** auf **Hinzufügen** klicken, die erforderlichen Informationen eingeben und auf **OK** klicken. Ihre Route wird dann auf dem Hauptblatt **Routen** aufgeführt. Sie können eine Route bearbeiten, indem Sie in der Liste der Routen darauf klicken. Um eine Route zu aktivieren, klicken sie in der Liste der Routen darauf, und legen Sie den Schalter **Aktiviert** auf **Aus** fest. Klicken Sie am unteren Rand des Blatts auf **OK**, um die Änderung zu speichern.

![][15]

## <a name="pricing-and-scale"></a>Preise und Skalierung
Die Preise eines vorhandenen IoT Hubs können mit folgenden Ausnahmen über die Einstellungen **Preise** geändert werden:

* In der aktuellen Implementierung kann ein IOT Hub mit Free-SKU nicht in eine der kostenpflichtigen SKUs geändert werden oder umgekehrt.
* Im Azure-Abonnement kann nur ein IoT Hub des Free-Tarifs enthalten sein.

![][12]

Der Wechsel von einem höheren Tarif (S2 oder S3) in einen niedrigeren Tarif (S1 oder S2) ist nur zulässig, wenn die Anzahl an gesendeten Nachrichten des Tages nicht dazu in Konflikt steht. Überschreitet die Anzahl der Nachrichten pro Tag beispielsweise 400.000, kann der Tarif für den IoT Hub nicht geändert werden. Wenn Sie jedoch zum Tarif S1 wechseln, wird der IoT Hub für diesen Tag gedrosselt.

## <a name="delete-the-iot-hub"></a>Löschen des IoT Hubs
Klicken Sie zum Suchen des zu löschenden IoT Hubs auf **Durchsuchen**, und wählen Sie dann den zu löschenden Hub aus. Durch Klicken auf die Schaltfläche **Löschen** unterhalb des Namens des IoT Hubs wird der IoT Hub gelöscht.

## <a name="next-steps"></a>Nächste Schritte
Folgen Sie diesen Links, um mehr über das Verwalten von Azure IoT Hub zu erfahren:

* [Massenverwaltung von IoT-Geräten][lnk-bulk]
* [IoT Hub-Metriken][lnk-metrics]
* [Vorgangsüberwachung][lnk-monitor]

Weitere Informationen zu den Funktionen von IoT Hub finden Sie unter:

* [IoT Hub-Entwicklerhandbuch][lnk-devguide]
* [Simulieren eines Geräts mit IoT Edge][lnk-iotedge]
* [Schützen Ihrer IoT-Lösung von Grund auf][lnk-securing]

[4]: ./media/iot-hub-create-through-portal/create-iothub.png
[5]: ./media/iot-hub-create-through-portal/location1.png
[8]: ./media/iot-hub-create-through-portal/portal-settings.png
[10]: ./media/iot-hub-create-through-portal/shared-access-policies.png
[11]: ./media/iot-hub-create-through-portal/messaging-settings.png
[12]: ./media/iot-hub-create-through-portal/pricing-error.png
[13]: ./media/iot-hub-create-through-portal/endpoint-creation.png
[14]: ./media/iot-hub-create-through-portal/routes-list.png
[15]: ./media/iot-hub-create-through-portal/route-edit.png

[lnk-bulk]: iot-hub-bulk-identity-mgmt.md
[lnk-metrics]: iot-hub-metrics.md
[lnk-monitor]: iot-hub-operations-monitoring.md

[lnk-devguide]: iot-hub-devguide.md
[lnk-iotedge]: iot-hub-linux-iot-edge-simulated-device.md
[lnk-securing]: iot-hub-security-ground-up.md
[lnk-devguide-endpoints]: iot-hub-devguide-endpoints.md


---
title: Verwalten von Endpunkten in Azure Traffic Manager | Microsoft Docs
description: "Dieser Artikel enthält Informationen zum Hinzufügen, Entfernen, Aktivieren und Deaktivieren von Endpunkten in Azure Traffic Manager."
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: ade2bbc2-35a7-43c5-8001-4698f7254526
ms.service: traffic-manager
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/11/2016
ms.author: kumud
translationtype: Human Translation
ms.sourcegitcommit: 8827793d771a2982a3dccb5d5d1674af0cd472ce
ms.openlocfilehash: 52f6d4f3e68e5eb120ee499827cc8549b8e547fd

---

# <a name="add-disable-enable-or-delete-endpoints"></a>Hinzufügen, Deaktivieren, Aktivieren oder Löschen von Endpunkten

Das Web-Apps-Feature in Azure App Service stellt bereits unabhängig vom Websitemodus Failover- und Roundrobin-Funktionen für das Routing von Datenverkehr für Websites in einem Datencenter zur Verfügung. Azure Traffic Manager ermöglicht das Angeben des Failover- und Roundrobin-Routings von Datenverkehr für Websites und Clouddienste in verschiedenen Datencentern. Der erste erforderliche Schritt zum Bereitstellen dieser Funktionalität ist das Hinzufügen die Funktionen des Cloud-Dienst- oder Website-Endpunkts zu Traffic Manager.

> [!NOTE]
> In diesem Artikel wird beschrieben, wie Sie das klassische Portal verwenden. Das klassische Azure-Portal unterstützt nur die Erstellung und Zuweisung von Clouddiensten und Web-Apps als Endpunkte. Das neue [Azure-Portal](https://portal.azure.com) ist die bevorzugte Oberfläche.

Sie können auch einzelne Endpunkte deaktivieren, die Teil eines Traffic Manager-Profils sind. Bei Deaktivieren eines Endpunkts bleibt er Teil des Profils, doch das Profil verhält sich so, als wäre der Endpunkt nicht vorhanden. Diese Aktion ist nützlich zum zeitweiligen Entfernen eines Endpunkts, der sich im Wartungsmodus befindet oder erneut bereitgestellt werden soll. Sobald der Endpunkt wieder betriebsbereit ist, kann er aktiviert werden.

> [!NOTE]
> Das Deaktivieren eines gehosteten Diensts hat nichts mit dessen Bereitstellungsstatus in Azure zu tun. Ein fehlerfrei funktionierender Endpunkt bleibt in Betrieb und kann Datenverkehr empfangen, selbst wenn er in Traffic Manager deaktiviert ist. Das Deaktivieren eines Endpunkts in einem Profil wirkt sich darüber hinaus nicht auf seinen Status in einem anderen Profil aus.

## <a name="to-add-a-cloud-service-or-website-endpoint"></a>So fügen Sie einen Cloud-Dienst oder Website-Endpunkt hinzu

1. Suchen Sie im klassischen Azure-Portal im Bereich „Traffic Manager“ nach dem Traffic Manager-Profil, das die zu ändernden Endpunkteinstellungen enthält. Klicken Sie zum Öffnen der Seite mit den Einstellungen auf den Pfeil rechts vom Profilnamen.
2. Klicken Sie oben auf der Seite auf **Endpunkte** , um die Endpunkte anzuzeigen, die schon in der Konfiguration enthalten sind.
3. Klicken Sie unten auf der Seite auf **Hinzufügen**, um auf die Seite **Dienstendpunkte hinzufügen** zuzugreifen. In der Standardeinstellung werden auf dieser Seite unter **Dienstendpunkte**die Cloud-Dienste aufgelistet.
4. Wählen Sie als Clouddienste die Clouddienste in der Liste aus, die als Endpunkte für dieses Profil hinzugefügt werden sollen. Durch Löschen des Namen des Cloud-Diensts wird dieser aus der Liste der Endpunkte entfernt.
5. Klicken Sie für Websites auf die Dropdownliste **Diensttyp**, und wählen Sie dann **Web-App** aus.
6. Wählen Sie die Websites in der Liste aus, um sie als Endpunkte für dieses Profil hinzuzufügen. Durch Löschen des Namen der Website wird diese aus der Liste der Endpunkte entfernt. Sie können eine Website pro Azure-Datencenter (auch als Region bezeichnet) auswählen. Wenn Sie die erste Website auswählen, können die anderen Websites in demselben Datencenter nicht mehr ausgewählt werden. Beachten Sie außerdem, dass nur Standardwebsites aufgelistet werden.
7. Nachdem Sie die Endpunkte für dieses Profil ausgewählt haben, klicken Sie auf das Häkchen in der unteren rechten Ecke, um die Änderungen zu speichern.

> [!NOTE]
> Nachdem Sie einen Endpunkt mit der Datenverkehr-Routingmethode *Failover* hinzugefügt oder entfernt haben, ist die Failoverprioritätsliste ggf. nicht mehr wie gewünscht sortiert. Sie können die Reihenfolge der Failoverprioritätsliste auf der Seite „Konfiguration“ anpassen. Weitere Informationen finden Sie unter [Konfigurieren des Routings für Failoverdatenverkehr](traffic-manager-configure-failover-routing-method.md).

## <a name="to-disable-an-endpoint"></a>So deaktivieren Sie einen Endpunkt

1. Suchen Sie im klassischen Azure-Portal im Bereich „Traffic Manager“ nach dem Traffic Manager-Profil, das die zu ändernden Endpunkteinstellungen enthält. Klicken Sie zum Öffnen der Seite mit den Einstellungen auf den Pfeil rechts vom Profilnamen.
2. Klicken Sie oben auf der Seite auf **Endpunkte** , um die Endpunkte anzuzeigen, die in der Konfiguration enthalten sind.
3. Klicken Sie auf den Endpunkt, den Sie deaktivieren möchten, und dann am unteren Rand der Seite auf **Deaktivieren** .
4. Clients senden während der Gültigkeitsdauer (Time-to-Live, TTL) weiterhin Datenverkehr an den Endpunkt. Sie können die Gültigkeitsdauer auf der Seite „Konfiguration“ des Traffic Manager-Profils ändern.

## <a name="to-enable-an-endpoint"></a>So aktivieren Sie einen Endpunkt

1. Suchen Sie im klassischen Azure-Portal im Bereich „Traffic Manager“ nach dem Traffic Manager-Profil, das die zu ändernden Endpunkteinstellungen enthält. Klicken Sie zum Öffnen der Seite mit den Einstellungen auf den Pfeil rechts vom Profilnamen.
2. Klicken Sie oben auf der Seite auf **Endpunkte** , um die Endpunkte anzuzeigen, die in der Konfiguration enthalten sind.
3. Klicken Sie auf den Endpunkt, den Sie aktivieren möchten, und dann am unteren Rand der Seite auf **Aktivieren** .
4. Clients werden je nach Vorgabe durch das Profil an den aktivierten Endpunkt weitergeleitet.

## <a name="to-delete-a-cloud-service-or-website-endpoint"></a>So löschen Sie einen Cloud-Dienst oder Website-Endpunkt

1. Suchen Sie im klassischen Azure-Portal im Bereich „Traffic Manager“ nach dem Traffic Manager-Profil, das die zu ändernden Endpunkteinstellungen enthält. Klicken Sie zum Öffnen der Seite mit den Einstellungen auf den Pfeil rechts vom Profilnamen.
2. Klicken Sie oben auf der Seite auf **Endpunkte** , um die Endpunkte anzuzeigen, die schon in der Konfiguration enthalten sind.
3. Klicken Sie auf der Seite "Endpunkte" auf den Namen des Endpunkts, den Sie aus dem Profil löschen möchten.
4. Klicken Sie unten auf der Seite auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

* [Verwalten von Traffic Manager-Profilen](traffic-manager-manage-profiles.md)
* [Konfigurieren von Routingmethoden](traffic-manager-configure-routing-method.md)
* [Problembehandlung beim Status „Heruntergestuft“ in Azure Traffic Manager](traffic-manager-troubleshooting-degraded.md)
* [Leistungsüberlegungen zu Traffic Manager](traffic-manager-performance-considerations.md)
* [Vorgänge für Traffic Manager (REST-API-Referenz)](http://go.microsoft.com/fwlink/p/?LinkID=313584)




<!--HONumber=Feb17_HO3-->



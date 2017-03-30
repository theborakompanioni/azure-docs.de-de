---
title: Erstellen eines Traffic Manager-Profils in Azure| Microsoft-Dokumentation
description: Dieser Artikel beschreibt das Erstellen eines Traffic Manager-Profils
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: 
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/21/2017
ms.author: kumud
translationtype: Human Translation
ms.sourcegitcommit: 6d749e5182fbab04adc32521303095dab199d129
ms.openlocfilehash: 58194b71b22b63f7d4a2a6bf0f4e66f456a96d03
ms.lasthandoff: 03/22/2017

---

# <a name="create-a-traffic-manager-profile"></a>Erstellen eines Traffic Manager-Profils

Dieser Artikel beschreibt, wie ein Profil mit dem Routingtyp **Priorität** erstellt werden kann, um Benutzer an zwei Azure-Web-Apps-Endpunkte zu routen. Mithilfe des Routingtyps **Priorität** wird der gesamte Datenverkehr an den ersten Endpunkt geroutet, während der zweite als Backup dient. Das hat zur Folge, dass Benutzer an den zweiten Endpunkt geroutet werden können, wenn die Integrität des ersten Endpunkts nicht intakt ist.

In diesem Artikel werden diesem neu erstellten Traffic Manager-Profil zwei zuvor erstellte Azure-Web-App-Endpunkte zugeordnet. Weitere Informationen zum Erstellen von Azure-Web-App-Endpunkten finden Sie auf der [Azure-Web-Apps-Dokumentationsseite](https://docs.microsoft.com/azure/app-service-web/). Sie können jeden Endpunkt hinzufügen, der über einen DNS-Namen verfügt und über das öffentliche Internet erreichbar ist; wir verwenden beispielhaft Azure-Web-Apps-Endpunkte.

### <a name="create-a-traffic-manager-profile"></a>Erstellen eines Traffic Manager-Profils
1. Melden Sie sich über einen Browser beim [Azure-Portal](http://portal.azure.com) an. Wenn Sie noch nicht über ein Konto verfügen, können Sie sich für eine [kostenlose einmonatige Testversion](https://azure.microsoft.com/free/) registrieren. 
2. Klicken Sie im **Hubmenü** auf **Neu** > **Netzwerk** > **Alle anzeigen**, klicken Sie dann auf **Traffic Manager**-Profil, um das Blatt **Traffic Manager-Profil erstellen** zu öffnen, und klicken Sie dann auf **Erstellen**.
3. Füllen Sie das Blatt **Traffic Manager-Profil erstellen** wie folgt aus:
    1. Geben Sie im Feld **Name** einen Namen für Ihr Profil ein. Dieser Name muss innerhalb der Zone „trafficmanager.net“ eindeutig sein und ergibt den DNS-Namen <name>.trafficmanager.net, der für den Zugriff auf Ihr Traffic Manager-Profil verwendet wird.
    2. Wählen Sie unter **Routingmethode** die Routingmethode **Priorität** aus.
    3. Wählen Sie unter **Abonnement** das Abonnement aus, unter dem Sie dieses Profil erstellen möchten
    4. Erstellen Sie unter **Ressourcengruppe** eine neue Ressourcengruppe, unter der Sie das Profil platzieren möchten.
    5. Wählen Sie unter **Ressourcengruppenstandort** den Speicherort für die Ressourcengruppe aus. Diese Einstellung bezieht sich auf den Speicherort der Ressourcengruppe und hat keine Auswirkungen auf das Traffic Manager-Profil, das global bereitgestellt wird.
    6. Klicken Sie auf **Erstellen**.
    7. Wenn die globale Bereitstellung Ihres Traffic Manager-Profils abgeschlossen ist, wird sie in der betreffenden Ressourcengruppe als eine der Ressourcen aufgelistet.

![Erstellen eines Traffic Manager-Profils](./media/traffic-manager-create-profile/Create-traffic-manager-profile.png)

## <a name="add-traffic-manager-endpoints"></a>Hinzufügen von Traffic Manager-Endpunkten

1. Suchen Sie in der Suchleiste des Portals nach dem Namen des **Traffic Manager-Profils**, das Sie im vorhergehenden Abschnitt erstellt haben, und klicken Sie in den angezeigten Ergebnissen auf das Traffic Manager-Profil.
2. Klicken Sie auf dem Blatt **Traffic Manager-Profil** im Abschnitt **Einstellungen** auf **Endpunkte**.
3. Klicken Sie auf dem dann angezeigten Blatt **Endpunkte** auf **Hinzufügen**.
4. Füllen Sie das Blatt **Endpunkt hinzufügen** wie folgt aus:
    1. Klicken Sie als **Typ** auf **Azure-Endpunkt**.
    2. Geben Sie einen **Namen** an, den Sie zur Bezeichnung des Endpunkts verwenden möchten.
    3. Klicken Sie als **Zielressourcentyp** auf **App Service**.
    4. Klicken Sie für **Zielressource** auf **App Service auswählen**, um die Auflistung der Web Apps im gleichen Abonnement anzuzeigen. Wählen Sie auf dem angezeigten Blatt **Ressource** den App Service aus, den Sie als ersten Endpunkt hinzufügen möchten.
    5. Legen Sie als **Priorität** den Wert **1** fest. Dies bewirkt, dass der gesamte Datenverkehr an diesen Endpunkt geleitet wird, sofern sein Status intakt ist.
    6. Lassen Sie **Als deaktiviert hinzufügen** deaktiviert.
    7. Klicken Sie auf **OK**
5.    Wiederholen Sie die Schritte 3 und 4 für den nächsten Azure-Web-Apps-Endpunkt. Achten Sie darauf, ihn mit einem auf **2** festgelegten Wert für **Priorität** hinzuzufügen.
6.    Wenn Sie das Hinzufügen beider Endpunkte abgeschlossen haben, werden diese auf dem Blatt **Traffic Manager-Profil** zusammen mit ihrem Überwachungsstatus als **Online** angezeigt.

![Hinzufügen eines Traffic Manager-Endpunkts](./media/traffic-manager-create-profile/add-traffic-manager-endpoint.png)

## <a name="use-the-traffic-manager-profile"></a>Verwenden des Traffic Manager-Profils
1.    Suchen Sie in der Suchleiste des Portals nach dem Namen des **Traffic Manager-Profils**, das Sie im vorhergehenden Abschnitt erstellt haben. Klicken Sie in den angezeigten Ergebnissen auf das Traffic Manager-Profil.
2. Klicken Sie auf dem Blatt **Traffic Manager-Profil** auf **Übersicht**.
3. Auf dem Blatt **Traffic Manager-Profil** wird der DNS-Name Ihres neu erstellten Traffic Manager-Profils angezeigt. Dieser kann von beliebigen Clients (etwa durch Navigation in einem Webbrowser) für das Routing zum richtigen Endpunkt nach Maßgabe des Routingtyps verwendet werden. In diesem Fall werden alle Anforderungen an den ersten Endpunkt weitergeleitet. Wenn dieser in Traffic Manager als fehlerhaft erkannt wird, wird für den Datenverkehr automatisch ein Failover auf den nächsten Endpunkt durchgeführt.

## <a name="delete-the-traffic-manager-profile"></a>Löschen des Traffic Manager-Profils
Wenn es nicht mehr benötigt wird, löschen Sie die Ressourcengruppe und das erstellte Traffic Manager-Profil. Wählen Sie zu diesem Zweck die Ressourcengruppe auf dem Blatt **Traffic Manager-Profil** aus, und klicken Sie auf **Löschen**.

## <a name="next-steps"></a>Nächste Schritte

- Weitere Informationen über [Routingtypen](traffic-manager-routing-methods.md).
- Weitere Informationen über Endpunkttypen [Endpunkttypen](traffic-manager-endpoint-types.md).
- Weitere Informationen über [Endpunktüberwachung](traffic-manager-monitoring.md).





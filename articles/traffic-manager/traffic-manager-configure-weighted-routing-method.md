---
title: "Konfigurieren der gewichteten Roundrobin-Routingmethode für Datenverkehr mit Azure Traffic Manager | Microsoft Docs"
description: "In diesem Artikel wird der Lastenausgleich des Datenverkehrs mit einer Roundrobinmethode in Traffic Manager erläutert."
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: 6dca6de1-18f7-4962-bd98-6055771fab22
ms.service: traffic-manager
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/20/2017
ms.author: kumud
translationtype: Human Translation
ms.sourcegitcommit: 1429bf0d06843da4743bd299e65ed2e818be199d
ms.openlocfilehash: dc2111a34fa0292a21510bbd84089979faf1cb71
ms.lasthandoff: 03/22/2017

---

# <a name="configure-the-weighted-traffic-routing-method-in-traffic-manager"></a>Konfigurieren der gewichteten Routingmethode für Datenverkehr in Traffic Manager

Ein gängiges Muster für das Routing von Datenverkehr besteht darin, eine Reihe identischer Endpunkte (die Clouddienste und Websites umfassen) bereitzustellen und Datenverkehr im Roundrobin-Verfahren an die einzelnen Endpunkte zu senden. Die folgenden Schritte beschreiben, wie diese Routingmethodentyp für Datenverkehr konfiguriert wird.

> [!NOTE]
> Beachten Sie, dass Azure Websites bereits Roundrobin-Lastenausgleichsfunktionen für Websites in einem Rechenzentrum (auch als „Region“ bezeichnet) zur Verfügung stellt. Traffic Manager ermöglicht das Angeben von Routingmethoden für Roundrobin-Datenverkehr für Websites in verschiedenen Datencentern.

## <a name="to-configure-the-weighted-traffic-routing-method"></a>So konfigurieren Sie die gewichtete Routingmethode für Datenverkehr

1. Melden Sie sich in einem Browser beim [Azure-Portal](http://portal.azure.com) an. Falls Sie noch nicht über ein Azure-Konto verfügen, können Sie sich für eine [kostenlose einmonatige Testversion](https://azure.microsoft.com/free/) registrieren. 
2. Suchen Sie auf der Suchleiste des Portals die **Traffic Manager-Profile**, und klicken Sie dann auf den Namen des Profils, für das Sie die Routingmethode konfigurieren möchten.
3. Stellen Sie auf dem Blatt **Traffic Manager-Profil** sicher, dass die Clouddienste sowie die Websites, die Ihre Konfiguration beinhalten soll, vorhanden sind.
4. Klicken Sie im Abschnitt **Einstellungen** auf **Konfiguration**, und nehmen Sie auf dem Blatt **Konfiguration** die folgenden Einstellungen vor:
    1. Überprüfen Sie in den Einstellungen der **Routingmethode für Datenverkehr**, ob die Routingmethode für Datenverkehr **Failover** ist. Klicken Sie andernfalls in der Dropdownliste auf **Failover** .
    2. Legen Sie die **Überwachungseinstellungen für Endpunkt** für alle Endpunkte in diesem Profil wie folgt identisch fest:
        1. Wählen Sie das entsprechende **Protokoll** aus, und geben Sie die Nummer für den **Port** an. 
        2. Geben Sie unter **Pfad** einen Schrägstrich */* ein. Sie müssen einen Pfad und einen Dateinamen angeben, um Endpunkte zu überwachen. Ein Schrägstrich „/“ ist ein gültiger Eintrag für den relativen Pfad und bedeutet, dass sich die Datei im Stammverzeichnis (Standardwert) befindet.
        3. Klicken Sie oben auf der Seite auf **Speichern**.
5. Testen Sie wie folgt die Änderungen in Ihrer Konfiguration:
    1.    Suchen Sie auf der Suchleiste des Portals nach dem Namen des Traffic Manager-Profils, und klicken Sie in den angezeigten Ergebnissen auf das Traffic Manager-Profil.
    2.    Klicken Sie auf dem Blatt **Traffic Manager-Profil** auf **Übersicht**.
    3.    Auf dem Blatt **Traffic Manager-Profil** wird der DNS-Name des neu erstellten Traffic Manager-Profils angezeigt. Dieser kann von beliebigen Clients (etwa durch Navigation in einem Webbrowser) für das Routing zum richtigen Endpunkt nach Maßgabe des Routingtyps verwendet werden. In diesem Fall werden alle Anforderungen in einem Roundrobinverfahren an die einzelnen Endpunkte weitergeleitet.
6. Sobald das Traffic Manager-Profil funktionsfähig ist, bearbeiten Sie den DNS-Eintrag auf dem autoritativen DNS-Server, damit Ihre Unternehmensdomäne auf den Namen der Traffic Manager-Domäne verweisen kann.

![Konfigurieren der gewichteten Routingmethode für Datenverkehr mit Traffic Manager][1]

## <a name="next-steps"></a>Nächste Schritte

- Informationen zur [prioritätsbasierten Routingmethode für Datenverkehr](traffic-manager-configure-priority-routing-method.md).
- Informationen zur [leistungsorientierten Routingmethode für Datenverkehr](traffic-manager-configure-performance-routing-method.md).
- Informationen zur [geografischen Routingmethode](traffic-manager-configure-geographic-routing-method.md).
- Informationen zum [Testen von Traffic Manager-Einstellungen](traffic-manager-testing-settings.md).

<!--Image references-->
[1]: ./media/traffic-manager-weighted-routing-method/traffic-manager-weighted-routing-method.png


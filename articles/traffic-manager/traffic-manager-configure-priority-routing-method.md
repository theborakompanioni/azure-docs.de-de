---
title: "Konfigurieren der prioritätsbasierten Routingmethode für Datenverkehr mit Azure Traffic Manager | Microsoft Docs"
description: "Dieser Artikel erläutert die Konfiguration der prioritätsbasierten Routingmethode für Datenverkehr in Traffic Manager."
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
ms.openlocfilehash: 0db83cde6facc89b8b8aa72e6419129ec868235c
ms.lasthandoff: 03/22/2017

---

# <a name="configure-priority-traffic-routing-method-in-traffic-manager"></a>Konfigurieren der prioritätsbasierten Routingmethode für Datenverkehr in Traffic Manager

Unabhängig vom Websitemodus stellen Azure Websites bereits Failoverfunktionen für Websites in einem Rechenzentrum (auch als „Region“ bezeichnet) zur Verfügung. Traffic Manager bietet ein Failover für Websites in verschiedenen Rechenzentren.

Ein häufiges Muster für Dienst-Failover ist das Senden von Datenverkehr an den primären Dienst und das Bereitstellen von identischen Sicherungsdiensten für das Failover. Die folgenden Schritte erläutern die Konfiguration dieses priorisierten Failovers mit Azure Cloud Services und Websites:

## <a name="to-configure-the-priority-traffic-routing-method"></a>So konfigurieren Sie die prioritätsbasierte Routingmethode für Datenverkehr

1. Melden Sie sich in einem Browser beim [Azure-Portal](http://portal.azure.com) an. Falls Sie noch nicht über ein Azure-Konto verfügen, können Sie sich für eine [kostenlose einmonatige Testversion](https://azure.microsoft.com/free/) registrieren. 
2. Suchen Sie auf der Suchleiste des Portals die **Traffic Manager-Profile**, und klicken Sie dann auf den Namen des Profils, für das Sie die Routingmethode konfigurieren möchten.
3. Stellen Sie auf dem Blatt **Traffic Manager-Profil** sicher, dass die Clouddienste sowie die Websites, die Ihre Konfiguration beinhalten soll, vorhanden sind.
4. Klicken Sie im Abschnitt **Einstellungen** auf **Konfiguration**, und nehmen Sie auf dem Blatt **Konfiguration** die folgenden Einstellungen vor:
    1. Überprüfen Sie in den Einstellungen der **Routingmethode für Datenverkehr**, ob die Routingmethode für Datenverkehr **Priorität** ist. Klicken Sie andernfalls in der Dropdownliste auf **Priorität**.
    2. Legen Sie die **Überwachungseinstellungen für Endpunkt** für alle Endpunkte in diesem Profil wie folgt identisch fest:
        1. Wählen Sie das entsprechende **Protokoll** aus, und geben Sie die Nummer für den **Port** an. 
        2. Geben Sie unter **Pfad** einen Schrägstrich */* ein. Sie müssen einen Pfad und einen Dateinamen angeben, um Endpunkte zu überwachen. Ein Schrägstrich „/“ ist ein gültiger Eintrag für den relativen Pfad und bedeutet, dass sich die Datei im Stammverzeichnis (Standardwert) befindet.
        3. Klicken Sie oben auf der Seite auf **Speichern**.
5. Klicken Sie im Abschnitt **Einstellungen** auf **Endpunkte**.
6. Überprüfen Sie auf dem Blatt **Endpunkte** die Prioritätsreihenfolge für die Endpunkte. Wenn Sie die Routingmethode für Datenverkehr **Priorität** auswählen, ist die Reihenfolge der ausgewählten Endpunkte von Bedeutung. Überprüfen Sie die Prioritätsreihenfolge der Endpunkte.  Der primäre Endpunkt steht an erster Stelle. Überprüfen Sie die angezeigte Reihenfolge dieses Endpunkts erneut. Alle Anforderungen werden an den ersten Endpunkt weitergeleitet. Wenn dieser in Traffic Manager als fehlerhaft erkannt wird, wird für den Datenverkehr automatisch ein Failover auf den nächsten Endpunkt durchgeführt. 
7. Wenn Sie die Prioritätsreihenfolge eines Endpunkts ändern möchten, klicken Sie auf den Endpunkt und dann auf dem angezeigten Blatt **Endpunkt** auf **Bearbeiten**, und ändern Sie den Wert für **Priorität** nach Bedarf. 
8. Klicken Sie auf **Speichern**, um die geänderten Endpunkteinstellungen zu speichern.
9. Klicken Sie nach der Durchführung der Konfigurationsänderungen unten auf der Seite auf **Speichern** .
10. Testen Sie wie folgt die Änderungen in Ihrer Konfiguration:
    1.    Suchen Sie auf der Suchleiste des Portals nach dem Namen des Traffic Manager-Profils, und klicken Sie in den angezeigten Ergebnissen auf das Traffic Manager-Profil.
    2.    Klicken Sie auf dem Blatt **Traffic Manager-Profil** auf **Übersicht**.
    3.    Auf dem Blatt **Traffic Manager-Profil** wird der DNS-Name des neu erstellten Traffic Manager-Profils angezeigt. Dieser kann von beliebigen Clients (etwa durch Navigation in einem Webbrowser) für das Routing zum richtigen Endpunkt nach Maßgabe des Routingtyps verwendet werden. In diesem Fall werden alle Anforderungen an den ersten Endpunkt weitergeleitet. Wenn dieser in Traffic Manager als fehlerhaft erkannt wird, wird für den Datenverkehr automatisch ein Failover auf den nächsten Endpunkt durchgeführt.
11. Sobald das Traffic Manager-Profil funktionsfähig ist, bearbeiten Sie den DNS-Eintrag auf dem autoritativen DNS-Server, damit Ihre Unternehmensdomäne auf den Namen der Traffic Manager-Domäne verweisen kann.

![Konfigurieren der prioritätsbasierten Routingmethode für Datenverkehr mit Traffic Manager][1]

## <a name="next-steps"></a>Nächste Schritte


- Informationen zur [gewichteten Routingmethode für Datenverkehr](traffic-manager-configure-weighted-routing-method.md).
- Informationen zur [leistungsorientierten Routingmethode](traffic-manager-configure-performance-routing-method.md).
- Informationen zur [geografischen Routingmethode](traffic-manager-configure-geographic-routing-method.md).
- Informationen zum [Testen von Traffic Manager-Einstellungen](traffic-manager-testing-settings.md).

<!--Image references-->
[1]: ./media/traffic-manager-priority-routing-method/traffic-manager-priority-routing-method.png

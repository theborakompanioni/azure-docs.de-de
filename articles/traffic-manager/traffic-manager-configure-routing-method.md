---
title: "Konfigurieren von Routingmethoden für Traffic Manager| Microsoft-Dokumente"
description: Dieser Artikel beschreibt, wie Sie verschiedene Routingmethoden in Traffic Manager konfigurieren
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
ms.date: 10/18/2016
ms.author: kumud
translationtype: Human Translation
ms.sourcegitcommit: 8827793d771a2982a3dccb5d5d1674af0cd472ce
ms.openlocfilehash: d4481115dbf8cc720019096969c55509f33d250c

---

# <a name="configure-traffic-manager-routing-methods"></a>Konfigurieren von Traffic Manager-Routingmethoden

Azure Traffic Manager stellt drei Routingmethoden bereit, die steuern, wie Verkehr an verfügbare Dienstendpunkte weitergeleitet wird. Die Methode für das Datenverkehrsrouting wird auf jede empfangene DNS-Abfrage angewendet, um zu bestimmen, welcher Endpunkt in der DNS-Antwort zurückgegeben werden soll.

Drei Methoden für das Datenverkehrsrouting sind in Traffic Manager verfügbar:

* **Priorität**: Wählen Sie „Priorität“ aus, wenn Sie einen primären Dienstendpunkt verwenden möchten. Stellen Sie Backups bereit, falls der primäre Endpunkt nicht verfügbar ist.
* **Gewichtet** : Wählen Sie „Gewichtet“, wenn Sie Datenverkehr über eine Gruppe von Endpunkten verteilen möchten – gleichmäßig oder gemäß einer von Ihnen definieren Gewichtung.
* **Leistung** : Wählen Sie „Leistung“ aus, wenn sich Ihre Endpunkte an unterschiedlichen geografischen Standorten befinden und Endbenutzer an den „nächstgelegenen“ Endpunkt (im Hinblick auf die geringste Netzwerklatenz) weitergeleitet werden sollen.

## <a name="configure-priority-routing-method"></a>Konfigurieren der vorrangigen Routingmethode

Unabhängig vom Websitemodus stellen Azure Websites bereits Failoverfunktionen für Websites in einem Rechenzentrum (auch als „Region“ bezeichnet) zur Verfügung. Traffic Manager bietet ein Failover für Websites in verschiedenen Rechenzentren.

Ein häufiges Muster für Dienst-Failover ist das Senden von Datenverkehr an den primären Dienst und das Bereitstellen von identischen Sicherungsdiensten für das Failover. Die folgenden Schritte erläutern die Konfiguration dieses priorisierten Failovers mit Azure Cloud Services und Websites:

1. Klicken Sie im klassischen Azure-Portal im linken Bereich auf das Symbol **Traffic Manager** , um den Bereich „Traffic Manager“ zu öffnen.
2. Suchen Sie im klassischen Azure-Portal im Bereich „Traffic Manager“ nach dem Traffic Manager-Profil, das die zu ändernden Einstellungen enthält. Klicken Sie zum Öffnen der Seite mit den Profileinstellungen auf den Pfeil rechts vom Profilnamen.
3. Klicken Sie auf Ihrer Profilseite auf **Endpunkte** am oberen Rand der Seite. Stellen Sie sicher, dass die Clouddienste und Websites, die Sie in Ihrer Konfiguration einschließen möchten, vorhanden sind.
4. Klicken Sie am oberen Rand auf **Konfigurieren**, um die Konfigurationsseite zu öffnen.
5. Überprüfen Sie in den Einstellungen der **Routingmethode für Datenverkehr**, ob die Routingmethode für Datenverkehr **Failover** ist. Klicken Sie andernfalls in der Dropdownliste auf **Failover** .
6. Passen Sie bei **Failoverprioritätsliste**die Failoverreihenfolge für Ihre Endpunkte an. Wenn Sie die Routingmethode für Datenverkehr **Failover** auswählen, ist die Reihenfolge der ausgewählten Endpunkte von Bedeutung. Der primäre Endpunkt steht an erster Stelle. Verwenden Sie die nach oben und unten weisenden Pfeile, um die Reihenfolge nach Bedarf zu ändern. Informationen zum Festlegen der Failoverpriorität mithilfe von Windows PowerShell finden Sie unter [Set-AzureTrafficManagerProfile](http://go.microsoft.com/fwlink/p/?LinkId=400880).
7. Stellen Sie sicher, dass die **Überwachungseinstellungen** ordnungsgemäß konfiguriert sind. Durch die Überwachung wird sichergestellt, dass die Endpunkte, die offline sind, keinen Datenverkehr empfangen. Sie müssen einen Pfad und einen Dateinamen angeben, um Endpunkte zu überwachen. Ein Schrägstrich „/“ ist ein gültiger Eintrag für den relativen Pfad und bedeutet, dass sich die Datei im Stammverzeichnis (Standardwert) befindet.
8. Klicken Sie nach der Durchführung der Konfigurationsänderungen unten auf der Seite auf **Speichern** .
9. Testen Sie die Änderungen in Ihrer Konfiguration.
10. Sobald das Traffic Manager-Profil funktionsfähig ist, bearbeiten Sie den DNS-Eintrag auf dem autoritativen DNS-Server, damit Ihre Unternehmensdomäne auf den Namen der Traffic Manager-Domäne verweisen kann.

## <a name="configure-weighted-routing-method"></a>Konfigurieren einer gewichteten Routingmethode

Ein gängiges Muster für das Routing von Datenverkehr besteht darin, eine Reihe identischer Endpunkte (die Clouddienste und Websites umfassen) bereitzustellen und Datenverkehr im Roundrobin-Verfahren an die einzelnen Endpunkte zu senden. Die folgenden Schritte beschreiben, wie diese Routingmethodentyp für Datenverkehr konfiguriert wird.

> [!NOTE]
> Beachten Sie, dass Azure Websites bereits Roundrobin-Lastenausgleichsfunktionen für Websites in einem Rechenzentrum (auch als „Region“ bezeichnet) zur Verfügung stellt. Traffic Manager ermöglicht das Angeben von Routingmethoden für Roundrobin-Datenverkehr für Websites in verschiedenen Datencentern.

1. Klicken Sie im klassischen Azure-Portal im linken Bereich auf das Symbol **Traffic Manager** , um den Bereich „Traffic Manager“ zu öffnen.
2. Suchen Sie im Bereich „Traffic Manager“ nach dem Traffic Manager-Profil, das die zu ändernden Einstellungen enthält. Klicken Sie zum Öffnen der Seite mit den Profileinstellungen auf den Pfeil rechts vom Profilnamen.
3. Klicken Sie auf der Seite für Ihr Profil oben auf **Endpunkte** , und prüfen Sie, ob die Dienstendpunkte, die Sie einschließen möchten, in Ihrer Konfiguration vorhanden sind.
4. Klicken Sie auf der Seite Ihres Profils oben auf **Konfigurieren** , um die Konfigurationsseite zu öffnen.
5. Überprüfen Sie in den Einstellungen der **Routingmethode für Datenverkehr**, ob die Routingmethode für Datenverkehr **Roundrobin** ist. Klicken Sie andernfalls in der Dropdownliste auf **Roundrobin** .
6. Stellen Sie sicher, dass die **Überwachungseinstellungen** ordnungsgemäß konfiguriert sind. Durch die Überwachung wird sichergestellt, dass die Endpunkte, die offline sind, keinen Datenverkehr empfangen. Sie müssen einen Pfad und einen Dateinamen angeben, um Endpunkte zu überwachen. Ein Schrägstrich „/“ ist ein gültiger Eintrag für den relativen Pfad und bedeutet, dass sich die Datei im Stammverzeichnis (Standardwert) befindet.
7. Klicken Sie nach der Durchführung der Konfigurationsänderungen unten auf der Seite auf **Speichern** .
8. Testen Sie die Änderungen in Ihrer Konfiguration.
9. Sobald das Traffic Manager-Profil funktionsfähig ist, bearbeiten Sie den DNS-Eintrag auf dem autoritativen DNS-Server, damit Ihre Unternehmensdomäne auf den Namen der Traffic Manager-Domäne verweisen kann.

## <a name="configure-performance-traffic-routing-method"></a>Konfigurieren der Routingmethode für Leistungsdatenverkehr

Mit der Routingmethode für Leistungsdatenverkehr können Sie Datenverkehr zu den Endpunkten leiten, mit der niedrigsten Latenz des Client-Netzwerks. Normalerweise stellt das Rechenzentrum mit der geringsten Wartezeit die kürzeste geografische Distanz dar. Diese Routingmethode für Datenverkehr kann keine Änderungen in Echtzeit in der Netzwerkkonfiguration oder im Lastenausgleich berücksichtigen.

1. Klicken Sie im klassischen Azure-Portal im linken Bereich auf das Symbol **Traffic Manager** , um den Bereich „Traffic Manager“ zu öffnen.
2. Suchen Sie im Bereich „Traffic Manager“ nach dem Traffic Manager-Profil, das die zu ändernden Einstellungen enthält. Klicken Sie zum Öffnen der Seite mit den Profileinstellungen auf den Pfeil rechts vom Profilnamen.
3. Klicken Sie auf der Seite für Ihr Profil oben auf **Endpunkte** , und prüfen Sie, ob die Dienstendpunkte, die Sie einschließen möchten, in Ihrer Konfiguration vorhanden sind.
4. Klicken Sie auf der Seite Ihres Profils oben auf **Konfigurieren** , um die Konfigurationsseite zu öffnen.
5. Überprüfen Sie in den Einstellungen der **Routingmethode für Datenverkehr**, ob die Routingmethode für Datenverkehr **Leistung** ist. Klicken Sie andernfalls in der Dropdownliste auf **Leistung**.
6. Stellen Sie sicher, dass die **Überwachungseinstellungen** ordnungsgemäß konfiguriert sind. Durch die Überwachung wird sichergestellt, dass die Endpunkte, die offline sind, keinen Datenverkehr empfangen. Sie müssen einen Pfad und einen Dateinamen angeben, um Endpunkte zu überwachen. Ein Schrägstrich „/“ ist ein gültiger Eintrag für den relativen Pfad und bedeutet, dass sich die Datei im Stammverzeichnis (Standardwert) befindet.
7. Klicken Sie nach der Durchführung der Konfigurationsänderungen unten auf der Seite auf **Speichern** .
8. Testen Sie die Änderungen in Ihrer Konfiguration.
9. Sobald das Traffic Manager-Profil funktionsfähig ist, bearbeiten Sie den DNS-Eintrag auf dem autoritativen DNS-Server, damit Ihre Unternehmensdomäne auf den Namen der Traffic Manager-Domäne verweisen kann.

## <a name="next-steps"></a>Nächste Schritte

* [Verwalten von Traffic Manager-Profilen](traffic-manager-manage-profiles.md)
* [Traffic Manager-Routingmethoden](traffic-manager-routing-methods.md)
* [Testen der Traffic Manager-Einstellungen](traffic-manager-testing-settings.md)
* [Verweisen einer Unternehmens-Internetdomäne auf eine Traffic Manager-Domäne](traffic-manager-point-internet-domain.md)
* [Hinzufügen von Traffic Manager-Endpunkten](traffic-manager-manage-endpoints.md)
* [Problembehandlung beim Status „Heruntergestuft“ in Traffic Manager](traffic-manager-troubleshooting-degraded.md)




<!--HONumber=Nov16_HO5-->



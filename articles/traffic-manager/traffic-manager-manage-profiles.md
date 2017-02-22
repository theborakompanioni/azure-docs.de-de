---
title: Verwalten von Azure Traffic Manager-Profilen | Microsoft Docs
description: "In diesem Artikel wird beschrieben, wie Sie ein Azure Traffic Manager-Profil erstellen, deaktivieren, aktivieren, löschen und seinen Verlauf anzeigen."
services: traffic-manager
documentationcenter: 
author: kumudd
manager: timlt
editor: 
ms.assetid: f06e0365-0a20-4d08-b7e1-e56025e64f66
ms.service: traffic-manager
ms.devlang: na
ms.topic: hero-article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/11/2016
ms.author: kumud
translationtype: Human Translation
ms.sourcegitcommit: 8827793d771a2982a3dccb5d5d1674af0cd472ce
ms.openlocfilehash: 7e7de7dc1eca6903403afef03fdd6afb98ff16c9

---

# <a name="manage-an-azure-traffic-manager-profile"></a>Verwalten von Azure Traffic Manager-Profilen

Für Traffic Manager-Profile werden Datenverkehr-Routingmethoden verwendet, um die Verteilung von Datenverkehr auf Ihre Clouddienste oder Websiteendpunkte zu steuern. In diesem Artikel wird beschrieben, wie Sie diese Profile erstellen und verwalten.

## <a name="create-a-traffic-manager-profile-using-quick-create"></a>Erstellen von Traffic Manager-Profilen mithilfe der "Schnellerfassung"

Sie können sehr schnell ein Traffic Manager-Profil erstellen, indem Sie im klassischen Azure-Portal die „Schnellerfassung“ verwenden. Mit der Schnellerfassung können Sie Profile mit grundlegenden Konfigurationseinstellungen erstellen. Allerdings können Sie die Schnellerfassung nicht für Einstellungen wie die Gruppe von Endpunkten (Clouddienste und Websites), die Failoverreihenfolge für die Routingmethode für Failoverdatenverkehr oder die Überwachungseinstellungen verwenden. Nach dem Erstellen Ihres Profils können Sie diese Einstellungen im klassischen Azure-Portal konfigurieren. Traffic Manager unterstützt bis zu 200 Endpunkte pro Profil. Für die meisten Verwendungsszenarien sind aber nur einige Endpunkte erforderlich.

### <a name="to-create-a-traffic-manager-profile"></a>So erstellen Sie ein Traffic Manager-Profil

1. **Stellen Sie Ihre Clouddienste und Websites in der Produktionsumgebung bereit.** Weitere Informationen zu Clouddiensten finden Sie unter [Clouddienste](http://go.microsoft.com/fwlink/p/?LinkId=314074). Weitere Informationen zu Websites finden Sie unter [Websites](http://go.microsoft.com/fwlink/p/?LinkId=393327).
2. Melden Sie sich beim **klassischen Azure-Portal** an. Klicken Sie links unten im Portal auf **Neu**. Klicken Sie auf **Netzwerkdienste > Traffic Manager** und dann auf **Schnellerfassung**, um mit der Konfiguration des Profils zu beginnen.
3. **Konfigurieren Sie das DNS-Präfix.**  Geben Sie Ihre Traffic Manager-Profilen einen eindeutigen DNS-Präfixnamen. Sie können nur das Präfix für einen Traffic Manager-Domänennamen angeben.
4. **Wählen Sie das Abonnement.**  Wählen Sie das entsprechende Azure-Abonnement. Jedes Profil ist einem einzelnen Abonnement zugeordnet. Wenn Sie nur über ein Abonnement verfügen, wird diese Option nicht angezeigt.
5. **Wählen Sie die Routingmethode für Datenverkehr aus.** Wählen Sie die Routingmethode für Datenverkehr in der **Routingrichtlinie für Datenverkehr** aus. Weitere Informationen zu Routingmethoden für Datenverkehr finden Sie unter [Informationen zu Traffic Manager-Routingmethoden für Datenverkehr](traffic-manager-routing-methods.md).
6. **Klicken Sie auf „Erstellen“, um das Profil zu erstellen**. Nach Abschluss der Profilkonfiguration finden Sie Ihr Profil im klassischen Azure-Portal im Bereich „Traffic Manager“.
7. **Konfigurieren Sie Endpunkte, Überwachung und weitere Einstellungen im klassischen Azure-Portal**. Bei Verwendung der Schnellerfassung werden nur grundlegende Einstellungen konfiguriert. Es müssen zusätzliche Einstellungen konfiguriert werden, z.B. die Liste mit den Endpunkten und die Failoverreihenfolge von Endpunkten.

## <a name="disable-enable-or-delete-a-profile"></a>Deaktivieren, Aktivieren oder Löschen von Profilen

Sie können ein vorhandenes Profil deaktivieren, damit Benutzeranforderungen von Traffic Manager nicht an die konfigurierten Endpunkte geleitet werden. Wenn Sie ein Traffic Manager-Profil deaktivieren, bleiben das Profil und die Informationen im Profil erhalten und können auf der Traffic Manager-Benutzeroberfläche bearbeitet werden.  Die Weiterleitungen werden fortgesetzt, wenn Sie das Profil wieder aktivieren. Wenn Sie ein Traffic Manager-Profil im klassischen Azure-Portal erstellen, wird es automatisch aktiviert. Falls ein Profil nicht mehr erforderlich ist, können Sie es löschen.

### <a name="to-disable-a-profile"></a>So deaktivieren Sie ein Profil

1. Ändern Sie bei Verwendung eines benutzerdefinierten Domänennamens den CNAME-Eintrag auf Ihrem Internet-DNS-Server, damit er nicht mehr auf Ihr Traffic Manager-Profil verweist.
2. Der Datenverkehr wird nicht mehr über die Traffic Manager-Profileinstellungen an die Endpunkte weitergeleitet.
3. Wählen Sie das Profil aus, das Sie deaktivieren möchten. Markieren Sie das Profil auf der Seite „Traffic Manager“ durch Klicken auf die Spalte neben dem Profilnamen. Beachten Sie Folgendes: Wenn Sie auf den Namen des Profils oder auf den Pfeil neben dem Namen klicken, wird die Seite mit den Einstellungen für das Profil geöffnet.
4. Klicken Sie nach Auswahl des Profils am unteren Rand der Seite auf **Deaktivieren** .

### <a name="to-enable-a-profile"></a>So aktivieren Sie ein Profil

1. Wählen Sie das Profil aus, das Sie deaktivieren möchten. Markieren Sie das Profil auf der Seite „Traffic Manager“ durch Klicken auf die Spalte neben dem Profilnamen. Beachten Sie Folgendes: Wenn Sie auf den Namen des Profils oder auf den Pfeil neben dem Namen klicken, wird die Seite mit den Einstellungen für das Profil geöffnet.
2. Klicken Sie nach Auswahl des Profils am unteren Rand der Seite auf **Aktivieren** .
3. Wenn Sie einen benutzerdefinierten Domänennamen verwenden, sollten Sie einen CNAME-Ressourceneintrag auf Ihrem Internet-DNS-Server erstellen, der auf den Domänennamen Ihres Traffic Manager-Profils verweist.
4. Der Datenverkehr wird wieder an die Endpunkte geleitet.

### <a name="to-delete-a-profile"></a>So löschen Sie ein Profil

1. Ändern Sie den DNS-Ressourceneintrag auf dem Internet-DNS-Server so, dass er nicht mehr einen CNAME-Ressourceneintrag verwendet, der auf den Domänennamen Ihres Traffic Manager-Profils verweist.
2. Wählen Sie das Profil aus, das Sie deaktivieren möchten. Markieren Sie das Profil auf der Seite „Traffic Manager“ durch Klicken auf die Spalte neben dem Profilnamen. Beachten Sie Folgendes: Wenn Sie auf den Namen des Profils oder auf den Pfeil neben dem Namen klicken, wird die Seite mit den Einstellungen für das Profil geöffnet.
3. Klicken Sie nach Auswahl des Profils am unteren Rand der Seite auf **Löschen** .

## <a name="view-traffic-manager-profile-change-history"></a>Anzeigen des Änderungsverlaufs für das Traffic Manager-Profil

Sie können den Änderungsverlauf für Ihr Traffic Manager-Profil im klassischen Azure-Portal unter „Verwaltungsdienste“ anzeigen.

### <a name="to-view-your-traffic-manager-change-history"></a>So zeigen Sie den Traffic Manager-Änderungsverlauf an

1. Klicken Sie im linken Bereich des klassischen Azure-Portals auf **Verwaltungsdienste**.
2. Klicken Sie auf der Seite "Verwaltungsdienste" auf **Vorgangsprotokolle**.
3. Auf der Seite "Vorgangsprotokolle" können Sie Filter verwenden, um den Änderungsverlauf für Ihr Traffic Manager-Profil anzuzeigen. Nachdem Sie die Filteroptionen ausgewählt haben, klicken Sie auf das Häkchen, um die Ergebnisse anzuzeigen.

   * Wenn Sie Profiländerungen für Ihre gesamten Profile anzeigen möchten, wählen Sie Ihr Abonnement und den gewünschten Zeitraum aus, und wählen Sie dann im Kontextmenü **Typ** die Option **Traffic Manager** aus.
   * Wenn Sie nach Profilnamen filtern möchten, geben Sie den Namen des Profils in das Feld **Dienstname** ein, oder wählen Sie ihn im Kontextmenü aus.
   * Wenn Sie Details für jede einzelne Änderung anzeigen möchten, wählen Sie die Zeile mit der Änderung aus, die Sie anzeigen möchten, und klicken Sie dann am unteren Rand der Seite auf **Details** . Im Fenster **Vorgangsdetails** können Sie die XML-Darstellung des API-Objekts anzeigen, das als Teil des Vorgangs erstellt oder aktualisiert wurde.

## <a name="next-steps"></a>Nächste Schritte

* [Hinzufügen eines Endpunkts](traffic-manager-endpoints.md)
* [Konfigurieren der Routingmethode „Failover“](traffic-manager-configure-failover-routing-method.md)
* [Konfigurieren der Routingmethode „Roundrobin“](traffic-manager-configure-round-robin-routing-method.md)
* [Konfigurieren der Routingmethode „Performance“](traffic-manager-configure-performance-routing-method.md)
* [Verweisen einer Unternehmensinternetdomäne auf eine Azure Traffic Manager-Domäne](traffic-manager-point-internet-domain.md)
* [Problembehandlung beim Status „Heruntergestuft“ in Azure Traffic Manager](traffic-manager-troubleshooting-degraded.md)



<!--HONumber=Feb17_HO3-->



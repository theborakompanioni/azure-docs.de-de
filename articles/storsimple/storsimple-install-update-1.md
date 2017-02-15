---
title: "Installieren von Update 1.2 auf dem StorSimple-Gerät | Microsoft Docs"
description: "Erfahren Sie, wie Sie Update 1.2 für die StorSimple 8000-Serie auf Ihrem StorSimple-Gerät der 8000-Serie installieren."
services: storsimple
documentationcenter: NA
author: alkohli
manager: carmonm
editor: 
ms.assetid: 7a513923-eb77-4078-b0ab-f8e90183796a
ms.service: storsimple
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: TBD
ms.date: 08/22/2016
ms.author: alkohli
translationtype: Human Translation
ms.sourcegitcommit: 219dcbfdca145bedb570eb9ef747ee00cc0342eb
ms.openlocfilehash: f16ae589c160338450a41857f88fe29763e1b9eb


---
# <a name="install-update-12-on-your-storsimple-device"></a>Installieren von Update 1.2 auf Ihrem StorSimple-Gerät
## <a name="overview"></a>Übersicht
In diesem Tutorial wird erläutert, wie Sie Update 1.2 auf einem StorSimple-Gerät installieren, auf dem eine Softwareversion vor Update 1 installiert ist. Es werden außerdem die zusätzlichen Schritte beschrieben, die für das Update erforderlich sind, wenn ein Gateway auf einer anderen Netzwerkschnittstelle als DATA 0 des StorSimple-Geräts konfiguriert ist.

Update 1.2 umfasst Gerätesoftwareupdates, LSI-Treiberupdates und Updates für die Datenträgerfirmware. Die Software- und LSI-Treiberupdates führen nicht zu einer Unterbrechung des Betriebs und können über das klassische Azure-Portal angewendet werden. Die Updates für die Datenträgerfirmware führen zu einer Betriebsunterbrechung und können nur über die Windows PowerShell-Schnittstelle des Geräts angewendet werden.

Abhängig davon, welche Version auf Ihrem Gerät ausgeführt wird, wird Update 1.2 angewendet. Prüfen Sie die Softwareversion Ihres Geräts, indem Sie zum Abschnitt **Auf einen Blick** im **Dashboard** für Ihr Gerät wechseln.

</br>

| Ausgeführte Softwareversion | Was geschieht im Portal? |
| --- | --- |
| Release – GA |Wenn Sie die Releaseversion (GA) ausführen, wenden Sie dieses Update nicht an. [Wenden Sie sich an den Microsoft Support](storsimple-contact-microsoft-support.md) , um Ihr Gerät zu aktualisieren. |
| Update 0.1 |Das Portal wendet Update 1.2 an. |
| Update 0.2 |Das Portal wendet Update 1.2 an. |
| Update 0.3 |Das Portal wendet Update 1.2 an. |
| Update 1 |Dieses Update ist nicht verfügbar. |
| Update 1.1 |Dieses Update ist nicht verfügbar. |

</br>

> [!IMPORTANT]
> * Update 1.2 wird möglicherweise nicht sofort angezeigt, da Updates in mehreren Phasen bereitgestellt werden. Suchen Sie in einigen Tagen erneut nach Updates, da dieses Update bald zur Verfügung steht.
> * Dieses Update umfasst einen Satz manueller und automatischer Vorabprüfungen, mit denen die Geräteintegrität in Bezug auf Hardwarestatus und Netzwerkkonnektivität ermittelt wird. Diese Vorabprüfungen werden nur ausgeführt, wenn Sie die Updates aus dem klassischen Azure-Portal ausführen.
> * Wir empfehlen, die Software- und Treiberupdates über das klassische Azure-Portal zu installieren. Sie sollten nur dann die Windows PowerShell-Schnittstelle des Geräts (zum Installieren der Updates) verwenden, wenn die Vorabprüfung für das Gateway im Portal fehlschlägt. Die Installation des Updates kann zwischen 5 und 10 Stunden dauern (Windows-Updates eingeschlossen). Wartungsmodusupdates müssen über die Windows PowerShell-Schnittstelle des Geräts ausgeführt werden. Da Updates im Wartungsmodus den Betrieb unterbrechen, führen sie zu einer Ausfallzeit für Ihr Gerät.
> 
> 

[!INCLUDE [storsimple-preparing-for-update](../../includes/storsimple-preparing-for-updates.md)]

## <a name="install-update-12-via-the-azure-classic-portal"></a>Installieren von Update 1.2 über das klassische Azure-Portal
Führen Sie die folgenden Schritte aus, um Ihr Gerät auf [Update 1.2](storsimple-update1-release-notes.md)zu aktualisieren. Führen Sie dieses Verfahren nur dann aus, wenn Sie ein Gateway auf der DATA 0-Netzwerkschnittstelle Ihres Geräts konfiguriert haben.

[!INCLUDE [storsimple-install-update2-via-portal](../../includes/storsimple-install-update2-via-portal.md)]

1. Stellen Sie sicher, dass auf Ihrem Gerät **Update 1.2 der StorSimple 8000-Serie (6.3.9600.17584)**ausgeführt wird. Das **Datum der letzten Aktualisierung** hat sich ebenfalls geändert. Sie werden auch feststellen, dass Wartungsmodus-Updates verfügbar sind. (Diese Meldung wird nach der Updateinstallation unter Umständen noch bis zu 24 Stunden lang angezeigt.)
   
   Wartungsmodus-Updates unterbrechen die Verwendung, führen zu Ausfallzeiten des Geräts und können nur über die Windows PowerShell-Schnittstelle Ihres Geräts angewendet werden.
   
   ![Seite "Wartung"](./media/storsimple-install-update-1/InstallUpdate12_10M.png "Maintenance page")
2. Laden Sie die Wartungsmodus-Updates herunter, indem Sie mithilfe der unter [Herunterladen von Hotfixes](#to-download-hotfixes) angegebenen Schritte nach KB3063416 suchen und es herunterladen, um Datenträger-Firmwareupdates zu installieren. (Die anderen Updates müssten inzwischen bereits installiert sein.)
3. Führen Sie die Schritte unter [Installieren und Überprüfen von Wartungsmodus-Hotfixes](#to-install-and-verify-maintenance-mode-hotfixes) aus, um die Wartungsmodus-Updates zu installieren.
4. Navigieren Sie im klassischen Azure-Portal zur Seite **Wartung**, und klicken Sie am unteren Seitenrand auf **Updates prüfen**, um nach Windows-Updates zu suchen. Klicken Sie anschließend auf **Updates installieren**. Sie sind fertig, wenn alle Updates erfolgreich installiert wurden.

## <a name="install-update-12-on-a-device-that-has-a-gateway-configured-for-a-non-data-0-network-interface"></a>Installieren von Update 1.2 auf einem Gerät mit einem Gateway auf einer anderen Netzwerkschnittstelle als DATA 0
Sie sollten dieses Verfahren nur dann verwenden, wenn die Gatewayüberprüfung beim Versuch, die Updates über das klassische Azure-Portal zu installieren, fehlgeschlagen ist. Die Überprüfung zeigt einen Fehler, weil Sie ein Gateway auf einer anderen Netzwerkschnittstelle als DATA 0 zugewiesen haben und auf Ihrem Gerät eine Softwareversion vor Update 1 ausgeführt wird. Wenn Ihr Gerät kein Gateway aufweist, das auf eine andere Netzwerkschnittstelle als DATA 0 eingestellt ist, können Sie das Gerät direkt über das klassische Azure-Portal aktualisieren. Weitere Informationen finden Sie unter [Installieren von Update 1.2 über das klassische Azure-Portal](#install-update-1.2-via-the-azure-classic-portal).

Die Softwareversionen, die mithilfe dieser Methode aktualisiert werden können, sind Update 0.1, Update 0.2 und Update 0.3.

> [!IMPORTANT]
> * Wenn auf dem Gerät die Releaseversion (GA) ausgeführt wird, wenden Sie sich an den [Microsoft Support](storsimple-contact-microsoft-support.md) , um Hilfe bei diesem Update zu erhalten.
> * Dieses Verfahren muss nur einmal ausgeführt werden, um Update 1.2 anzuwenden. Über das klassischen Azure-Portal können Sie nachfolgende Updates installieren.
> 
> 

Wenn auf Ihrem Gerät eine Softwareversion vor Update 1 ausgeführt wird und ein Gateway für eine andere Netzwerkschnittstelle als DATA 0 eingerichtet wurde, haben Sie die folgenden Möglichkeiten zum Anwenden von Update 1.2:

* **Option 1**: Laden Sie das Update herunter, und wenden Sie es mithilfe des `Start-HcsHotfix`-Cmdlets über die Windows PowerShell-Schnittstelle des Geräts an. Dies ist die empfohlene Methode. **Verwenden Sie diese Methode nicht zum Anwenden von Update 1.2, wenn auf Ihrem Gerät Update 1.0 oder Update 1.1 ausgeführt wird.**
* **Option 2**: Entfernen Sie die Gatewaykonfiguration, und installieren Sie das Update direkt über das klassische Azure-Portal.

Ausführliche Anweisungen für jede dieser Optionen werden in den folgenden Abschnitten bereitgestellt.

## <a name="option-1-use-windows-powershell-for-storsimple-to-apply-update-12-as-a-hotfix"></a>Option 1: Verwenden von Windows PowerShell für StorSimple zum Anwenden von Update 1.2 als Hotfix
Sie sollten dieses Verfahren nur dann verwenden, wenn Sie Update 0.1, 0.2 oder 0.3 ausführen und die Gatewayüberprüfung beim Installieren der Updates über das klassische Azure-Portal fehlgeschlagen ist. Wenn Sie die Releaseversion (GA) der Software ausführen, wenden Sie sich zum Aktualisieren Ihres Geräts an den [Microsoft Support](storsimple-contact-microsoft-support.md) .

Wenn Sie Update 1.2 als Hotfix installieren möchten, müssen Sie folgende Hotfixes herunterladen und installieren:

| Reihenfolge | KB | Beschreibung | Updatetyp |
| --- | --- | --- | --- |
| 1 |KB3063418 |Softwareupdate |Normal |
| 2 |KB3043005 |LSI-SAS-Controller-Update |Normal |
| 3 |KB3063416 |Datenträgerfirmware |Wartung |

Stellen Sie Folgendes sicher, bevor Sie dieses Verfahren zum Anwenden des Updates verwenden:

* Beide Gerätecontroller sind online.

Führen Sie die folgenden Schritte aus, um Update 1.2 anzuwenden. **Die Installation des Updates nimmt etwa 2 Stunden in Anspruch (ca. 30 Minuten für die Software, 30 Minuten für die Treiber, 45 Minuten für die Datenträgerfirmware).**

[!INCLUDE [storsimple-install-update-option1](../../includes/storsimple-install-update-option1.md)]

## <a name="option-2-use-the-azure-classic-portal-to-apply-update-12-after-removing-the-gateway-configuration"></a>Option 2: Verwenden des klassischen Azure-Portals zum Anwenden von Update 1.2 nach dem Entfernen der Gatewaykonfiguration
Dieses Verfahren gilt nur für StorSimple-Geräte mit einer Softwareversion vor Update 1 und einem Gateway, das auf einer anderen Netzwerkschnittstelle als DATA 0 eingerichtet ist. Sie müssen die Gatewayeinstellung deaktivieren, bevor Sie das Update anwenden.

Das Update kann einige Stunden dauern. Wenn sich Ihre Hosts in unterschiedlichen Subnetzen befinden, kann das Entfernen der Gateway-Konfiguration auf den iSCSI-Schnittstellen zu Ausfallzeiten führen. Es wird empfohlen, DATA 0 für iSCSI-Datenverkehr zu konfigurieren, um Ausfallzeiten zu reduzieren.

Führen Sie die folgenden Schritte aus, um die Netzwerkschnittstelle mit dem Gateway zu deaktivieren und anschließend das Update anzuwenden.

[!INCLUDE [storsimple-install-update-option2](../../includes/storsimple-install-update-option2.md)]

[!INCLUDE [storsimple-install-troubleshooting](../../includes/storsimple-install-troubleshooting.md)]

## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen finden Sie unter [Versionshinweise zu Update 1.2](storsimple-update1-release-notes.md)




<!--HONumber=Nov16_HO3-->



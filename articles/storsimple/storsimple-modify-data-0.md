---
title: "Ändern der DATA 0-Einstellungen für ein StorSimple-Gerät | Microsoft Docs"
description: "Erfahren Sie, wie Sie die DATA 0-Netzwerkschnittstelle auf dem StorSimple-Gerät mithilfe von Windows PowerShell für StorSimple neu konfigurieren."
services: storsimple
documentationcenter: 
author: alkohli
manager: carmonm
editor: 
ms.assetid: 58e3d509-f425-4a7f-b650-d496a7c85193
ms.service: storsimple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/17/2016
ms.author: alkohli
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: 3a47ff1eed220cede820e8698c3384300e94688d


---
# <a name="modify-the-data-0-network-interface-settings-on-your-storsimple-device"></a>Ändern der DATA 0-Netzwerkschnittstelleneinstellungen auf dem StorSimple-Gerät
## <a name="overview"></a>Übersicht
Ihr Microsoft Azure StorSimple-Gerät hat sechs Netzwerkschnittstellen (DATA 0 bis DATA 5). Die DATA 0-Schnittstelle wird immer über die Windows PowerShell-Schnittstelle oder die serielle Konsole konfiguriert und ist automatisch für die Cloud aktiviert. Beachten Sie, dass Sie die DATA 0-Netzwerkschnittstelle nicht über das klassische Azure-Portal konfigurieren können. 

Die DATA 0-Schnittstelle wird zunächst über den Setup-Assistent während der ersten Bereitstellung des StorSimple-Geräts konfiguriert. Wenn sich das Gerät im Betriebsmodus befindet, müssen Sie die DATA 0-Einstellungen möglicherweise neu konfigurieren. In diesem Tutorial werden zwei Methoden zum Ändern der DATA 0-Netzwerkeinstellungen beschrieben. Beide Methoden werden über Windows PowerShell für StorSimple ausgeführt.

In diesem Tutorial lernen Sie Folgendes:

* Ändern von DATA 0-Netzwerkeinstellungen über den Setup-Assistenten
* Ändern von DATA 0-Netzwerkeinstellungen über das Cmdlet `Set-HcsNetInterface`

## <a name="modify-data-0-network-settings-through-setup-wizard"></a>Ändern von DATA 0-Netzwerkeinstellungen über den Setup-Assistenten
Sie können die DATA 0-Netzwerkeinstellungen neu konfigurieren, indem Sie eine Verbindung mit der Windows PowerShell-Schnittstelle Ihres StorSimple-Geräts herstellen und eine Sitzung des Setup-Assistenten starten. Führen Sie die folgenden Schritte aus, um DATA 0-Einstellungen zu ändern.

#### <a name="to-modify-data-0-network-settings-through-setup-wizard"></a>So ändern Sie die DATA 0-Netzwerkeinstellungen über den Setup-Assistenten
1. Wählen Sie im Menü der seriellen Konsole Option 1 aus, d.h. die **Anmeldung mit Vollzugriff**. Geben Sie das **Geräteadministratorkennwort** an, wenn Sie dazu aufgefordert werden. Das Standardkennwort lautet `Password1`.
2. Geben Sie an der Eingabeaufforderung Folgendes ein:
   
    `Invoke-HcsSetupWizard`
3. Ein Setup-Assistent wird angezeigt, mit dessen Hilfe sie die DATA 0-Schnittstelle Ihres Geräts konfigurieren können. Geben Sie neue Werte für die IP-Adresse, das Gateway und die Netzmaske an.

> [!NOTE]
> Die festen IP-Adressen des Controllers müssen auf der Seite **Konfigurieren** des StorSimple-Geräts im klassischen Azure-Portal neu konfiguriert werden. Weitere Informationen finden Sie unter [Ändern der Netzwerkschnittstelle](storsimple-modify-device-config.md#modify-network-interfaces).
> 
> 

## <a name="modify-data-0-network-settings-through-set-hcsnetinterface-cmdlet"></a>Ändern der DATA 0-Netzwerkeinstellungen über das Cmdlet „Set-HcsNetInterface“
Sie können die DATA 0-Netzwerkschnittstelle auch mithilfe des Cmdlets `Set-HcsNetInterface` neu konfigurieren. Das Cmdlet wird über die Windows PowerShell-Schnittstelle Ihres StorSimple-Geräts ausgeführt. Bei diesem Verfahren können auch die festen IPs des Controllers hier konfiguriert werden. Führen Sie die folgenden Schritte aus, um die DATA 0-Einstellungen zu ändern. 

#### <a name="to-modify-data-0-network-settings-through-the-set-hcsnetinterface-cmdlet"></a>So ändern Sie DATA 0-Netzwerkeinstellungen über das Cmdlet „Set-HcsNetInterface“
1. Wählen Sie im Menü der seriellen Konsole Option 1 aus, d.h. die **Anmeldung mit Vollzugriff**. Geben Sie das Geräteadministratorkennwort an, wenn Sie dazu aufgefordert werden. Das Standardkennwort lautet `Password1`.
2. Geben Sie an der Eingabeaufforderung Folgendes ein:
   
    `Set-HCSNetInterface -InterfaceAlias Data0 -IPv4Address <> -IPv4Netmask <> -IPv4Gateway <> -Controller0IPv4Address <> -Controller1IPv4Address <> -IsiScsiEnabled 1 -IsCloudEnabled 1`
   
    Geben Sie folgenden Werte für DATA 0 in spitzen Klammern (< >) an:
   
   * IPv4-Adresse
   * IPv4-Gateway
   * IPv4-Subnetzmaske
   * Feste IPv4-Adresse für Controller 0
   * Feste IPv4-Adresse für Controller 1
     
     Weitere Informationen zur Verwendung dieses Cmdlets finden Sie in der [Cmdlet-Referenz für Windows PowerShell für StorSimple](https://technet.microsoft.com/library/dn688161.aspx).

## <a name="next-steps"></a>Nächste Schritte
* Um Netzwerkschnittstellen nicht als DATA 0 zu konfigurieren, können Sie die [Seite „Konfigurieren“ im klassischen Azure-Portal](storsimple-modify-device-config.md)verwenden. 
* Wenn beim Konfigurieren der Netzwerkschnittstellen Probleme auftreten, lesen Sie die Informationen unter [Beheben von Bereitstellungsproblemen](storsimple-troubleshoot-deployment.md).




<!--HONumber=Nov16_HO3-->



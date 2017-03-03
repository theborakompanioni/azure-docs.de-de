---
title: Erstellen einer VM-Skalierungsgruppe mit dem Azure-Portal | Microsoft Docs
description: "Stellen Sie Skalierungsgruppen über das Azure-Portal bereit."
keywords: "Skalierungsgruppen für virtuelle Computer"
services: virtual-machine-scale-sets
documentationcenter: 
author: gatneil
manager: madhana
editor: tysonn
tags: azure-resource-manager
ms.assetid: 9c1583f0-bcc7-4b51-9d64-84da76de1fda
ms.service: virtual-machine-scale-sets
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm
ms.devlang: na
ms.topic: article
ms.date: 09/15/2016
ms.author: negat
translationtype: Human Translation
ms.sourcegitcommit: 35d4c8593dd9698017df85675395430f345f4e86
ms.openlocfilehash: 17054073e921708cc0c9392ed1b94e9579a9f940
ms.lasthandoff: 02/18/2017


---
# <a name="create-a-virtual-machine-scale-set-using-the-azure-portal"></a>Erstellen einer VM-Skalierungsgruppe mit dem Azure-Portal
In diesem Tutorial erfahren Sie, wie Sie in wenigen Minuten eine VM-Skalierungsgruppe über das Azure-Portal erstellen. Wenn Sie kein Azure-Abonnement besitzen, können Sie ein [kostenloses Konto](https://azure.microsoft.com/free/) erstellen, bevor Sie beginnen.

## <a name="choose-the-vm-image-from-the-marketplace"></a>Auswählen des VM-Image aus dem Marketplace
Über das Portal können Sie ganz einfach eine Skalierungsgruppe mit CentOS-, CoreOS-, Debian-, openSUSE-, Red Hat Enterprise Linux-, SUSE Linux Enterprise Server-, Ubuntu Server- oder Windows Server-Images bereitstellen.

Navigieren Sie zunächst in einem Browser zum [Azure-Portal](https://portal.azure.com) . Klicken Sie auf `New`, suchen Sie nach `scale set`, und wählen Sie dann den Eintrag `Virtual machine scale set` aus:

![ScaleSetPortalOverview](./media/virtual-machine-scale-sets-portal-create/ScaleSetPortalOverview.PNG)

## <a name="create-the-scale-set"></a>Erstellen der Skalierungsgruppe
Nun können Sie die Standardeinstellungen verwenden und die Skalierungsgruppe schnell erstellen.

* Geben Sie auf dem Blatt `Basics` einen Namen für die Skalierungsgruppe ein. Dieser Name dient als Grundlage für den FQDN des Load Balancers vor der Skalierungsgruppe und muss daher im gesamten Azure-System eindeutig sein.
* Wählen Sie den gewünschten Betriebssystemtyp aus, geben Sie den gewünschten Benutzernamen ein, und wählen Sie den bevorzugten Authentifizierungstyp aus. Falls Sie ein Kennwort auswählen, muss es mindestens 12 Zeichen lang sein und drei der folgenden vier Komplexitätsanforderungen erfüllen: ein Kleinbuchstabe, ein Großbuchstabe, eine Zahl und ein Sonderzeichen. Weitere Informationen finden Sie unter den [Anforderungen für Benutzernamen und Kennwörter](../virtual-machines/virtual-machines-windows-faq.md#what-are-the-username-requirements-when-creating-a-vm). Wenn Sie `SSH public key`auswählen, fügen Sie nur Ihren öffentlichen Schlüssel (NICHT Ihren privaten Schlüssel) ein:

![ScaleSetPortalBasics](./media/virtual-machine-scale-sets-portal-create/ScaleSetPortalBasics.PNG)

* Wählen Sie aus, ob die Skalierungsgruppe auf eine einzelne Platzierungsgruppe beschränkt sein soll oder ob sie mehrere Platzierungsgruppen umfassen soll. Wenn Sie festlegen, dass die Skalierungsgruppe mehrere Platzierungsgruppen umfassen kann, ermöglicht dies Skalierungsgruppen mit einem Volumen von über 100 virtuellen Computern (bis zu 1.000) mit bestimmten Einschränkungen. Weitere Informationen finden Sie in [dieser Dokumentation](./virtual-machine-scale-sets-placement-groups.md).
* Geben Sie den gewünschten Namen und Speicherort für Ihre Ressourcengruppe ein, und klicken Sie anschließend auf `OK`.
* Geben Sie auf dem Blatt `Virtual machine scale set service settings` die gewünschte Domänennamenbezeichnung ein. (Diese fungiert als Grundlage für den FQDN des Load Balancers vor der Skalierungsgruppe.) Die Bezeichnung muss im gesamten Azure-System eindeutig sein.
* Wählen Sie das gewünschte Image für den Betriebssystem-Datenträger, die Anzahl von Instanzen und die Computergröße aus.
* Wählen Sie den gewünschten Datenträgertyp aus: verwaltet oder nicht verwaltet. Weitere Informationen finden Sie in [dieser Dokumentation](./virtual-machine-scale-sets-managed-disks.md). Wenn Sie ausgewählt haben, dass die Skalierungsgruppe mehrere Platzierungsgruppen umfasst, ist diese Option nicht verfügbar, da für Skalierungsgruppen, die mehrere Platzierungsgruppen umfassen, ein verwalteter Datenträger erforderlich ist.
* Aktivieren oder deaktivieren Sie die automatische Skalierung, und konfigurieren Sie sie gegebenenfalls:

![ScaleSetPortalService](./media/virtual-machine-scale-sets-portal-create/ScaleSetPortalService.PNG)

* Klicken Sie nach Abschluss der Überprüfung auf dem Blatt `Summary` auf `OK`, um die Bereitstellung der Skalierungsgruppe zu starten.


## <a name="connect-to-a-vm-in-the-scale-set"></a>Herstellen einer Verbindung mit einem virtuellen Computer in der Skalierungsgruppe
Wenn Sie ausgewählt haben, dass sich die Skalierungsgruppe auf eine einzelne Platzierungsgruppe beschränkt, wird die Skalierungsgruppe mit NAT-Regeln bereitgestellt, die so konfiguriert sind, dass Sie einfach und schnell eine Verbindung mit der Skalierungsgruppe herstellen können.(Andernfalls müssen Sie wahrscheinlich im selben Netzwerk, in dem sich die Skalierungsgruppe befindet, eine Jumpbox erstellen, um eine Verbindung mit den virtuellen Computern in der Skalierungsgruppe herzustellen.) Um die virtuellen Computer anzuzeigen, navigieren Sie zur Registerkarte `Inbound NAT Rules` des Load Balancers für die Skalierungsgruppe:

![ScaleSetPortalNatRules](./media/virtual-machine-scale-sets-portal-create/ScaleSetPortalNatRules.PNG)

Mit diesen NAT-Regeln können Sie eine Verbindung mit einem beliebigen virtuellen Computer in der Skalierungsgruppe herstellen. Wenn also etwa für eine Windows-Skalierungsgruppe eine NAT-Regel für den eingehenden Port 50000 vorhanden ist, können Sie mit diesem Computer unter `<load-balancer-ip-address>:50000`eine RDP-Verbindung herstellen. Bei einer Linux-Skalierungsgruppe müsste für die Verbindungsherstellung der Befehl `ssh -p 50000 <username>@<load-balancer-ip-address>`verwendet werden.

## <a name="next-steps"></a>Nächste Schritte
In [dieser Dokumentation](virtual-machine-scale-sets-cli-quick-create.md)erfahren Sie, wie Sie Skalierungsgruppen mithilfe der Befehlszeilenschnittstelle bereitstellen.

In [dieser Dokumentation](virtual-machine-scale-sets-windows-create.md)erfahren Sie, wie Sie Skalierungsgruppen mithilfe von PowerShell bereitstellen.

In [dieser Dokumentation](virtual-machine-scale-sets-vs-create.md)erfahren Sie, wie Sie Skalierungsgruppen mithilfe von Visual Studio bereitstellen.

Eine allgemeine Dokumentation finden Sie in der [Dokumentationsübersicht für Skalierungsgruppen](virtual-machine-scale-sets-overview.md).

Allgemeine Informationen finden Sie auf der [Hauptseite für Skalierungsgruppen](https://azure.microsoft.com/services/virtual-machine-scale-sets/).



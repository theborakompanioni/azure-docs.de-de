---
title: "Mehrere IP-Adressen für virtuelle Azure-Computer: Portal | Microsoft-Dokumentation"
description: Informationen zum Zuweisen von mehreren IP-Adressen zu einem virtuellen Computer mithilfe des Azure-Portals | Resource Manager
services: virtual-network
documentationcenter: na
author: anavinahar
manager: narayan
editor: 
tags: azure-resource-manager
ms.assetid: 3a8cae97-3bed-430d-91b3-274696d91e34
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 11/30/2016
ms.author: annahar
translationtype: Human Translation
ms.sourcegitcommit: eeb56316b337c90cc83455be11917674eba898a3
ms.openlocfilehash: d264bd47d76db8015a64f09248c57c94572e2693
ms.lasthandoff: 04/03/2017


---
# <a name="assign-multiple-ip-addresses-to-virtual-machines-using-the-azure-portal"></a>Zuweisen von mehreren IP-Adressen zu virtuellen Computern mithilfe des Azure-Portals

>[!INCLUDE [virtual-network-multiple-ip-addresses-intro.md](../../includes/virtual-network-multiple-ip-addresses-intro.md)]
>
In diesem Artikel wird beschrieben, wie Sie über das Azure Resource Manager-Bereitstellungsmodell mithilfe des Azure-Portals einen virtuellen Computer erstellen. Ressourcen, die mit dem klassischen Bereitstellungsmodell erstellt wurden, können nicht mehrere IP-Adressen zugewiesen werden. Weitere Informationen zu den Azure-Bereitstellungsmodellen finden Sie im Artikel zum Thema [Understand deployment models (Bereitstellungsmodelle verstehen)](../resource-manager-deployment-model.md).

[!INCLUDE [virtual-network-multiple-ip-addresses-template-scenario.md](../../includes/virtual-network-multiple-ip-addresses-scenario.md)]

## <a name = "create"></a>Erstellen eines virtuellen Computers mit mehreren IP-Adressen

Wenn Sie einen virtuellen Computer mit mehreren IP-Adressen oder einer statischen privaten IP-Adresse erstellen möchten, müssen Sie ihn mithilfe von PowerShell oder der Azure-Befehlszeilenschnittstelle erstellen. Informationen zur Vorgehensweise erhalten Sie, indem Sie am Anfang dieses Artikels auf die Option „PowerShell“ oder „CLI“ klicken. Sie können einen virtuellen Computer mit einer einzelnen dynamischen privaten IP-Adresse und (optional) einer einzelnen öffentlichen IP-Adresse über das Portal erstellen. Führen Sie dazu die Schritte in den Artikeln [Erstellen eines virtuellen Windows-Computers](../virtual-machines/virtual-machines-windows-hero-tutorial.md) bzw. [Erstellen eines virtuellen Linux-Computers](../virtual-machines/linux/quick-create-portal.md) aus. Nach der Erstellung des virtuellen Computers können Sie über das Portal den IP-Adresstyp von dynamisch in statisch ändern und weitere IP-Adressen hinzufügen. Führen Sie dazu die folgenden Schritte im Abschnitt [Hinzufügen von IP-Adressen zu einem virtuellen Computer](#add) in diesem Artikel aus.

## <a name="add"></a>Hinzufügen von IP-Adressen zu einem virtuellen Computer

Sie können einer NIC private und öffentliche IP-Adressen hinzufügen. Führen Sie dazu die folgenden Schritte aus. In den Beispielen in den folgenden Abschnitten wird davon ausgegangen, dass Sie bereits einen virtuellen Computer mit den im [Szenario](#Scenario) in diesem Artikel beschriebenen drei IP-Konfigurationen erstellt haben, dies ist jedoch nicht erforderlich.

### <a name="coreadd"></a>Grundlegende Schritte

1. Navigieren Sie zum Azure-Portal unter „https://portal.azure.com“, und melden Sie sich, falls erforderlich, an.
2. Klicken Sie im Portal auf **Weitere Dienste**, geben Sie in das Feld „Filter“ *virtuelle Computer* ein, und klicken Sie dann auf **Virtuelle Computer**.
3. Klicken Sie auf dem Blatt **VMs** Blatt auf die VM, der Sie IP-Adressen hinzufügen möchten. Klicken Sie auf dem angezeigten Blatt des virtuellen Computers auf **Netzwerkschnittstellen**, und wählen Sie die Netzwerkschnittstelle aus, der Sie die IP-Adressen hinzufügen möchten. Im Beispiel in der folgenden Abbildung wurde die NIC mit dem Namen *myNIC* vom virtuellen Computer mit dem Namen *MyVM* ausgewählt:

    ![Netzwerkschnittstelle](./media/virtual-network-multiple-ip-addresses-portal/figure1.png)

4. Klicken Sie auf dem Blatt, das für die ausgewählte NIC angezeigt wird, auf **IP-Konfigurationen**.

Führen Sie die Schritte in einem der folgenden Abschnitte aus, abhängig vom Typ der IP-Adresse, die Sie hinzufügen möchten.

### <a name="add-a-private-ip-address"></a>**Hinzufügen einer privaten IP-Adresse**

Führen Sie die folgenden Schritte aus, um eine neue private IP-Adresse hinzuzufügen:

1. Führen Sie die Schritte im Abschnitt [Grundlegende Schritte](#coreadd) in diesem Artikel aus.
2. Klicken Sie auf **Hinzufügen**. Erstellen Sie in dem angezeigten Blatt **IP-Konfiguration hinzufügen** eine IP-Konfiguration mit dem Namen *IPConfig-4* mit *10.0.0.7* als *statische* private IP-Adresse, und klicken Sie dann auf **OK**.

    > [!NOTE]
    > Wenn Sie eine statische IP-Adresse hinzufügen möchten, müssen Sie eine nicht verwendete, gültige Adresse im Subnetz angeben, mit dem die NIC verbunden ist. Wenn die ausgewählte Adresse nicht verfügbar ist, wird im Portal ein „X“ für die IP-Adresse angezeigt, und Sie müssen eine andere Adresse auswählen.

3. Nachdem Sie auf „OK“ geklickt haben, wird das Blatt geschlossen und die neue IP-Konfiguration aufgelistet. Klicken Sie auf **OK**, um das Blatt **IP-Konfiguration hinzufügen** zu schließen.
4. Klicken Sie auf **Hinzufügen** um weitere IP-Konfigurationen hinzuzufügen, oder schließen Sie alle geöffneten Blätter, um das Hinzufügen von IP-Adressen abzuschließen.
5. Fügen Sie die privaten IP-Adressen dem Betriebssystem des virtuellen Computers hinzu. Führen Sie dazu die Schritte für Ihr Betriebssystem im Abschnitt [Hinzufügen von IP-Adressen zu einem VM-Betriebssystem](#os-config) in diesem Artikel aus.

### <a name="add-a-public-ip-address"></a>Hinzufügen einer öffentlichen IP-Adresse

Eine öffentliche IP-Adresse wird hinzugefügt, indem eine öffentliche IP-Adressressource entweder einer neuen oder einer vorhandenen IP-Konfiguration zugeordnet wird.

> [!NOTE]
> Für öffentliche IP-Adressen fällt eine geringe Gebühr an. Weitere Informationen zu den Preisen finden Sie auf der Seite [Preise für IP-Adressen](https://azure.microsoft.com/pricing/details/ip-addresses) . Die Anzahl der öffentlichen IP-Adressen, die in einem Abonnement verwendet werden können, ist beschränkt. Weitere Informationen über die Einschränkungen finden Sie im Artikel zu den [Azure-Einschränkungen](../azure-subscription-service-limits.md#networking-limits) .
> 

### <a name="create-public-ip"></a>Erstellen einer öffentlichen IP-Adressressource

Eine öffentliche IP-Adresse ist eine Einstellung für eine öffentliche IP-Adressressource. Wenn Sie über eine öffentliche IP-Adressressource verfügen, die aktuell keiner IP-Konfiguration zugeordnet ist, und die Sie einer IP-Konfiguration zuordnen möchten, überspringen Sie die folgenden Schritte, und führen Sie die geeigneten Schritte in einem der nachfolgenden Abschnitte aus. Wenn Sie über keine öffentliche IP-Adressressource verfügen, führen Sie die folgenden Schritte aus, um eine zu erstellen:

1. Navigieren Sie zum Azure-Portal unter „https://portal.azure.com“, und melden Sie sich, falls erforderlich, an.
3. Klicken Sie im Portal auf **Neu** > **Netzwerke** > **Öffentliche IP-Adresse**.
4. Geben Sie im angezeigten Blatt **Öffentliche IP-Adresse erstellen** einen **Namen** ein, wählen Sie einen **Zuweisungstyp für die IP-Adresse**, ein **Abonnement**, eine **Ressourcengruppe** und einen **Speicherort** aus, und klicken Sie dann auf **Erstellen**, wie in der folgenden Abbildung gezeigt:

    ![Erstellen einer öffentlichen IP-Adressressource](./media/virtual-network-multiple-ip-addresses-portal/figure5.png)

5. Führen Sie die Schritte in einem der folgenden Abschnitte aus, um die öffentliche IP-Adressressource einer IP-Konfiguration zuzuordnen.

#### <a name="associate-the-public-ip-address-resource-to-a-new-ip-configuration"></a>Zuordnen der öffentlichen IP-Adressressource zu einer neuen IP-Konfiguration

1. Führen Sie die Schritte im Abschnitt [Grundlegende Schritte](#coreadd) in diesem Artikel aus.
2. Klicken Sie auf **Hinzufügen**. Erstellen Sie in dem angezeigten Blatt **IP-Konfiguration hinzufügen** eine IP-Konfiguration mit dem Namen *IPConfig-4*. Aktivieren Sie die **öffentliche IP-Adresse**, und wählen Sie im angezeigten Blatt **Öffentliche IP-Adresse wählen** eine vorhandene, verfügbare öffentliche IP-Adressressource aus.

    Nachdem Sie die öffentliche IP-Adressressource ausgewählt haben, klicken Sie auf **OK**, um das Blatt zu schließen. Wenn Sie über keine vorhandene öffentliche IP-Adresse verfügen, können Sie mithilfe der Schritte im Abschnitt [Erstellen einer öffentlichen IP-Adressressource](#create-public-ip) in diesem Artikel eine Adresse erstellen. 

3. Überprüfen Sie die neue IP-Konfiguration. Selbst wenn Sie nicht explizit eine private IP-Adresse zugewiesen haben, wurde der IP-Konfiguration automatisch eine private IP-Adresse zugewiesen, da alle IP-Konfigurationen eine private IP-Adresse aufweisen müssen.
4. Klicken Sie auf **Hinzufügen** um weitere IP-Konfigurationen hinzuzufügen, oder schließen Sie alle geöffneten Blätter, um das Hinzufügen von IP-Adressen abzuschließen.
5. Fügen Sie die private IP-Adresse dem Betriebssystem des virtuellen Computers hinzu. Führen Sie dazu die Schritte für Ihr Betriebssystem im Abschnitt [Hinzufügen von IP-Adressen zu einem VM-Betriebssystem](#os-config) in diesem Artikel aus. Fügen Sie dem Betriebssystem nicht die öffentliche IP-Adresse hinzu.

#### <a name="associate-the-public-ip-address-resource-to-an-existing-ip-configuration"></a>Zuordnen der öffentlichen IP-Adressressource zu einer vorhandenen IP-Konfiguration

1. Führen Sie die Schritte im Abschnitt [Grundlegende Schritte](#coreadd) in diesem Artikel aus.
2. Klicken Sie auf die IP-Konfiguration, der Sie die öffentliche IP-Adressressource hinzufügen möchten.
3. Klicken Sie auf dem angezeigten Blatt „IPConfig“ auf **IP-Adresse**.
4. Wählen Sie auf dem angezeigten Blatt **Öffentliche IP-Adresse wählen** eine öffentliche IP-Adresse aus.
5. Klicken Sie auf **Speichern**. Die Blätter werden dann geschlossen. Wenn Sie über keine vorhandene öffentliche IP-Adresse verfügen, können Sie mithilfe der Schritte im Abschnitt [Erstellen einer öffentlichen IP-Adressressource](#create-public-ip) in diesem Artikel eine Adresse erstellen.
3. Überprüfen Sie die neue IP-Konfiguration.
4. Klicken Sie auf **Hinzufügen** um weitere IP-Konfigurationen hinzuzufügen, oder schließen Sie alle geöffneten Blätter, um das Hinzufügen von IP-Adressen abzuschließen. Fügen Sie dem Betriebssystem nicht die öffentliche IP-Adresse hinzu.


[!INCLUDE [virtual-network-multiple-ip-addresses-os-config.md](../../includes/virtual-network-multiple-ip-addresses-os-config.md)]


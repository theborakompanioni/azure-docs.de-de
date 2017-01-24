---
title: Verwalten von Netzwerksicherheitsgruppen (NSGs) mithilfe des Azure-Portals | Microsoft Docs
description: Erfahren Sie, wie Sie vorhandene Netzwerksicherheitsgruppen (NSGs) mithilfe des Azure-Portals verwalten.
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: 
tags: azure-resource-manager
ms.assetid: 5d55679d-57da-457c-97dc-1e1973909ee5
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/14/2016
ms.author: jdial
translationtype: Human Translation
ms.sourcegitcommit: 1fe845d442c7010580d4592f205e92e8ef70e34a
ms.openlocfilehash: e9bcf8a893ff209337f6a5763b631a22f8514e20


---
# <a name="manage-nsgs-using-the-portal"></a>Erstellen von Netzwerksicherheitsgruppen (NSGs) mithilfe des Portals

> [!div class="op_single_selector"]
> * [Portal](virtual-network-manage-nsg-arm-portal.md)
> * [PowerShell](virtual-network-manage-nsg-arm-ps.md)
> * [Azure-CLI](virtual-network-manage-nsg-arm-cli.md)
>

[!INCLUDE [virtual-network-manage-nsg-intro-include.md](../../includes/virtual-network-manage-nsg-intro-include.md)]

> [!NOTE]
> Azure verfügt über zwei verschiedene Bereitstellungsmodelle für das Erstellen und Verwenden von Ressourcen: [Resource Manager-Bereitstellung und klassische Bereitstellung](../resource-manager-deployment-model.md). Dieser Artikel befasst sich mit dem Resource Manager-Bereitstellungsmodell, das von Microsoft für die meisten neuen Bereitstellungen anstatt des klassischen Bereitstellungsmodells empfohlen wird.
>

[!INCLUDE [virtual-network-manage-nsg-arm-scenario-include.md](../../includes/virtual-network-manage-nsg-arm-scenario-include.md)]

## <a name="retrieve-information"></a>Abrufen von Informationen
Sie können Ihre vorhandenen NSGs sehen, Regeln für eine vorhandene NSG abrufen und herausfinden, welchen Ressourcen eine NSG zugeordnet ist.

### <a name="view-existing-nsgs"></a>Anzeigen lassen vorhandener NSGs

Führen Sie die folgenden Schritte aus, um alle vorhandenen NSGs in einem Abonnement anzuzeigen:

1. Navigieren Sie in einem Browser zu http://portal.azure.com, und melden Sie sich, falls erforderlich, mit Ihrem Azure-Konto an.

2. Klicken Sie auf **Durchsuchen >** > **Netzwerksicherheitsgruppen**.

    ![Azure-Portal – NSGs](./media/virtual-network-manage-nsg-arm-portal/figure1.png)

3. Überprüfen Sie die Liste der Netzwerksicherheitsgruppen auf dem Blatt **Netzwerksicherheitsgruppen** .

    ![Azure-Portal – NSGs](./media/virtual-network-manage-nsg-arm-portal/figure2.png)

### <a name="view-nsgs-in-a-resource-group"></a>Anzeigen der NSGs in einer Ressourcengruppe

Führen Sie die folgenden Schritte aus, um die Liste der NSGs in der Ressourcengruppe **RG-NSG** anzuzeigen:

1. Klicken Sie auf **Ressourcengruppen >** > **RG-NSG** > **...**.

    ![Azure-Portal – NSGs](./media/virtual-network-manage-nsg-arm-portal/figure3.png)

2. Suchen Sie in der Liste der Ressourcen nach Elementen, die das NSG-Symbol anzeigen, wie auf dem Blatt **Ressourcen** unten gezeigt.

    ![Azure-Portal – NSGs](./media/virtual-network-manage-nsg-arm-portal/figure4.png)

### <a name="list-all-rules-for-an-nsg"></a>Auflisten aller Regeln für eine NSG

Führen Sie die folgenden Schritte aus, um die Regeln für eine NSG namens **NSG-FrontEnd** anzuzeigen:

1. Klicken Sie auf dem Blatt **Netzwerksicherheitsgruppen** oder auf dem oben gezeigten Blatt **Ressourcen** auf **NSG-FrontEnd**.

2. Klicken Sie auf der Registerkarte **Einstellungen** auf **Eingangssicherheitsregeln**.

    ![Azure-Portal – NSGs](./media/virtual-network-manage-nsg-arm-portal/figure5.png)

3. Das Blatt **Eingangssicherheitsregeln** wird wie unten dargestellt angezeigt.

    ![Azure-Portal – NSGs](./media/virtual-network-manage-nsg-arm-portal/figure6.png)

4. Klicken Sie auf der Registerkarte **Einstellungen** auf **Ausgangssicherheitsregeln**, um diese anzuzeigen.

    > [!NOTE]
    > Um Standardregeln anzuzeigen, klicken Sie auf das Symbol **Standardregeln** am oberen Rand des Blatts, auf dem die Regeln angezeigt werden.
    >

### <a name="view-nsgs-associations"></a>Anzeigen von NSG-Zuordnungen

Führen Sie die folgenden Schritte aus, um anzuzeigen, welchen Ressourcen die NSG **NSG-FrontEnd** zugeordnet ist:

1. Klicken Sie auf dem Blatt **Netzwerksicherheitsgruppen** oder auf dem oben gezeigten Blatt **Ressourcen** auf **NSG-FrontEnd**.

2. Klicken Sie auf der Registerkarte **Einstellungen** auf **Subnetze**, um anzuzeigen, welche Subnetze der NSG zugeordnet sind.

    ![Azure-Portal – NSGs](./media/virtual-network-manage-nsg-arm-portal/figure7.png)

3. Klicken Sie auf der Registerkarte **Einstellungen** auf **Netzwerkschnittstellen**, um anzuzeigen, welche NICs der NSG zugeordnet sind.

## <a name="manage-rules"></a>Verwalten von Regeln
Sie können einer vorhandenen NSG Regeln hinzufügen, vorhandene Regeln bearbeiten und Regeln entfernen.

### <a name="add-a-rule"></a>Hinzufügen einer Regel
Um eine Regel hinzuzufügen, die **eingehenden** Verkehr an Port **443** von jedem Computer aus an die **NSG-FrontEnd**-NSG zulässt, führen Sie die folgenden Schritte aus:

1. Klicken Sie auf dem Blatt **Netzwerksicherheitsgruppen** oder auf dem oben gezeigten Blatt **Ressourcen** auf **NSG-FrontEnd**.
2. Klicken Sie auf der Registerkarte **Einstellungen** auf **Eingangssicherheitsregeln**.
3. Klicken Sie auf dem Blatt **Eingangssicherheitsregeln** auf **Hinzufügen**. Tragen Sie dann auf dem Blatt **Eingangssicherheitsregel hinzufügen** die Werte wie unten dargestellt ein, und klicken Sie anschließend auf **OK**.

    ![Azure-Portal – NSGs](./media/virtual-network-manage-nsg-arm-portal/figure8.png)

    Nach wenigen Sekunden sehen Sie, dass die neue Regel auf dem Blatt **Eingangssicherheitsregel** angezeigt wird.

    ![Azure-Portal – NSGs](./media/virtual-network-manage-nsg-arm-portal/figure9.png)

### <a name="change-a-rule"></a>Ändern einer Regel
Führen Sie die folgenden Schritte aus, um die oben erstellte Regel zu ändern, sodass eingehender Verkehr nur aus dem **Internet** zugelassen wird:

1. Klicken Sie auf dem Blatt **Netzwerksicherheitsgruppen** oder auf dem oben gezeigten Blatt **Ressourcen** auf **NSG-FrontEnd**.
2. Klicken Sie auf der Registerkarte **Einstellungen** auf die oben erstellte Regel.
3. Ändern Sie auf dem Blatt **allow-https** die Eigenschaft **Quelle** wie unten dargestellt, und klicken Sie anschließend auf **Speichern**.

    ![Azure-Portal – NSGs](./media/virtual-network-manage-nsg-arm-portal/figure10.png)

### <a name="delete-a-rule"></a>Löschen einer Regel

Führen Sie die folgenden Schritte aus, um die oben erstellte Regel zu löschen:

1. Klicken Sie auf dem Blatt **Netzwerksicherheitsgruppen** oder auf dem oben gezeigten Blatt **Ressourcen** auf **NSG-FrontEnd**.
2. Klicken Sie auf der Registerkarte **Einstellungen** auf die oben erstellte Regel.
3. Klicken Sie auf dem Blatt **allow-https** auf **Löschen**, und klicken Sie anschließen auf **Ja**.

    ![Azure-Portal – NSGs](./media/virtual-network-manage-nsg-arm-portal/figure11.png)

## <a name="manage-associations"></a>Verwalten von Zuordnungen
Sie können eine NSG Subnetzen und NICs zuordnen. Sie können auch die Zuordnung einer NSG zu jeder beliebigen Ressource, der sie zugeordnet ist, aufheben.

### <a name="associate-an-nsg-to-a-nic"></a>Zuordnen einer NSG zu einer NIC
Führen Sie die folgenden Schritte aus, um die **NSG-FrontEnd**-NSG der **TestNICWeb1**-NIC zuzuordnen:

1. Klicken Sie auf dem Blatt **Netzwerksicherheitsgruppen** oder auf dem oben gezeigten Blatt **Ressourcen** auf **NSG-FrontEnd**.
2. Klicken Sie auf der Registerkarte **Einstellungen** auf **Netzwerkschnittstellen** > **Zuordnen** > **TestNICWeb1**.

    ![Azure-Portal – NSGs](./media/virtual-network-manage-nsg-arm-portal/figure12.png)

### <a name="dissociate-an-nsg-from-a-nic"></a>Aufheben der Zuordnung einer NSG zu einer NIC

Führen Sie die folgenden Schritte aus, um die Zuordnung der **NSG-FrontEnd**-NSG zur **TestNICWeb1**-NIC aufzuheben:

1. Klicken Sie im Azure-Portal auf **Ressourcengruppen** > **RG-NSG** > **...** > **TestNICWeb1**.

2. Klicken Sie auf dem Blatt **TestNICWeb1** auf **Sicherheit ändern...** > **Keine**.

    ![Azure-Portal – NSGs](./media/virtual-network-manage-nsg-arm-portal/figure13.png)

> [!NOTE]
> Auf diesem Blatt können Sie auch die NIC jeder beliebigen vorhandenen NSG zuordnen.
>

### <a name="dissociate-an-nsg-from-a-subnet"></a>Aufheben der Zuordnung einer NSG zu einem Subnetz

Führen Sie die folgenden Schritte aus, um die Zuordnung der **NSG-FrontEnd**-NSG zum **FrontEnd**-Subnetz aufzuheben:

1. Klicken Sie im Azure-Portal auf **Ressourcengruppen >** > **RG-NSG** > **...** > **TestVNet**.

2. Klicken Sie auf dem Blatt **Einstellungen** auf **Subnetze** > **FrontEnd** > **Netzwerksicherheitsgruppe** > **Keine**.

    ![Azure-Portal – NSGs](./media/virtual-network-manage-nsg-arm-portal/figure14.png)

3. Klicken Sie auf dem Blatt **FrontEnd** auf **Speichern**.

    ![Azure-Portal – NSGs](./media/virtual-network-manage-nsg-arm-portal/figure15.png)

### <a name="associate-an-nsg-to-a-subnet"></a>Zuordnen einer NSG zu einem Subnetz

Führen Sie die folgenden Schritte aus, um die **NSG-FrontEnd**-NSG erneut dem **FrontEnd**-Subnetz zuzuordnen:

1. Klicken Sie im Azure-Portal auf **Ressourcengruppen >** > **RG-NSG** > **...** > **TestVNet**.
2. Klicken Sie auf dem Blatt **Einstellungen** auf **Subnetze** > **FrontEnd** > **Netzwerksicherheitsgruppe** > **NSG-FrontEnd**.
3. Klicken Sie auf dem Blatt **FrontEnd** auf **Speichern**.

> [!NOTE]
> Sie können auch vom Blatt **Einstellungen** der NSG aus eine NSG einem Subnetz zuordnen.
>

## <a name="delete-an-nsg"></a>Löschen einer NSG
Sie können eine NSG nur löschen, wenn sie keiner Ressource zugeordnet ist. Führen Sie die folgenden Schritte durch, um eine NSG zu löschen:

1. Klicken Sie im Azure-Portal auf **Ressourcengruppen** > **RG-NSG** > **...** > **NSG-FrontEnd**.
2. Klicken Sie auf dem Blatt **Einstellungen** auf **Netzwerkschnittstellen**.
3. Wenn NICs aufgeführt sind, klicken Sie auf die NIC, und führen Sie Schritt 2 in [Aufheben der Zuordnung einer NSG zu einer NIC](#Dissociate-an-NSG-from-a-NIC)aus.
4. Wiederholen Sie Schritt 3 für jede NIC.
5. Klicken Sie auf dem Blatt **Einstellungen** auf **Subnetze**.
6. Wenn Subnetze aufgeführt sind, klicken Sie auf das Subnetz, und führen Sie die Schritte 2 und 3 in [Aufheben der Zuordnung einer NSG zu einem Subnetz](#Dissociate-an-NSG-from-a-subnet)aus.
7. Führen Sie einen Bildlauf nach links zum **NSG-FrontEnd**-Blatt aus, und klicken Sie anschließen auf **Löschen** > **Ja**.

    ![Azure-Portal – NSGs](./media/virtual-network-manage-nsg-arm-portal/figure16.png)

## <a name="next-steps"></a>Nächste Schritte
* [Aktivieren der Protokollierung](virtual-network-nsg-manage-log.md) für NSGs.



<!--HONumber=Jan17_HO1-->



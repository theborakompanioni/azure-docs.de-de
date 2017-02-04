---
title: "Mehrere IP-Adressen für virtuelle Computer – Portal | Microsoft Docs"
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
ms.sourcegitcommit: 7ce83952f0f16e01a4837ce2155571d223d7b551
ms.openlocfilehash: b1a2549e0f04dbc00a47a57ecc888ca36339c646


---
# <a name="assign-multiple-ip-addresses-to-virtual-machines-using-the-azure-portal"></a>Zuweisen von mehreren IP-Adressen zu virtuellen Computern mithilfe des Azure-Portals

> [!div class="op_single_selector"]
> * [Portal](virtual-network-multiple-ip-addresses-portal.md)
> * [PowerShell](virtual-network-multiple-ip-addresses-powershell.md)
> * [BEFEHLSZEILENSCHNITTSTELLE (CLI)](virtual-network-multiple-ip-addresses-cli.md)
>

Ein virtueller Azure-Computer (VM) verfügt über eine oder mehrere angefügte Netzwerkschnittstellen (NIC). Jede NIC kann eine oder mehrere zugewiesene statische und dynamische öffentliche oder private IP-Adressen besitzen. Durch das Zuweisen von mehreren IP-Adressen zu einem virtuellen Computer bestehen folgende Möglichkeiten:

* Hosten mehrerer Websites oder Dienste mit unterschiedlichen IP-Adressen und SSL-Zertifikaten auf einem einzelnen Server
* Übernehmen der Rolle als virtuelles Netzwerkgerät, z.B. als Firewall oder Load Balancer
* Fähigkeit zum Hinzufügen einer privaten IP-Adresse für eine der NICs zu einem Azure Load Balancer-Back-End-Pool hinzufügen. Bisher konnte nur die primäre IP-Adresse für die primäre NIC einem Back-End-Pool hinzugefügt werden. Weitere Informationen dazu, wie in Konfigurationen mit mehreren IPs ein Lastenausgleich durchgeführt werden kann, finden Sie im Artikel [Load balancing multiple IP configurations](../load-balancer/load-balancer-multiple-ip.md) (Lastenausgleich bei Konfigurationen mit mehreren IPs).

Jeder NIC, die an einen virtuellen Computer angefügt ist, ist mindestens eine IP-Konfiguration zugeordnet. Jeder Konfiguration wird eine statische oder dynamische private IP-Adresse zugewiesen. Jeder Konfiguration kann auch eine öffentliche IP-Adressressource zugeordnet sein. Einer öffentlichen IP-Adressressource wird entweder eine dynamische oder eine statische IP-Adresse zugewiesen. Weitere Informationen zu IP-Adressen in Azure finden Sie im Artikel [IP-Adressen in Azure](virtual-network-ip-addresses-overview-arm.md).

In diesem Artikel wird erläutert, wie mit dem Azure-Portal einem mit dem Azure Resource Manager-Bereitstellungsmodell erstellten virtuellen Computer mehrere IP-Adressen zugewiesen werden. Ressourcen, die mit dem klassischen Bereitstellungsmodell erstellt wurden, können nicht mehrere IP-Adressen zugewiesen werden. Weitere Informationen zu den Azure-Bereitstellungsmodellen finden Sie im Artikel zum Thema [Understand deployment models (Bereitstellungsmodelle verstehen)](../resource-manager-deployment-model.md).

[!INCLUDE [virtual-network-preview](../../includes/virtual-network-preview.md)]

## <a name="scenario"></a>Szenario
Ein virtueller Computer mit einer einzelnen NIC wird erstellt und mit einem virtuellen Netzwerk verbunden. Der virtuelle Computer benötigt drei verschiedene *private* IP-Adressen und zwei *öffentliche* IP-Adressen. Die IP-Adressen werden den folgenden IP-Konfigurationen zugewiesen:

* **IPConfig-1:** Weist eine *dynamische* private IP-Adresse (Standard) und eine *statische* öffentliche IP-Adresse zu.
* **IPConfig-2:** Weist eine *statische* private IP-Adresse und eine *statische* öffentliche IP-Adresse zu.
* **IPConfig-3:** Weist eine *dynamische* private IP-Adresse und keine öffentliche IP-Adresse zu.
  
    ![Mehrere IP-Adressen](./media/virtual-network-multiple-ip-addresses-powershell/OneNIC-3IP.png)

Die IP-Konfigurationen werden der NIC zugeordnet, wenn diese erstellt wird, und die NIC wird an den virtuellen Computer angefügt, wenn dieser erstellt wird. Die für das Szenario verwendeten IP-Adresstypen dienen lediglich zur Veranschaulichung. Sie können beliebige IP-Adressen und Zuweisungstypen zuweisen.

## <a name="a-name--createacreate-a-vm-with-multiple-ip-addresses"></a><a name = "create"></a>Erstellen eines virtuellen Computers mit mehreren IP-Adressen

Wenn Sie einen virtuellen Computer mit mehreren IP-Adressen erstellen möchten, müssen Sie ihn mithilfe von PowerShell oder der Azure-Befehlszeilenschnittstelle erstellen. Informationen zur Vorgehensweise erhalten Sie, indem Sie am Anfang dieses Artikels auf die Option „PowerShell“ oder „CLI“ klicken. Sie können einen virtuellen Computer mit einer einzelnen statischen privaten IP-Adresse und (optional) einer einzelnen öffentlichen IP-Adresse über das Portal erstellen. Führen Sie dazu die Schritte in den Artikeln [Erstellen eines virtuellen Windows-Computers](../virtual-machines/virtual-machines-windows-hero-tutorial.md) bzw. [Erstellen eines virtuellen Linux-Computers](../virtual-machines/virtual-machines-linux-quick-create-portal.md) aus. Nach der Erstellung des virtuellen Computers können Sie mithilfe des Portals die IP-Adresstypen ändern und weitere IP-Adressen hinzufügen. Führen Sie dazu die folgenden Schritte im Abschnitt [Hinzufügen von IP-Adressen zu einem virtuellen Computer](#add) in diesem Artikel aus.

## <a name="a-nameaddaadd-ip-addresses-to-a-vm"></a><a name="add"></a>Hinzufügen von IP-Adressen zu einem virtuellen Computer

Sie können einer NIC private und öffentliche IP-Adressen hinzufügen. Führen Sie dazu die folgenden Schritte aus. In den Beispielen in den folgenden Abschnitten wird davon ausgegangen, dass Sie bereits einen virtuellen Computer mit den im [Szenario](#Scenario) in diesem Artikel beschriebenen drei IP-Konfigurationen erstellt haben, dies ist jedoch nicht erforderlich.

> [!NOTE]
> Obwohl in diesem Artikel alle IP-Konfigurationen einer einzigen NIC zugewiesen werden, ist es auch möglich, jeder NIC in einem virtuellen Computer mehrere IP-Konfigurationen zuzuweisen. Wie Sie einen virtuellen Computer mit mehreren NICs erstellen, erfahren Sie im Artikel [Erstellen einer VM mit mehreren Netzwerkkarten (NICs) mithilfe von PowerShell](virtual-network-deploy-multinic-arm-ps.md).

### <a name="a-namecoreaddacore-steps"></a><a name="coreadd"></a>Grundlegende Schritte

1. Wenn Sie sich für die Vorschau registrieren möchten, senden Sie eine E-Mail an [Mehrere IPs](mailto:MultipleIPsPreview@microsoft.com?subject=Request%20to%20enable%20subscription%20%3csubscription%20id%3e), und geben Sie Ihre Abonnement-ID und die beabsichtigte Verwendung an. Versuchen Sie nicht, die verbleibenden Schritte auszuführen:
    - Führen Sie die verbleibenden Schritte erst aus, wenn Sie eine E-Mail-Benachrichtigung mit dem Hinweis erhalten haben, dass Sie für die Vorschauversion akzeptiert wurden.
    - Ohne den in der E-Mail enthaltenen Anweisungen zu folgen, erhalten Sie:
2. Navigieren Sie zum Azure-Portal unter „https://portal.azure.com“, und melden Sie sich, falls erforderlich, an.
3. Klicken Sie im Portal auf **Weitere Dienste**, geben Sie in das Feld „Filter“ *virtuelle Computer* ein, und klicken Sie dann auf **Virtuelle Computer**.
4. Klicken Sie auf dem Blatt **VMs** Blatt auf die VM, der Sie IP-Adressen hinzufügen möchten. Klicken Sie auf dem angezeigten Blatt des virtuellen Computers auf **Netzwerkschnittstellen**, und wählen Sie die Netzwerkschnittstelle aus, der Sie die IP-Adressen hinzufügen möchten. Im Beispiel in der folgenden Abbildung wurde die NIC mit dem Namen *myNIC* vom virtuellen Computer mit dem Namen *MyVM* ausgewählt:

    ![Netzwerkschnittstelle](./media/virtual-network-multiple-ip-addresses-portal/figure1.png)

5. Klicken Sie in dem für die ausgewählte Netzwerkkarte angezeigten Blatt auf **IP-Konfigurationen**, wie in der folgenden Abbildung gezeigt:

    ![IP-Konfigurationen](./media/virtual-network-multiple-ip-addresses-portal/figure2.png)

Führen Sie die Schritte in einem der folgenden Abschnitte aus, abhängig vom Typ der IP-Adresse, die Sie hinzufügen möchten.

### <a name="add-a-private-ip-address"></a>**Hinzufügen einer privaten IP-Adresse**

Führen Sie die folgenden Schritte aus, um eine neue private IP-Adresse hinzuzufügen:

1. Führen Sie die Schritte im Abschnitt [Grundlegende Schritte](#coreadd) in diesem Artikel aus.
2. Klicken Sie auf **Hinzufügen**. Erstellen Sie in dem angezeigten Blatt **IP-Konfiguration hinzufügen** eine IP-Konfiguration mit dem Namen *IPConfig-4* mit *10.0.0.7* als eine *statische* private IP-Adresse, und klicken Sie dann auf **OK**, wie in der folgenden Abbildung gezeigt:

    ![Private IP-Adresse hinzufügen](./media/virtual-network-multiple-ip-addresses-portal/figure3.png)

    > [!NOTE]
    > Wenn Sie eine statische IP-Adresse hinzufügen möchten, müssen Sie eine nicht verwendete, gültige Adresse im Subnetz angeben, mit dem die NIC verbunden ist. Wenn die ausgewählte Adresse nicht verfügbar ist, wird im Portal ein „X“ für die IP-Adresse angezeigt, und Sie müssen eine andere Adresse auswählen.

    Falls die private IP-Adresse lieber dynamisch zugeordnet werden soll, wählen Sie stattdessen *Dynamisch* als **Zuordnungsmethode** aus. Sie müssen dann keine IP-Adresse angeben.
3. Nachdem Sie auf „OK“ geklickt haben, wird das Blatt geschlossen und die neue IP-Konfiguration aufgelistet, wie in der folgenden Abbildung gezeigt:

    ![IP-Konfigurationen](./media/virtual-network-multiple-ip-addresses-portal/figure4.png)

    Klicken Sie auf **OK**, um das Blatt **IP-Konfiguration hinzufügen** zu schließen.
4. Klicken Sie auf **Hinzufügen** um weitere IP-Konfigurationen hinzuzufügen, oder schließen Sie alle geöffneten Blätter, um das Hinzufügen von IP-Adressen abzuschließen.
5. Fügen Sie die privaten IP-Adressen dem Betriebssystem des virtuellen Computers hinzu. Führen Sie dazu die Schritte für Ihr Betriebssystem im Abschnitt [Hinzufügen von IP-Adressen zu einem VM-Betriebssystem](#os-config) in diesem Artikel aus.

### <a name="add-a-public-ip-address"></a>Hinzufügen einer öffentlichen IP-Adresse

Eine öffentliche IP-Adresse wird hinzugefügt, indem eine öffentliche IP-Adressressource entweder einer neuen oder einer vorhandenen IP-Konfiguration zugeordnet wird.

> [!NOTE]
> Für öffentliche IP-Adressen fällt eine geringe Gebühr an. Weitere Informationen zu den Preisen finden Sie auf der Seite [Preise für IP-Adressen](https://azure.microsoft.com/pricing/details/ip-addresses) . Die Anzahl der öffentlichen IP-Adressen, die in einem Abonnement verwendet werden können, ist beschränkt. Weitere Informationen über die Einschränkungen finden Sie im Artikel zu den [Azure-Einschränkungen](../azure-subscription-service-limits.md#networking-limits) .
> 

### <a name="a-namecreate-public-ipacreate-a-public-ip-address-resource"></a><a name="create-public-ip"></a>Erstellen einer öffentlichen IP-Adressressource

Eine öffentliche IP-Adresse ist eine Einstellung für eine öffentliche IP-Adressressource. Wenn Sie über eine öffentliche IP-Adressressource verfügen, die aktuell keiner IP-Konfiguration zugeordnet ist, und die Sie einer IP-Konfiguration zuordnen möchten, überspringen Sie die folgenden Schritte, und führen Sie die geeigneten Schritte in einem der nachfolgenden Abschnitte aus. Wenn Sie über keine öffentliche IP-Adressressource verfügen, führen Sie die folgenden Schritte aus, um eine zu erstellen:

1. Navigieren Sie zum Azure-Portal unter „https://portal.azure.com“, und melden Sie sich, falls erforderlich, an.
3. Klicken Sie im Portal auf **Neu** > **Netzwerke** > **Öffentliche IP-Adresse**.
4. Geben Sie im angezeigten Blatt **Öffentliche IP-Adresse erstellen** einen **Namen** ein, wählen Sie einen **Zuweisungstyp für die IP-Adresse**, ein **Abonnement**, eine **Ressourcengruppe** und einen **Speicherort** aus, und klicken Sie dann auf **Erstellen**, wie in der folgenden Abbildung gezeigt:

    ![Erstellen einer öffentlichen IP-Adressressource](./media/virtual-network-multiple-ip-addresses-portal/figure5.png)

5. Führen Sie die Schritte in einem der folgenden Abschnitte aus, um die öffentliche IP-Adressressource einer IP-Konfiguration zuzuordnen.

#### <a name="associate-the-public-ip-address-resource-to-a-new-ip-configuration"></a>Zuordnen der öffentlichen IP-Adressressource zu einer neuen IP-Konfiguration

1. Führen Sie die Schritte im Abschnitt [Grundlegende Schritte](#coreadd) in diesem Artikel aus.
2. Klicken Sie auf **Hinzufügen**. Erstellen Sie in dem angezeigten Blatt **IP-Konfiguration hinzufügen** eine IP-Konfiguration mit dem Namen *IPConfig-4*. Aktivieren Sie die **öffentliche IP-Adresse**, und wählen Sie im angezeigten Blatt **Öffentliche IP-Adresse auswählen** eine vorhandene, verfügbare öffentliche IP-Adressressource aus, wie in der folgenden Abbildung gezeigt:

    ![Neue IP-Konfiguration](./media/virtual-network-multiple-ip-addresses-portal/figure6.png)

    Nachdem Sie die öffentliche IP-Adressressource ausgewählt haben, klicken Sie auf **OK**, um das Blatt zu schließen. Wenn Sie über keine vorhandene öffentliche IP-Adresse verfügen, können Sie mithilfe der Schritte im Abschnitt [Erstellen einer öffentlichen IP-Adressressource](#create-public-ip) in diesem Artikel eine Adresse erstellen. 

3. Überprüfen Sie die neue IP-Konfiguration, wie in der folgenden Abbildung gezeigt:

    ![IP-Konfigurationen](./media/virtual-network-multiple-ip-addresses-portal/figure7.png)

    > [!NOTE]
    > Selbst wenn Sie nicht explizit eine private IP-Adresse zugewiesen haben, wurde der IP-Konfiguration automatisch eine private IP-Adresse zugewiesen, da alle IP-Konfigurationen eine private IP-Adresse aufweisen müssen.
    >

4. Klicken Sie auf **Hinzufügen** um weitere IP-Konfigurationen hinzuzufügen, oder schließen Sie alle geöffneten Blätter, um das Hinzufügen von IP-Adressen abzuschließen.
5. Fügen Sie die private IP-Adresse dem Betriebssystem des virtuellen Computers hinzu. Führen Sie dazu die Schritte für Ihr Betriebssystem im Abschnitt [Hinzufügen von IP-Adressen zu einem VM-Betriebssystem](#os-config) in diesem Artikel aus. Fügen Sie dem Betriebssystem nicht die öffentliche IP-Adresse hinzu.

#### <a name="associate-the-public-ip-address-resource-to-an-existing-ip-configuration"></a>Zuordnen der öffentlichen IP-Adressressource zu einer vorhandenen IP-Konfiguration

1. Führen Sie die Schritte im Abschnitt [Grundlegende Schritte](#coreadd) in diesem Artikel aus.
2. Wählen Sie die IP-Konfiguration aus, der Sie die öffentliche IP-Adressressource zuordnen möchten, aktivieren Sie die öffentliche IP-Adresse, und wählen Sie eine vorhandene, verfügbare öffentliche IP-Adressressource aus. Im Beispiel in der folgenden Abbildung wurde die öffentliche IP-Adressressource *myPublicIp3* zu *IPConfig-3* zugeordnet.

    ![Vorhandene IP-Konfiguration](./media/virtual-network-multiple-ip-addresses-portal/figure8.png)

    Nachdem Sie die öffentliche IP-Adressressource ausgewählt haben, klicken Sie auf **Speichern**, um die Blätter zu schließen. Wenn Sie über keine vorhandene öffentliche IP-Adresse verfügen, können Sie mithilfe der Schritte im Abschnitt [Erstellen einer öffentlichen IP-Adressressource](#create-public-ip) in diesem Artikel eine Adresse erstellen.

3. Überprüfen Sie die neue IP-Konfiguration, wie in der folgenden Abbildung gezeigt:

    ![IP-Konfigurationen](./media/virtual-network-multiple-ip-addresses-portal/figure9.png)

4. Klicken Sie auf **Hinzufügen** um weitere IP-Konfigurationen hinzuzufügen, oder schließen Sie alle geöffneten Blätter, um das Hinzufügen von IP-Adressen abzuschließen. Fügen Sie dem Betriebssystem nicht die öffentliche IP-Adresse hinzu.


[!INCLUDE [virtual-network-multiple-ip-addresses-os-config.md](../../includes/virtual-network-multiple-ip-addresses-os-config.md)]



<!--HONumber=Dec16_HO2-->



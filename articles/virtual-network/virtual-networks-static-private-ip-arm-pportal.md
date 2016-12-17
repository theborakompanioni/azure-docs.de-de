---
title: 'Gewusst wie: Einrichten einer statischen privaten IP-Adresse im ARM-Modus mithilfe des Azure-Portals | Microsoft Docs'
description: Grundlegendes zu privaten IP-Adressen (DIPs) und zur Verwaltung dieser IP-Adressen im ARM-Modus mithilfe des Azure-Portals
services: virtual-network
documentationcenter: na
author: jimdial
manager: carmonm
editor: tysonn
tags: azure-resource-manager
ms.assetid: 11245645-357d-4358-9a14-dd78e367b494
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/04/2016
ms.author: jdial
translationtype: Human Translation
ms.sourcegitcommit: 5d56a0d2c9c44d37520c6a9a3db326f9bdc5420e
ms.openlocfilehash: e4f6d14f79536c58e40d3ac4b48d00a8c397f1b4


---
# <a name="how-to-set-a-static-private-ip-address-in-the-azure-portal"></a>Gewusst wie: Einrichten einer statischen privaten IP-Adresse im Azure-Portal

> [!div class="op_single_selector"]
- [Azure-Portal](virtual-networks-static-private-ip-arm-pportal.md)
- [PowerShell](virtual-networks-static-private-ip-arm-ps.md)
- [Azure-Befehlszeilenschnittstelle](virtual-networks-static-private-ip-arm-cli.md)
- [Azure-Portal (klassisch)](virtual-networks-static-private-ip-classic-pportal.md)
- [PowerShell (klassisch)](virtual-networks-static-private-ip-classic-ps.md)
- [Azure CLI (klassisch)](virtual-networks-static-private-ip-classic-cli.md)


[!INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Dieser Artikel gilt für das Ressourcen-Manager-Bereitstellungsmodell. Sie können [eine statische private IP-Adresse auch im klassischen Bereitstellungsmodell verwalten](virtual-networks-static-private-ip-classic-pportal.md).

[!INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

Die folgenden Schritte setzen voraus, dass bereits eine einfache Umgebung erstellt wurde. Wenn Sie die in diesem Dokument aufgeführten Schritte ohne Veränderungen ausführen möchten, erstellen Sie zunächst eine Testumgebung, wie unter [Erstellen eines VNets](virtual-networks-create-vnet-arm-pportal.md)beschrieben.

## <a name="how-to-create-a-vm-for-testing-static-private-ip-addresses"></a>Erstellen einer VM zum Testen statischer privater IP-Adressen
Im Azure-Portal ist es nicht möglich, beim Erstellen eines virtuellen Computers im Ressourcen-Manager-Bereitstellungsmodus eine statische private IP-Adresse festzulegen. Sie müssen den virtuellen Computer zuerst erstellen und dann eine statische private IP-Adresse festlegen.

Führen Sie die folgenden Schritte aus, um in einem VNET namens *TestVNet* im Subnetz *FrontEnd* einen virtuellen Computer mit dem Namen *DNS01* zu erstellen.

1. Navigieren Sie in einem Browser zu http://portal.azure.com, und melden Sie sich, falls erforderlich, mit Ihrem Azure-Konto an.
2. Klicken Sie auf **NEU** > **Compute** > **Windows Server 2012 R2 Datacenter**. Beachten Sie, dass in der Liste **Bereitstellungsmodell auswählen** bereits **Resource Manager** angezeigt wird. Klicken Sie anschließend auf **Erstellen**, wie in der folgenden Abbildung gezeigt.
   
    ![Erstellen einer VM im Azure-Portal](./media/virtual-networks-static-ip-arm-pportal/figure01.png)
3. Geben Sie auf dem Blatt **Grundlagen** den Namen des zu erstellenden virtuellen Computers (in diesem Szenario*DNS01* ), das lokale Administratorkonto und das Kennwort ein, wie in der folgenden Abbildung gezeigt.
   
    ![Blatt "Grundlagen"](./media/virtual-networks-static-ip-arm-pportal/figure02.png)
4. Stellen Sie sicher, dass unter **Speicherort** die Region *USA, Mitte* ausgewählt ist. Klicken Sie anschließend **unter Ressourcengruppe** auf **Vorhandene auswählen**, klicken Sie erneut auf **Ressourcengruppe**, klicken Sie auf *TestRG* und dann auf **OK**.
   
    ![Blatt "Grundlagen"](./media/virtual-networks-static-ip-arm-pportal/figure03.png)
5. Wählen Sie auf dem Blatt **Größe auswählen** die Option **A1 Standard**, und klicken Sie anschließend auf **Auswählen**.
   
    ![Blatt "Größe auswählen"](./media/virtual-networks-static-ip-arm-pportal/figure04.png)    
6. Überprüfen Sie auf dem Blatt **Einstellungen**, dass für die folgenden Eigenschaften die nachfolgenden Werte festgelegt sind, und klicken Sie anschließend auf **OK**.
   
    -**Speicherkonto**: *vnetstorage*
   
   * **Netzwerk**: *TestVNet*
   * **Subnetz**: *FrontEnd*
     
     ![Blatt "Größe auswählen"](./media/virtual-networks-static-ip-arm-pportal/figure05.png)     
7. Klicken Sie auf dem Blatt **Zusammenfassung** auf **OK**. Beachten Sie die Kachel, die im unteren Bereich des Dashboards angezeigt wird.
   
    ![Erstellen einer VM im Azure-Portal](./media/virtual-networks-static-ip-arm-pportal/figure06.png)

## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a>Abrufen der Informationen zur statischen privaten IP-Adresse für einen virtuellen Computer
Um die Informationen zur statischen privaten IP-Adresse für den virtuellen Computer anzuzeigen, der mit den obigen Schritten erstellt wurde, führen Sie die folgenden Schritte aus.

1. Klicken Sie im Azure-Portal auf **ALLE DURCHSUCHEN** > **Virtuelle Computer** > **DNS01All** > **Alle Einstellungen** > **Netzwerkschnittstellen**, und klicken Sie anschließend auf die einzige aufgeführte Netzwerkschnittstelle.
   
    ![Bereitstellen der VM-Kachel](./media/virtual-networks-static-ip-arm-pportal/figure07.png)
2. Klicken Sie auf dem Blatt **Netzwerkschnittstelle** auf **Alle Einstellungen** > **IP-Adressen**, und beachten Sie die Werte für **Zuweisung** und **IP-Adresse**.
   
    ![Bereitstellen der VM-Kachel](./media/virtual-networks-static-ip-arm-pportal/figure08.png)

## <a name="how-to-add-a-static-private-ip-address-to-an-existing-vm"></a>Hinzufügen einer statischen privaten IP-Adresse zu einem vorhandenen virtuellen Computer
Führen Sie die folgenden Schritte aus, um dem virtuellen Computer, der mit dem obigen Skript erstellt wurde, eine statische private IP-Adresse hinzuzufügen:

1. Klicken Sie auf dem oben gezeigten Blatt **IP-Adressen** unter **Zuweisung** auf **Statisch**.
2. Geben Sie im Feld *IP-Adresse* den Wert **192.168.1.101** ein, und klicken Sie anschließend auf **Speichern**.
   
    ![Erstellen einer VM im Azure-Portal](./media/virtual-networks-static-ip-arm-pportal/figure09.png)

> [!NOTE]
> Wenn Sie, nachdem Sie auf **Speichern** geklickt haben, feststellen, dass die Zuweisung immer noch den Wert **dynamisch** anzeigt, bedeutet dies, dass die eingegebene IP-Adresse bereits verwendet wird. Versuchen Sie, eine andere IP-Adresse zu verwenden.
> 
> 

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a>Entfernen einer statischen privaten IP-Adresse von einem virtuellen Computer
Um die statische private IP-Adresse für den zuvor erstellten virtuellen Computer zu entfernen, führen Sie den folgenden Schritt aus.

1. Klicken Sie auf dem oben gezeigten Blatt **IP-Adressen** unter **Zuweisung** auf **Dynamisch**, und klicken Sie anschließend auf **Speichern**.

## <a name="next-steps"></a>Nächste Schritte
* Erfahren Sie mehr über [reservierte öffentliche IP-Adressen](virtual-networks-reserved-public-ip.md) .
* Erfahren Sie mehr über [öffentliche IP-Adressen auf Instanzebene (ILPIP)](virtual-networks-instance-level-public-ip.md) .
* Lesen Sie die Informationen zu [REST-APIs für reservierte IP-Adressen](https://msdn.microsoft.com/library/azure/dn722420.aspx).




<!--HONumber=Nov16_HO3-->



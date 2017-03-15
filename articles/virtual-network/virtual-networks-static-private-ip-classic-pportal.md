---
title: "Konfigurieren von privaten IP-Adressen für virtuelle Computer (klassisch) – Azure-Portal | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie private IP-Adressen für virtuelle Computer (klassisch) über das Azure-Portal konfigurieren."
services: virtual-network
documentationcenter: na
author: jimdial
manager: timlt
editor: tysonn
tags: azure-service-management
ms.assetid: b8ef8367-58b2-42df-9f26-3269980950b8
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/04/2016
ms.author: jdial
ms.custom: H1Hack27Feb2017
translationtype: Human Translation
ms.sourcegitcommit: 63f2f6dde56c1b5c4b3ad2591700f43f6542874d
ms.openlocfilehash: bde6de3495c2909b63b1f85e420a4ff5e7ac2c1a
ms.lasthandoff: 02/28/2017


---
# <a name="configure-private-ip-addresses-for-a-virtual-machine-classic-using-the-azure-portal"></a>Konfigurieren von privaten IP-Adressen für einen virtuellen Computer (klassisch) über das Azure-Portal

[!INCLUDE [virtual-networks-static-private-ip-selectors-classic-include](../../includes/virtual-networks-static-private-ip-selectors-classic-include.md)]

[!INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Dieser Artikel gilt für das klassische Bereitstellungsmodell. Sie können [eine statische private IP-Adresse auch im Ressourcen-Manager-Bereitstellungsmodell verwalten](virtual-networks-static-private-ip-arm-pportal.md).

[!INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

Die folgenden Schritte setzen voraus, dass bereits eine einfache Umgebung erstellt wurde. Wenn Sie die in diesem Dokument aufgeführten Schritte ohne Veränderungen ausführen möchten, erstellen Sie zunächst eine Testumgebung, wie unter [Erstellen eines VNets](virtual-networks-create-vnet-classic-pportal.md)beschrieben.

## <a name="how-to-specify-a-static-private-ip-address-when-creating-a-vm"></a>Angeben einer statischen privaten IP-Adresse beim Erstellen eines virtuellen Computers
Erstellen Sie in einem VNet mit dem Namen *TestVNet* im Subnetz *FrontEnd* einen virtuellen Computer mit dem Namen *DNS01* und der statischen privaten IP-Adresse *192.168.1.101*. Gehen Sie dabei wie folgt vor:

1. Navigieren Sie in einem Browser zu http://portal.azure.com, und melden Sie sich, falls erforderlich, mit Ihrem Azure-Konto an.
2. Klicken Sie auf **NEU** > **Compute** > **Windows Server 2012 R2 Datacenter**. Beachten Sie, dass in der Liste **Bereitstellungsmodell auswählen** bereits **Klassisch** angezeigt wird. Klicken Sie anschließend auf **Erstellen**.
   
    ![Erstellen einer VM im Azure-Portal](./media/virtual-networks-static-ip-classic-pportal/figure01.png)
3. Geben Sie auf dem Blatt **VM erstellen** den Namen des zu erstellenden virtuellen Computers (in diesem Szenario*DNS01* ), das lokale Administratorkonto und das gewünschte Kennwort ein.
   
    ![Erstellen einer VM im Azure-Portal](./media/virtual-networks-static-ip-classic-pportal/figure02.png)
4. Klicken Sie auf **Optionale Konfiguration** > **Netzwerk** > **Virtual Network** und anschließend auf **TestVNet**. Wenn **TestVNet** nicht verfügbar ist, stellen Sie sicher, dass Sie den Speicherort *USA (Mitte)* verwenden und dass die am Anfang dieses Artikels beschriebene Testumgebung erstellt wurde.
   
    ![Erstellen einer VM im Azure-Portal](./media/virtual-networks-static-ip-classic-pportal/figure03.png)
5. Stellen Sie sicher, dass auf dem Blatt **Netzwerk** das Subnetz *FrontEnd* ausgewählt ist, und klicken Sie anschließend auf **IP-Adressen**. Klicken Sie unter **IP-Adresszuweisung** auf **Statisch**, und geben Sie dann im Feld **IP-Adressen** den Wert *192.168.1.101* ein, wie nachfolgend dargestellt.
   
    ![Erstellen einer VM im Azure-Portal](./media/virtual-networks-static-ip-classic-pportal/figure04.png)    
6. Klicken Sie auf dem Blatt **IP-Adressen** auf **OK**, klicken Sie auf dem Blatt **Netzwerk** auf **OK**, und klicken Sie dann auf dem Blatt **Optionale Konfiguration** auf **OK**.
7. Klicken Sie auf dem Blatt **VM erstellen** auf **Erstellen**. Beachten Sie die Kachel, die im unteren Bereich des Dashboards angezeigt wird.
   
    ![Erstellen einer VM im Azure-Portal](./media/virtual-networks-static-ip-classic-pportal/figure05.png)

## <a name="how-to-retrieve-static-private-ip-address-information-for-a-vm"></a>Abrufen der Informationen zur statischen privaten IP-Adresse für einen virtuellen Computer
Um die Informationen zur statischen privaten IP-Adresse für den virtuellen Computer anzuzeigen, der mit den obigen Schritten erstellt wurde, führen Sie die folgenden Schritte aus.

1. Klicken Sie im Azure-Portal auf **ALLE DURCHSUCHEN** > **Virtuelle Computer (Klassisch)** > **DNS01All** > **Alle Einstellungen** > **IP-Adressen**, um die Informationen zur IP-Adresszuweisung und IP-Adresse anzuzeigen, wie nachfolgend dargestellt.
   
    ![Erstellen einer VM im Azure-Portal](./media/virtual-networks-static-ip-classic-pportal/figure06.png)

## <a name="how-to-remove-a-static-private-ip-address-from-a-vm"></a>Entfernen einer statischen privaten IP-Adresse von einem virtuellen Computer
Um die statische private IP-Adresse für den zuvor erstellten virtuellen Computer zu entfernen, führen Sie die folgenden Schritte aus.

1. Klicken Sie auf dem oben gezeigten Blatt **IP-Adressen** rechts neben **IP-Adresszuweisung** auf **Dynamisch**, klicken Sie anschließend auf **Speichern** und klicken Sie dann auf **Ja**.
   
    ![Erstellen einer VM im Azure-Portal](./media/virtual-networks-static-ip-classic-pportal/figure07.png)

## <a name="how-to-add-a-static-private-ip-address-to-an-existing-vm"></a>Hinzufügen einer statischen privaten IP-Adresse zu einem vorhandenen virtuellen Computer
Führen Sie die folgenden Schritte aus, um dem virtuellen Computer, der mit dem obigen Skript erstellt wurde, eine statische private IP-Adresse hinzuzufügen:

1. Klicken Sie auf dem oben gezeigten Blatt **IP-Adressen** rechts neben **IP-Adresszuweisung** auf **Statisch**.
2. Geben Sie im Feld **IP-Adresse** den Wert *192.168.1.101* ein, klicken Sie anschließend auf **Speichern**, und klicken Sie dann auf **Ja**.

## <a name="next-steps"></a>Nächste Schritte
* Erfahren Sie mehr über [reservierte öffentliche IP-Adressen](virtual-networks-reserved-public-ip.md) .
* Erfahren Sie mehr über [öffentliche IP-Adressen auf Instanzebene (ILPIP)](virtual-networks-instance-level-public-ip.md) .
* Lesen Sie die Informationen zu [REST-APIs für reservierte IP-Adressen](https://msdn.microsoft.com/library/azure/dn722420.aspx).



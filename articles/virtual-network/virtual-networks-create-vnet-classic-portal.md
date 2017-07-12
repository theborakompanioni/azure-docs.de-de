---
title: "Erstellen eines virtuellen Azure-Netzwerks (klassisch) – klassisches Portal | Microsoft-Dokumentation"
description: Erfahren Sie, wie Sie ein virtuelles Netzwerk (klassisch) mithilfe einer Netcfg-Datei im klassischen Azure-Portal erstellen.
services: virtual-network
documentationcenter: 
author: jimdial
manager: timlt
editor: 
tags: azure-service-management
ms.assetid: 69894a0b-8050-451e-8a25-c513e1bd271e
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/30/2017
ms.author: jdial
ms.translationtype: Human Translation
ms.sourcegitcommit: d365f7137527d60eb509b4f431295de2218ea706
ms.openlocfilehash: 5ff91cccb711d61ed120e4a4e820d6a5dfc5e4a6
ms.contentlocale: de-de
ms.lasthandoff: 01/31/2017


---

<a id="create-a-virtual-network-classic-with-a-netcfg-file-using-the-azure-classic-portal" class="xliff"></a>

# Erstellen eines virtuellen Netzwerks (klassisch) mithilfe einer Netcfg-Datei im klassischen Azure-Portal
[!INCLUDE [virtual-networks-create-vnet-selectors-classic-include](../../includes/virtual-networks-create-vnet-selectors-classic-include.md)]

[!INCLUDE [virtual-networks-create-vnet-intro](../../includes/virtual-networks-create-vnet-intro-include.md)]

[!INCLUDE [azure-arm-classic-important-include](../../includes/azure-arm-classic-important-include.md)]

Dieser Artikel beschreibt, wie ein virtuelles Netzwerk mit einer Netcfg-Datei über das klassische Bereitstellungsmodell im klassischen Azure-Portal erstellt wird. Sie können auch [ohne Netcfg-Datei über das klassische Bereitstellungsmodell ein virtuelles Netzwerk erstellen](virtual-networks-create-vnet-classic-pportal.md) oder im Azure-Portal [über das Azure Resource Manager-Bereitstellungsmodell ein virtuelles Netzwerk erstellen](virtual-networks-create-vnet-arm-pportal.md).

[!INCLUDE [virtual-networks-create-vnet-scenario-include](../../includes/virtual-networks-create-vnet-scenario-include.md)]

<a id="how-to-create-a-vnet-with-a-network-config-file-in-the-microsoft-azure-classic-portal" class="xliff"></a>

## Erstellen eines VNet mit einer Netzwerkkonfigurationsdatei im klassischen Microsoft Azure-Portal
Azure verwendet eine XML-Datei, um alle für ein Abonnement verfügbaren VNets zu definieren. Sie können diese Datei herunterladen und bearbeiten, um VNets über das klassische Bereitstellungsmodell zu erstellen oder vorhandene VNets zu ändern oder zu löschen. In diesem Artikel wird erläutert, wie Sie diese als Netzwerkkonfigurationsdatei (oder Netcfg-Datei) bezeichnete Datei herunterladen, ihr ein VNet hinzufügen und die Datei zum Erstellen des VNet hochladen. Weitere Informationen zur Netzwerkkonfigurationsdatei finden Sie unter [Azure virtual network configuration schema](https://msdn.microsoft.com/library/azure/jj157100.aspx) (Konfigurationsschema für virtuelle Azure-Netzwerke).

Führen Sie zum Erstellen eines VNet unter Verwendung einer Netcfg-Datei im klassischen Azure-Portal die folgenden Schritte aus:

1. Navigieren Sie in einem Browser zu „http://manage.windowsazure.com“, und melden Sie sich, falls erforderlich, mit Ihrem Azure-Konto an.
2. Scrollen Sie in der Liste der Dienste nach unten, klicken Sie auf **NETZWERKE** und dann auf **EXPORTIEREN**, wie in der folgenden Abbildung gezeigt:

    ![Virtuelle Azure-Netzwerke](./media/virtual-networks-create-vnet-classic-portal/networks.png)
3. Wählen Sie im Dialogfeld **Netzwerkkonfiguration exportieren** das Abonnement aus, aus dem Sie die Konfiguration des virtuellen Netzwerks exportieren möchten, und klicken Sie dann auf das Häkchen in der unteren rechten Ecke des Felds.
4. Folgen Sie den Anweisungen des Browsers zum Speichern der Datei **NetworkConfig.xml** . Notieren Sie sich den Speicherort der Datei.
5. Öffnen Sie die in Schritt 4 gespeicherte Datei mit einem beliebigen XML- oder Text-Editor, und suchen Sie nach dem `<VirtualNetworkSites>`-Element im `<VirtualNetworkConfiguration>`-Element. Wenn Sie bereits über VNets verfügen, wird jedes in seinem eigenen `<VirtualNetworkSite>`-Element aufgeführt. Wenn die Datei kein `<VirtualNetworkSites>`-Element innerhalb des `<VirtualNetworkConfiguration>`-Elements enthält, erstellen Sie eines.
6. Wenn Ihre vorhandene NetworkConfig-Datei keine VNets enthält, wird Ihre NetworkConfig.xml-Datei dem folgenden Beispiel ähneln, nachdem Sie das in diesem Szenario beschriebene VNet hinzugefügt haben:

    ```xml
    <NetworkConfiguration xmlns="http://schemas.microsoft.com/ServiceHosting/2011/07/NetworkConfiguration">
      <VirtualNetworkConfiguration>
        <VirtualNetworkSites>
          <VirtualNetworkSite name="TestVNet" Location="Central US">
            <AddressSpace>
              <AddressPrefix>192.168.0.0/16</AddressPrefix>
            </AddressSpace>
            <Subnets>
              <Subnet name="FrontEnd">
                <AddressPrefix>192.168.1.0/24</AddressPrefix>
              </Subnet>
              <Subnet name="BackEnd">
                <AddressPrefix>192.168.2.0/24</AddressPrefix>
              </Subnet>
            </Subnets>
          </VirtualNetworkSite>
        </VirtualNetworkSites>
      </VirtualNetworkConfiguration>
    </NetworkConfiguration>
    ```
7. Speichern Sie die Netzwerkkonfigurationsdatei.
8. Klicken Sie im klassischen Azure-Portal auf **NEU**, **NETZWERKDIENSTE**, **VIRTUELLES NETZWERK** und **IMPORTKONFIGURATION**, wie in der folgenden Abbildung gezeigt:

    ![KONFIGURATION IMPORTIEREN](./media/virtual-networks-create-vnet-classic-portal/import.png)
10. Klicken Sie im Dialogfeld **Netzwerkkonfigurationsdatei importieren** auf **DATEI SUCHEN...**. Navigieren Sie zu dem Ordner, in dem Sie die Datei in Schritt 7 gespeichert haben, wählen Sie die Datei aus, und klicken Sie dann auf **Öffnen**, wie in der folgenden Abbildung gezeigt:

    ![Seite "Netzwerkkonfiguration importieren"](./media/virtual-networks-create-vnet-classic-portal/vnet-create-portal-netcfg-figure4.png)

    Klicken Sie in der unteren rechten Ecke des Felds auf die Pfeilschaltfläche, um mit dem nächsten Schritt fortzufahren.

9. Beachten Sie im Dialogfeld **Netzwerk erstellen** den Eintrag für das neue VNet, wie in der folgenden Abbildung dargestellt:

    ![Seite "Netzwerk erstellen"](./media/virtual-networks-create-vnet-classic-portal/vnet-create-portal-netcfg-figure5.png)
10. Um das VNet zu erstellen, klicken Sie auf das Häkchen in der unteren rechten Ecke des Felds in der vorherigen Abbildung. Nach einigen Sekunden wird das VNet in der Liste der verfügbaren VNets angezeigt, wie in der folgenden Abbildung dargestellt:

    ![Neues virtuelles Netzwerk](./media/virtual-networks-create-vnet-classic-portal/vnet-create-portal-netcfg-figure6.png)


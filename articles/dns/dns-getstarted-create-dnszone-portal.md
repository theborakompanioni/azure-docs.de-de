---
title: Erstellen und Verwalten einer DNS-Zone im Azure-Portal | Microsoft Docs
description: "Erfahren Sie, wie Sie DNS-Zonen in Azure DNS erstellen. Dies ist eine Schritt-für-Schritt-Anleitung zum Erstellen und Verwalten Ihrer ersten DNS-Zone über das Azure-Portal."
services: dns
documentationcenter: na
author: georgewallace
manager: timlt
editor: 
tags: azure-resource-manager
ms.assetid: f44c5ea1-4c85-469e-888e-5f5b34451664
ms.service: dns
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 12/05/2016
ms.author: gwallace
translationtype: Human Translation
ms.sourcegitcommit: 36fa9cd757b27347c08f80657bab8a06789a3c2f
ms.openlocfilehash: 7119ce18e04b427c268d4d8636b1a5fa02ed0cc3
ms.lasthandoff: 02/27/2017

---

# <a name="create-a-dns-zone-in-the-azure-portal"></a>Erstellen einer DNS-Zone im Azure-Portal

> [!div class="op_single_selector"]
> * [Azure-Portal](dns-getstarted-create-dnszone-portal.md)
> * [PowerShell](dns-getstarted-create-dnszone.md)
> * [Azure CLI 1.0](dns-getstarted-create-dnszone-cli-nodejs.md)
> * [Azure CLI 2.0](dns-getstarted-create-dnszone-cli.md)

Dieser Artikel führt Sie Schritt für Schritt durch die Erstellung einer DNS-Zone mithilfe des Azure-Portals. Sie können die DNS-Zone auch mithilfe von PowerShell oder der Befehlszeilenschnittstelle (CLI) erstellen.

[!INCLUDE [dns-create-zone-about](../../includes/dns-create-zone-about-include.md)]

## <a name="create-a-dns-zone"></a>Erstellen einer DNS-Zone

1. Melden Sie sich auf dem Azure-Portal an.
2. Klicken Sie im Hubmenü auf **Neu > Netzwerk** und anschließend auf **DNS-Zone**, um das Blatt „DNS-Zone erstellen“ zu öffnen.

    ![DNS-Zone](./media/dns-getstarted-create-dnszone-portal/openzone650.png)

4. Geben Sie Ihrer DNS-Zone auf dem Blatt **DNS-Zone erstellen** einen Namen. Beispiel: *contoso.com*.
 
    ![Erstellen einer Zone](./media/dns-getstarted-create-dnszone-portal/newzone250.png)

5. Geben Sie als nächstes die gewünschte Ressourcengruppe an. Sie können entweder eine neue Ressourcengruppe erstellen oder eine bereits vorhandene auswählen. Wenn Sie eine neue Ressourcengruppe erstellen möchten, verwenden Sie die Dropdownliste **Speicherort**, um den Speicherort der Ressourcengruppe anzugeben. Diese Einstellung bezieht sich auf den Speicherort der Ressourcengruppe und hat keine Auswirkung auf die DNS-Zone. Der Speicherort der DNS-Zone ist immer global und wird nicht angezeigt.

6. Sie können das Kontrollkästchen **An Dashboard anheften** aktiviert lassen, um Ihre neue Zone auf dem Dashboard leichter wiederzufinden. Klicken Sie dann auf **Erstellen**.

    ![An Dashboard anheften](./media/dns-getstarted-create-dnszone-portal/pindashboard150.png)

7. Nachdem Sie auf „Erstellen“ klicken, sehen Sie, wie Ihre neue Zone auf dem Dashboard konfiguriert wird.

    ![Wird erstellt](./media/dns-getstarted-create-dnszone-portal/creating150.png)

8. Wenn Ihre neue Zone erstellt wurde, wird das Blatt Ihrer neuen Zone auf dem Dashboard geöffnet.

## <a name="view-records"></a>Anzeigen von Datensätzen

Beim Erstellen einer DNS-Zone werden auch die folgenden Einträge erstellt:

* Der Eintrag „Autoritätsursprung“ (SOA). Der SOA ist im Stamm jeder DNS-Zone vorhanden.
* Die autoritativen Namenserver (NS)-Einträge. Diese zeigen, welche Namenserver die Zone hosten. Azure DNS verwendet einen Pool von Namenservern, sodass verschiedene Namenserver verschiedenen Zonen in Azure DNS zugewiesen werden können. Weitere Informationen finden Sie unter [Delegieren einer Domäne an Azure DNS](dns-domain-delegation.md) .

Im unteren Teil des Blatts „DNS-Zone“ finden Sie die Ressourceneintragssätze für die DNS-Zone.

![Zone](./media/dns-getstarted-create-dnszone-portal/viewzone500.png)

## <a name="test-name-servers"></a>Testen der Namenserver

Mithilfe von DNS-Tools wie nslookup oder dig oder mit dem [PowerShell-Cmdlet Resolve-DnsName](https://technet.microsoft.com/library/jj590781.aspx) können Sie testen, ob Ihre DNS-Zone auf den Azure DNS-Namenservern vorhanden ist.

Wenn Sie Ihre Domäne noch nicht delegiert haben, um die neue Zone in Azure DNS zu verwenden, müssen Sie die DNS-Abfrage direkt auf einen der Namenserver für die Zone leiten. Die Namenserver für Ihre Zone sind im Azure-Portal angegeben:
    
![Zone](./media/dns-getstarted-create-dnszone-portal/viewzonens500.png)

Verwenden Sie im folgenden Befehl den korrekten Namenserver für Ihre Zone.

    nslookup
    > set type=SOA
    > server ns1-01.azure-dns.com
    > contoso.com

    Server: ns1-01.azure-dns.com
    Address:  208.76.47.1

    contoso.com
            primary name server = ns1-01.azure-dns.com
            responsible mail addr = azuredns-hostmaster.microsoft.com
            serial  = 1
            refresh = 3600 (1 hour)
            retry   = 300 (5 mins)
            expire  = 2419200 (28 days)
            default TTL = 300 (5 mins)

## <a name="next-steps"></a>Nächste Schritte

Nach dem Erstellen einer DNS-Zone müssen Sie [Ressourceneintragssätze und Einträge erstellen](dns-getstarted-create-recordset-portal.md), um DNS-Einträge für Ihre Internetdomäne zu erstellen.


